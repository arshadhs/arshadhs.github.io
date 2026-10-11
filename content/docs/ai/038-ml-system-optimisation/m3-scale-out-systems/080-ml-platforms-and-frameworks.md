---
title: "ML Platforms and Frameworks"
draft: false
tags: ["ML System Optimisation", "MLSysOps", "Parameter Server", "Distributed SGD", "Communication", "Batch Size", "AI", "ML"]
categories: ["AI", "ML"]
weight: 800
menu: main
---

# ML Platforms and Frameworks

Training at scale requires more than dividing data between GPUs. A platform must decide **where model state lives, who computes gradients, how updates are combined, and how information moves between devices**.

This page focuses on the **Parameter Server model** and the practical constraints that determine whether adding resources makes training faster.

Topics covered:

- Parameter servers, workers, and independent scaling of storage and computation
- Training-state memory and parameter sharding
- Distributed loss, gradient aggregation, and the pull–compute–push cycle
- Communication costs, RDMA, MPI, PCIe, and NVLink
- Global batch size, learning-rate scaling, and warm-up
- Monitoring, fault recovery, and sparse versus dense workloads

## Learning Objectives

By the end of this page, you should be able to:

- distinguish parameter-server responsibilities from worker responsibilities
- estimate model-state memory without confusing it with weights alone
- write a consistent distributed SGD update
- calculate ideal compute scaling and gradient-transfer time
- explain why communication can erase the benefit of extra workers
- adjust batch size and learning rate together, while checking convergence
- choose an architecture using workload characteristics rather than GPU count alone

## Big Picture

A parameter server acts like a shared model-state service. Workers process different examples, calculate proposed changes, and send those changes to the servers that own the relevant parameters.

{{< mermaid >}}
flowchart TD
    W1["Worker 1: data shard A"] <-->|"Pull parameters; push gradients"| S1["Server 1: parameter shard 1"]
    W1 <-->|"Pull parameters; push gradients"| S2["Server 2: parameter shard 2"]
    W2["Worker 2: data shard B"] <-->|"Pull parameters; push gradients"| S1
    W2 <-->|"Pull parameters; push gradients"| S2

    style W1 fill:#E1F5FE
    style W2 fill:#E1F5FE
    style S1 fill:#C8E6C9
    style S2 fill:#C8E6C9
{{< /mermaid >}}

Each worker may need parameters from **every** server, even though it processes only its own data shard. Data partitioning and parameter partitioning solve different problems.

For the broader choice between data, model, and pipeline parallelism, see [Distributed Training Strategies]({{< relref "070-distributed-training-strategies.md" >}}).

<!-- Source grounding: Session 9.pptx, slides 5–14; 9 - MLSO.vtt, 00:13–00:22 and 00:29–00:31. -->

## 1. Why Distribute Training?

Two constraints often appear together:

1. **Computation:** processing a large dataset takes too long on one worker.
2. **Memory:** weights, gradients, and optimiser state exceed the available memory.

Let `P` be the number of model parameters, `N` the number of examples, and `c` the approximate computation per example.

Model-related storage grows with `P`, while processing the dataset requires approximately:

{{% colour "green" %}}
{{< katex display=true >}}
W_{\text{compute}}=O(Nc)
{{< /katex >}}
{{% /colour %}}

Distributing examples addresses computation. Distributing model state addresses storage. A useful design must also account for the communication introduced by both.

### Weights Alone Are Not the Training Memory

FP16 weights use two bytes per parameter. An illustrative mixed-precision training configuration with Adam uses about **16 bytes per parameter** for weights, gradients, master weights, and optimiser states combined.

{{% colour "green" %}}
{{< katex display=true >}}
M_{\text{weights}}=2P\ \text{bytes},
\qquad
M_{\text{training state}}\approx16P\ \text{bytes}
{{< /katex >}}
{{% /colour %}}

Using decimal units, where `1 GB = 10^9 bytes`:

| Parameters | FP16 weights alone | Training state at 16 bytes per parameter |
|---:|---:|---:|
| 1 billion | 2 GB | 16 GB |
| 7 billion | 14 GB | 112 GB |
| 70 billion | 140 GB | 1.12 TB |
| 1 trillion | 2 TB | 16 TB |

A seven-billion-parameter model can therefore have **14 GB of weights but approximately 112 GB of training state** under this assumption.

{{% hint warning %}}
The 16-byte estimate is configuration-dependent, not a universal constant. It excludes activations, input data, temporary workspaces, and allocator overhead. Weights fitting in memory does not imply that training fits.
{{% /hint %}}

## 2. Parameter Servers and Workers ☆

The architecture separates **state management** from **gradient computation**.

| Role | Main responsibilities | Typical pressure |
|---|---|---|
| Parameter server | Own parameter shards, receive gradients, apply updates, return current parameters; often own corresponding optimiser state | Memory capacity, update throughput, network bandwidth |
| Worker | Read a data shard, obtain parameters, run forward and backward passes, send gradients | Compute throughput, local memory, input throughput |

The parameter service is **logically centralised**, because it manages a shared model. It need not be one physical machine: several servers can each own a different shard.

A worker is also a logical role. A machine with several GPUs may run several workers; “one worker” does not necessarily mean “one machine”.

### Independent Scaling

- Add workers when gradient computation is the bottleneck.
- Add parameter servers when server-side memory or update service is the bottleneck.
- Improve the communication path when workers spend most of their time waiting for parameters or sending gradients.

This separation allows resources to match their jobs. It does not guarantee that either side can scale without limits.

### Parameter Sharding

With `S` servers, partition the model into `S` disjoint parameter groups:

{{% colour "green" %}}
{{< katex display=true >}}
\theta=[\theta_1,\theta_2,\ldots,\theta_S]
{{< /katex >}}
{{% /colour %}}

For balanced shards, each server owns approximately `P/S` parameters. If all of the estimated training state is sharded evenly:

{{% colour "green" %}}
{{< katex display=true >}}
M_{\text{state per server}}\approx\frac{16P}{S}\ \text{bytes}
{{< /katex >}}
{{% /colour %}}

For the seven-billion-parameter example, four servers would each hold approximately **28 GB** of that state, before additional overhead.

{{% hint warning %}}
Sharding server-side state does not automatically shard each worker's forward-pass model. In the basic data-parallel arrangement, workers still hold local model replicas. If a replica and its working memory do not fit, additional model or tensor partitioning is needed.
{{% /hint %}}

<!-- Source grounding: slides 11–17 and 21–24; transcript 00:29–00:31, 00:41–00:46, and 00:52. Normalisation is made explicit to reconcile local means with the sum notation. -->

## 3. Distributed Loss and Gradient Updates ☆

### Local and Global Loss

Worker `k` owns a dataset shard containing `N_k` examples. Its local average loss is:

{{% colour "green" %}}
{{< katex display=true >}}
L_k(\theta)=\frac{1}{N_k}\sum_{i\in D_k}\ell(x_i,y_i;\theta)
{{< /katex >}}
{{% /colour %}}

Here, `D_k` is the worker's data shard and `ℓ` measures the error on one example. Training aims to find parameters that minimise the loss. For a global mean over all `N` examples:

{{% colour "green" %}}
{{< katex display=true >}}
L(\theta)=\sum_{k=1}^{K}\frac{N_k}{N}L_k(\theta),
\qquad N=\sum_{k=1}^{K}N_k
{{< /katex >}}
{{% /colour %}}

When all shards are equally sized, this becomes the mean of the `K` local losses.

### Gradients from Mini-Batches

During one synchronous step, each worker evaluates its local mini-batch using the same parameter version. If its mini-batch contains `b_k` examples, its mean gradient is:

{{% colour "green" %}}
{{< katex display=true >}}
g_k=\frac{1}{b_k}\sum_{i\in B_k}\nabla_\theta\ell(x_i,y_i;\theta^{(t)})
{{< /katex >}}
{{% /colour %}}

The combined mean gradient weights each worker by its batch size:

{{% colour "green" %}}
{{< katex display=true >}}
g=\sum_{k=1}^{K}\frac{b_k}{B_{\text{global}}}g_k,
\qquad B_{\text{global}}=\sum_{k=1}^{K}b_k
{{< /katex >}}
{{% /colour %}}

For equal local batch sizes:

{{% colour "green" %}}
{{< katex display=true >}}
g=\frac{1}{K}\sum_{k=1}^{K}g_k,
\qquad
\theta^{(t+1)}=\theta^{(t)}-\eta g
{{< /katex >}}
{{% /colour %}}

The learning rate `η` controls the size of the update. Each server applies the component belonging to its own shard:

{{% colour "green" %}}
{{< katex display=true >}}
\theta_s^{(t+1)}=\theta_s^{(t)}-\eta\frac{1}{K}\sum_{k=1}^{K}g_{k,s}
{{< /katex >}}
{{% /colour %}}

Here, `g_{k,s}` is the portion of worker `k`'s gradient associated with shard `s`.

### Sum Versus Mean: Keep the Convention Consistent

Distributed updates are also written using a **sum** of worker gradients. This is consistent when local objectives are defined as contributions to a sum, or when the learning rate includes the required normalisation.

If workers send local **mean** gradients for equal batches, summing them without dividing by `K` makes the update `K` times larger than averaging them at the same learning rate.

For example, with four workers, `Σg_k = 4 × mean(g_k)`. Never switch silently between these conventions.

## 4. The Pull–Compute–Push Cycle

One basic synchronous iteration proceeds as follows:

1. **Pull:** workers obtain the current parameter shards and assemble their local model state.
2. **Compute:** each worker processes its mini-batch and calculates gradients independently.
3. **Push:** workers send gradient components to the servers that own the corresponding parameters.
4. **Aggregate and update:** servers combine the required contributions and update their shards.
5. **Repeat:** workers obtain the next parameter version and continue training.

{{< mermaid >}}
flowchart TD
    A["Pull current parameters"] --> B["Compute local gradients"]
    B --> C["Push gradient shards"]
    C --> D["Aggregate and update"]
    D --> A

    style A fill:#E1F5FE
    style B fill:#C8E6C9
    style C fill:#FFF9C4
    style D fill:#EDE7F6
{{< /mermaid >}}

### Synchronisation and Staleness

In synchronous training, the update waits for the required workers. A slow worker can delay the group.

In asynchronous training, updates can be applied as they arrive. Workers may then calculate gradients using older parameters, producing **stale gradients**.

A system can instead use an explicit threshold, such as accepting 90 contributions out of 100 workers. This reduces waiting but changes the update policy. It must define which versions are accepted and how the selected gradients are normalised; it is not automatically equivalent to either full synchrony or unrestricted asynchronous SGD.

<!-- Source grounding: slides 18–20 and 25–31; transcript 00:56–01:00 and 01:07; slides 39–40 and transcript 01:40–01:43. -->

## 5. Compute Scaling Versus Communication ☆

### Ideal Compute Scaling

For a **fixed total workload** split evenly among `K` workers, the compute portion ideally falls to:

{{% colour "green" %}}
{{< katex display=true >}}
T_{\text{compute}}(K)\approx\frac{T_{\text{compute}}(1)}{K},
\qquad W_{\text{per worker}}=O\!\left(\frac{Nc}{K}\right)
{{< /katex >}}
{{% /colour %}}

An illustrative workload requiring 64 days of compute on one worker would have these ideal times:

| Workers | Ideal compute time |
|---:|---:|
| 1 | 64 days |
| 4 | 16 days |
| 16 | 4 days |
| 64 | 1 day |

These figures exclude communication, synchronisation, imbalance, and input bottlenecks. They also do not describe a run where more workers simultaneously increase the total workload.

### Total Step Time

Without overlap, a simple model is:

{{% colour "green" %}}
{{< katex display=true >}}
T_{\text{step}}\approx T_{\text{compute}}+T_{\text{communication}}
{{< /katex >}}
{{% /colour %}}

As compute becomes faster, communication may become the dominant term. Systems that overlap these activities require a more detailed timing model.

### Communication Volume

For a dense gradient with `P` entries and `b_g` bytes per entry, one worker's gradient push contains:

{{% colour "green" %}}
{{< katex display=true >}}
V_{\text{push}}=b_gP\ \text{bytes}
{{< /katex >}}
{{% /colour %}}

With `K` workers, a full push and pull per worker generates aggregate payload volume of approximately:

{{% colour "green" %}}
{{< katex display=true >}}
V_{\text{total}}\approx K(b_g+b_\theta)P\ \text{bytes}
{{< /katex >}}
{{% /colour %}}

Here, `b_θ` is the number of bytes per transferred parameter. This excludes metadata, retries, and protocol overhead.

At fixed precision, traffic therefore grows roughly with **worker count × parameter count**. On a shared bottleneck link, more workers can increase waiting rather than useful throughput.

More server shards can increase coordination and the number of server contacts. A simplified `O(S)` contact count is **not** a universal wall-clock law: sharding can also distribute memory pressure and provide parallel network paths. Distinguish message count, transferred bytes, and elapsed time.

### Worked Example: Transfer a 70-Billion-Parameter Gradient

Assume a full FP16 gradient and decimal units:

{{% colour "green" %}}
{{< katex display=true >}}
V=70\times10^9\times2=140\times10^9\ \text{bytes}=140\ \text{GB}
{{< /katex >}}
{{% /colour %}}

A `100 Gbit/s` link has an ideal byte rate of:

{{% colour "green" %}}
{{< katex display=true >}}
R=\frac{100}{8}=12.5\ \text{GB/s}
{{< /katex >}}
{{% /colour %}}

The ideal transfer time for **one gradient push** is:

{{% colour "green" %}}
{{< katex display=true >}}
T_{\text{transfer}}=\frac{V}{R}=\frac{140}{12.5}=11.2\ \text{s}
{{< /katex >}}
{{% /colour %}}

| Link rate | Ideal byte rate | 7-billion-parameter FP16 gradient | 70-billion-parameter FP16 gradient |
|---:|---:|---:|---:|
| 100 Gbit/s | 12.5 GB/s | 1.12 s | 11.2 s |
| 400 Gbit/s | 50 GB/s | 0.28 s | 2.8 s |
| 800 Gbit/s | 100 GB/s | 0.14 s | 1.4 s |

{{% hint warning %}}
These are payload-only lower bounds for one direction, assuming the whole stated link rate is available. They exclude the parameter pull, contention, and software overhead. **Gbit/s is not GB/s**, and a combined bidirectional bandwidth figure is not necessarily available to a one-way transfer.
{{% /hint %}}

## 6. Communication Tools and Hardware Paths

RDMA, MPI, PCIe, and NVLink belong to different layers of the system. They are not interchangeable names for “a faster network”.

| Technology | What it is | Why it matters |
|---|---|---|
| RDMA | Remote Direct Memory Access, used for network transfers between registered memory regions | Reduces CPU involvement and operating-system overhead in the transfer data path |
| MPI | Message Passing Interface: a standard programming interface for communication between processes | Provides communication operations, including blocking and non-blocking forms |
| PCIe | Peripheral Component Interconnect Express: a general-purpose system interconnect | Connects components such as GPUs, network cards, and SSDs to the host system |
| NVLink | A high-bandwidth GPU interconnect | Supports fast communication between connected GPUs in supported topologies |

### Important Distinctions

- **RDMA does not mean zero CPU use:** setup, memory registration, and control still require system work.
- **An MPI non-blocking call does not imply asynchronous SGD:** communication semantics and training-update policy are separate choices.
- **NVLink does not replace every inter-node network path:** local GPU connectivity and cluster networking must both be considered.
- **PCIe lane allocation matters:** several GPUs, network cards, and SSDs may compete for the host's available connectivity.
- **Memory bandwidth matters as well as capacity:** CPU workloads that repeatedly read large tensors can stall even when sufficient RAM is installed.

The useful design question is: **which path carries each transfer, and what else shares that path?** Faster GPUs cannot compensate indefinitely for a congested interconnect or slow input path.

<!-- Source grounding: slides 32–37 and 41–42; transcript 01:08–01:10, 01:24–01:39, and 01:43–01:44. Hardware-generation forecasts and inconsistent product-rate claims are intentionally omitted. -->

## 7. Batch Size and Learning Rate ☆

### Global Batch Size

With `K` workers and equal local mini-batch size `B_local`:

{{% colour "green" %}}
{{< katex display=true >}}
B_{\text{global}}=K B_{\text{local}}
{{< /katex >}}
{{% /colour %}}

Eight workers processing 32 examples each produce a global batch of **256**. Increasing to 32 workers while keeping the local batch unchanged produces **1,024** examples per update.

This changes the optimisation process: each update uses more examples, and a fixed dataset requires fewer updates per pass.

### Linear Learning-Rate Scaling

A common starting heuristic scales the learning rate in proportion to global batch size:

{{% colour "green" %}}
{{< katex display=true >}}
\eta_{\text{new}}=\eta_{\text{base}}\frac{B_{\text{new}}}{B_{\text{base}}}
{{< /katex >}}
{{% /colour %}}

For baseline batch size 256 and learning rate 0.1:

| Workers | Local batch | Global batch | Linearly scaled learning rate |
|---:|---:|---:|---:|
| 8 | 32 | 256 | 0.1 |
| 32 | 32 | 1,024 | 0.4 |
| 128 | 32 | 4,096 | 1.6 |
| 256 | 32 | 8,192 | 3.2 |

For 32 workers:

{{% colour "green" %}}
{{< katex display=true >}}
\eta_{\text{new}}=0.1\times\frac{1024}{256}=0.4
{{< /katex >}}
{{% /colour %}}

The table illustrates the heuristic, not a guarantee that every listed rate will train stably. The model, optimiser, data, and gradient-normalisation convention all matter.

### Warm-Up

Warm-up begins with a **smaller** learning rate and gradually increases it towards the target rate. A later schedule may then reduce the rate.

This avoids immediately applying a large update before early training has stabilised. It is not the same as starting with the largest rate and decreasing it from the first step.

When scaling a run:

1. calculate the new global batch
2. choose an initial learning-rate rule
3. introduce or adjust warm-up where appropriate
4. compare training loss and validation quality
5. retain the change only if throughput and convergence together improve

## 8. Monitoring and Fault Recovery

### Measure Where Time Goes

A useful monitoring dashboard separates symptoms instead of relying on GPU utilisation alone.

| Observation | What to investigate |
|---|---|
| GPUs idle between bursts | Parameter transfers, synchronisation, or input delivery |
| Servers heavily loaded while workers wait | Server update throughput, memory access, or network contention |
| One worker consistently finishes late | Uneven data, device performance, or local input bottlenecks |
| Faster steps but poorer validation quality | Batch size, learning rate, warm-up, or stale updates |

Run short, comparable trials and record step time, throughput, network activity, memory use, and loss behaviour. The best configuration is not necessarily the largest one: seek a good **time to acceptable model quality**, with sensible resource cost.

### Worker Failure Is Not Server Failure

If a worker fails, a system may retry its work, restart it, or explicitly reconfigure the active worker set. A synchronous run must handle the missing contribution rather than wait forever.

If training continues with fewer workers, aggregation and global batch size may need to change. Simply dropping a worker's gradient does not preserve the original update automatically.

If a parameter server fails, its shard and optimiser state must be recovered or reconstructed. Distribution alone does not provide that recovery: checkpoints, replication, or another explicit mechanism are required.

### Operational Lesson

A service can continue running on healthy GPUs while still sending new requests to a failed device. Fault detection must therefore connect to **routing and scheduling**, not merely to an alert.

Preserve a recoverable checkpoint or deployment version, validate changes separately, and confirm that work actually reaches healthy resources after recovery.

<!-- Source grounding: slides 13 and 23; transcript 00:23–00:28, 00:49–00:50, and 01:01–01:06. Practical examples retain the systems lesson without institutional or personal details. -->

## 9. Where Parameter Servers Fit Best

### Sparse Embedding Workloads

Recommendation systems can have very large embedding tables. A particular batch may touch only a small subset of the rows.

Rather than transferring an entire table, workers can request the relevant rows and send updates for the entries they used. Parameter servers are well suited to this combination of **large shared state and sparse access**.

### Dense Neural-Network Workloads

In dense training, a step may generate gradients for nearly every parameter. Repeatedly pushing and pulling full tensors through central services can create a communication bottleneck.

Collective communication such as all-reduce is therefore often useful for dense gradient aggregation. Sharded-state approaches can also distribute memory without requiring a conventional parameter-server service.

| Workload property | Architectural implication |
|---|---|
| Huge embedding table; each batch touches few rows | Sparse pull/push through parameter servers can be effective |
| Dense gradients; replicated model fits | Collective gradient aggregation is often attractive |
| Full model or training state does not fit locally | Additional state sharding or model partitioning is needed |

These are design tendencies, not absolute rules. Access patterns, hardware topology, consistency requirements, and implementation determine the final choice.

### Preserve Useful Model Behaviour

Further training for a narrow domain can improve specialised knowledge while weakening other abilities. Retain earlier checkpoints and evaluate both the new task and previously useful behaviour.

In some applications, a separate component can refine presentation or handle another task instead of repeatedly retraining the same model. Composition is a design option, not a guarantee that quality problems disappear.

<!-- Source grounding: slide 43 and transcript 01:45–01:47; domain adaptation and component composition discussed at 00:23–00:28. Framework names are not expanded into unsupported implementation tutorials. -->

## 10. Common Mistakes

| Mistake | Correction |
|---|---|
| Treat FP16 weight memory as total training memory | Include gradients, optimiser state, activations, and overhead |
| Assume server sharding makes every worker's model fit | Check the worker's own memory and execution layout |
| Sum local mean gradients but use the averaging learning rate unchanged | Keep loss normalisation, aggregation, and learning rate consistent |
| Divide by 100 GB/s for a 100 Gbit/s link | Convert bits to bytes first: 100/8 = 12.5 GB/s |
| Use total bidirectional bandwidth for one-way transfer time | Use the available rate for that direction and topology |
| Add workers without checking global batch size | Recalculate batch size and validate the optimisation settings |
| Expect linear end-to-end speedup | Include communication, synchronisation, imbalance, and input costs |
| Assume distribution automatically handles every failure | Define recovery, checkpointing, and worker-membership policies |

## Practice Questions

These are illustrative checks based on the concepts above.

1. **A model has seven billion parameters. Estimate FP16 weight memory and training-state memory at 16 bytes per parameter.**
   - Answer: **14 GB** of weights and **112 GB** of training state, excluding activations and other overhead.
2. **Four parameter servers share that estimated training state evenly. How much does each hold?**
   - Answer: **28 GB**, before overhead. This does not determine worker memory.
3. **A fixed workload requires 64 days of compute on one worker. What is the ideal compute time on 16 workers?**
   - Answer: **4 days**, before communication and other overheads.
4. **How long does one 140 GB gradient push take over an ideal 100 Gbit/s link?**
   - Answer: **11.2 seconds**, because the ideal byte rate is **12.5 GB/s**.
5. **Thirty-two workers use local batches of 32. What is the global batch? If the baseline is batch 256 at learning rate 0.1, what does linear scaling suggest?**
   - Answer: global batch **1,024** and candidate learning rate **0.4**; validate stability and quality.
6. **Why can adding workers make training slower?**
   - Answer: communication and waiting can grow faster than the compute portion shrinks.
7. **Why are parameter servers attractive for large embedding tables?**
   - Answer: they distribute shared state while allowing sparse access and updates to only the rows used.

## Key Takeaways

{{% hint success %}}
- Parameter servers manage shared model state; workers calculate gradients.
- Storage scaling and compute scaling are distinct from communication scaling.
- A correct distributed update keeps gradient normalisation and learning rate consistent.
- Transfer time depends on payload bytes and usable bandwidth, not a network label alone.
- Larger worker counts change global batch size unless local batches are adjusted.
- Monitor time to useful model quality and define recovery explicitly.
{{% /hint %}}

## Understanding Checklist

- [ ] I can distinguish workers, parameter shards, and physical machines.
- [ ] I can estimate weights-only memory and a stated training-state budget.
- [ ] I can explain pull, compute, push, aggregate, and update.
- [ ] I can distinguish summed gradients from averaged gradients.
- [ ] I can convert Gbit/s to GB/s and calculate a one-way transfer lower bound.
- [ ] I can calculate global batch size and a candidate scaled learning rate.
- [ ] I can explain warm-up, stale gradients, and explicit fault recovery.
- [ ] I can compare sparse embedding access with dense gradient communication.

---
{{< home-link "Home" >}} | {{< section-index >}}
