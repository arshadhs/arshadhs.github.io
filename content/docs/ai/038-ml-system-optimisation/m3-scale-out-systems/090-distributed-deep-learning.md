---
title: "Distributed Deep Learning: Synchronisation and Communication"
draft: false
tags: ["ML System Optimisation", "MLSysOps", "Distributed Deep Learning", "Distributed SGD", "Synchronisation", "Gradient Compression", "AI", "ML"]
categories: ["AI", "ML"]
weight: 900
menu:
  main:
    identifier: mlsysops_ch090
    parent: mlsysops_m3
---

# Distributed Deep Learning: Synchronisation and Communication

Distributed training divides computation between workers, but those workers rarely finish together. A training system must decide **when to update the model, how much outdated information to accept, and how to reduce communication**.

Relevant themes from the content outline:

- Asynchronous parallelism and parameter staleness
- Effects on convergence and variance
- System and program optimisation for neural networks
- Communication–computation trade-offs

<!-- Outline alignment: _COURSE_MLSysOps_ZG516.pdf, page 6. This page covers the parameter-server material supplied for this topic. Collective communication, matrix multiplication, and Local SGD remain separate coverage. -->

The [Parameter Server model]({{< relref "080-ml-platforms-and-frameworks.md" >}}) provides the architectural foundation. Here, the focus is on the policies that determine its performance.

Continue with [Distributed SGD and All-Reduce]({{< relref "095-distributed-sgd-and-all-reduce.md" >}}) for weighted objectives, neighbour-based consensus, and ring, tree, and recursive collective algorithms.

## Learning Objectives

- Compare synchronous, asynchronous, and bounded-staleness execution.
- Calculate waiting time, utilisation, gradient throughput, and transfer time.
- Distinguish worker progress from model-version staleness.
- Explain the differences between the four k-worker/k-batch policies.
- Assess sparsification, quantisation, overlap, and parameter placement.
- Choose optimisations using time to a target model quality.

## 1. Gradients, Model Versions, and Staleness ☆

A worker **pulls parameters**, computes a gradient using its local mini-batch, and **pushes the gradient** to the server. The server applies an update to the shared model. Workers send gradient vectors: a scalar loss alone does not tell the server how each parameter should change.

With local mean mini-batch loss represented by L, worker k computes:

{{% colour "green" %}}
{{< katex display=true >}}
g_k = \nabla_{\theta} L_k(\theta)
{{< /katex >}}
{{% /colour %}}

During this computation, other workers may update the shared model. Let t be the current server version immediately before applying this gradient, and let v be the version used to compute it. Its **version staleness** is:

{{% colour "green" %}}
{{< katex display=true >}}
\tau_k = t-v_k \geq 0
{{< /katex >}}
{{% /colour %}}

**Example:** a worker used version 1, and the server is now at version 4. The gradient is three updates old. It may still be useful, but it describes the loss landscape at an earlier parameter vector.

Staleness counts intervening model updates. It is neither a percentage of unused data nor a direct measure of elapsed time.

<!-- Sources: 10_MLSO_Session_10.pptx, slides 3–5; both corresponding transcripts, discussion of the pull–compute–push cycle and staleness. -->

## 2. Bulk Synchronous Parallel Execution — BSP ☆

In **BSP**, all workers use the current model, compute their gradients, and meet at a barrier. The server waits for every worker before forming the next model.

For K workers with equally sized mini-batches and local mean gradients:

{{% colour "green" %}}
{{< katex display=true >}}
\theta^{(t+1)} = \theta^{(t)} - \eta\frac{1}{K}\sum_{k=1}^{K} g_k\left(\theta^{(t)}\right)
{{< /katex >}}
{{% /colour %}}

Here, η is the learning rate. Unequal mini-batch sizes require an appropriately weighted average; see the distributed-gradient discussion in the preceding page.

The benefit is a consistent update using fresh gradients. The cost is **straggler waiting**: fast workers remain idle until the slowest worker finishes.

### Worked Example: Three Unequal Workers

Suppose workers W1, W2, and W3 require **2, 5, and 7 time units** per mini-batch. Ignore communication and server-update time.

| Worker | Compute time | Waiting time per cycle |
|---|---:|---:|
| W1 | 2 | 5 |
| W2 | 5 | 2 |
| W3 | 7 | 0 |

One cycle takes seven time units. The three workers provide 14 useful worker-time units out of 21 available worker-time units:

{{% colour "green" %}}
{{< katex display=true >}}
U_{\mathrm{BSP}} = \frac{2+5+7}{3\times7}=\frac{2}{3}\approx66.67\%
{{< /katex >}}
{{% /colour %}}

The gradient-contribution rate is 3/7 per time unit, while the **model-update rate is 1/7**. Three gradients produce one averaged update.

Within the first 15 time units, updates occur at times 7 and 14: **two model updates using six gradients**.

<!-- Sources: slides 7–8; worker-time example expanded to show the denominator and distinguish gradients from updates. -->

## 3. Asynchronous Parallel Execution — ASP ☆

In **ASP**, the server applies each arriving gradient immediately. A worker then obtains newer parameters and continues, without waiting for the other workers.

{{% colour "green" %}}
{{< katex display=true >}}
\theta^{(t+1)}=\theta^{(t)}-\eta g_k\left(\theta^{(v_k)}\right)
{{< /katex >}}
{{% /colour %}}

The gradient was computed at version v, but it changes the current model at version t. This removes the global barrier and introduces stale updates.

### Continuing the Three-Worker Example

For compute times 2, 5, and 7, the ideal long-run gradient-arrival rate is:

{{% colour "green" %}}
{{< katex display=true >}}
R_{\mathrm{ASP}}=\frac{1}{2}+\frac{1}{5}+\frac{1}{7}\approx0.8429
{{< /katex >}}
{{% /colour %}}

| Worker | Arrival times up to and including time 15 | Gradients |
|---|---|---:|
| W1 | 2, 4, 6, 8, 10, 12, 14 | 7 |
| W2 | 5, 10, 15 | 3 |
| W3 | 7, 14 | 2 |

There are **12 gradient arrivals and 12 model updates** in this finite interval. With negligible communication, workers keep computing throughout.

To illustrate version staleness, process simultaneous arrivals in worker-number order, applying each update before that worker pulls again. W3 initially pulls version 0. When it first finishes at time 7, four other updates have already occurred, so its first gradient has staleness 4.

High utilisation does not guarantee faster learning. A fast worker contributes more frequently, and stale gradients may require smaller learning rates or more updates to reach the same quality.

{{% hint warning %}}
Compare **time to a target accuracy or loss**, alongside throughput. Two policies can perform different numbers of model updates while processing the same number of gradients.
{{% /hint %}}

<!-- Sources: slides 9–12; transcripts' contrast between barrier waiting, throughput, and stale gradients. The arrival convention is explicit because simultaneous events affect version counts. -->

## 4. Stale Synchronous Parallel Execution — SSP ☆

**SSP** permits workers to progress at different speeds, but bounds how far ahead the fastest worker may move. It occupies the middle ground between a full barrier and unrestricted asynchronous execution.

Let i denote a worker's iteration/progress counter and s the allowed progress gap:

{{% colour "green" %}}
{{< katex display=true >}}
i_{\mathrm{fast}}-i_{\mathrm{slow}}\leq s
{{< /katex >}}
{{% /colour %}}

A worker that would advance beyond this gap waits until the slower workers catch up. A bound of zero enforces synchronous progress; a larger bound gives workers more freedom.

**Example with s = 1:** all workers begin iteration 1 at time zero. W1 finishes at time 2 and begins iteration 2. At time 4, it wants to begin iteration 3, but W3 is still on iteration 1. The proposed gap is two, so W1 waits. When W3 begins iteration 2 at time 7, W1 may begin iteration 3.

This handles temporary differences in speed. A persistently slow worker still limits sustained progress because the others cannot move indefinitely ahead.

{{% hint info %}}
SSP's worker-progress bound and a gradient's server-version age use **different counters**. A server may receive multiple updates during one worker iteration. Do not substitute one count for the other without specifying the implementation.
{{% /hint %}}

SSP does not mean waiting for a fixed wall-clock timeout or dropping every slow worker's result. Its essential rule is the bounded progress gap.

<!-- Sources: slides 14–15 and both transcripts. Counter distinction clarified against the SSP definition in Ho et al., 2013. -->

## 5. Partial Participation: Four k-Based Policies

Another approach updates after a smaller group of contributions. Two choices determine the policy:

1. **Count distinct workers or count mini-batches?** A fast worker can supply several mini-batches, but it still counts as only one distinct worker.
2. **Cancel unfinished work or retain it?** Retaining it avoids wasted computation but allows gradients calculated from older models to arrive later.

Here k is the update threshold, with k smaller than the total number of workers K.

| Policy | Update after | Unfinished work | Stale gradients possible? |
|---|---|---|---|
| k-BSP | k distinct workers | Cancelled at the update | No, under the stated round policy |
| k-batch-BSP | Any k mini-batches | Cancelled at the update | No, under the stated round policy |
| k-ASP | k distinct workers | Retained | Yes |
| k-batch-ASP | Any k mini-batches | Retained | Yes |

In the BSP variants, accepted gradients use the same current model. In k-batch-BSP, a fast worker may compute another mini-batch using that model before the update threshold is reached.

In k-ASP, a worker that has contributed waits for the group update, while unfinished workers keep computing. In k-batch-ASP, workers keep producing mini-batches without that distinct-worker restriction.

### Worked Example: k = 2

Use the same compute times of 2, 5, and 7, starting all workers at time zero. Count events through time 15, with negligible communication and update time.

| Policy | Model-update times | Updates | Accepted gradients |
|---|---|---:|---:|
| BSP | 7, 14 | 2 | 6 |
| ASP | Every gradient arrival | 12 | 12 |
| k-BSP | 5, 10, 15 | 3 | 6 |
| k-batch-BSP | 4, 8, 12 | 3 | 6 |
| k-ASP | 5, 7, 10, 14 | 4 | 8 |
| k-batch-ASP | 4, 6, 8, 10, 14, 15 | 6 | 12 |

For **k-BSP**, W1 finishes at time 2 and waits; W2 finishes at time 5 and triggers the update. W3's unfinished calculation is cancelled. The pattern repeats every five time units.

For **k-batch-BSP**, W1 produces mini-batches at times 2 and 4. These alone meet the threshold, so W2 and W3 are cancelled. The pattern repeats every four time units.

The second policy looks faster, but here only W1's data contributes. Whether this preserves the intended sampling distribution depends on how data and mini-batches are assigned. Cancelled computation and omitted contributions must be counted alongside the shorter update interval.

The asynchronous variants keep late work, reducing cancellation while accepting stale information. Grouping arrivals also changes the model-version counter, so a smaller numerical staleness count alone is insufficient evidence of better convergence.

<!-- Sources: slides 16–21; the table is a worked trace under explicitly stated assumptions, rather than a universal performance ranking. -->

## 6. Why Stragglers Become More Noticeable at Scale

Even nominally identical workers exhibit variable compute times. BSP observes the maximum completion time, so adding workers increases the opportunity for a slow completion in each round.

An illustrative model assumes independent exponential completion times with rate μ, so the mean time of one worker is 1/μ. For K workers:

{{% colour "green" %}}
{{< katex display=true >}}
\mathbb{E}[T_{\mathrm{BSP}}]=\frac{H_K}{\mu},\qquad H_K=\sum_{j=1}^{K}\frac{1}{j}
{{< /katex >}}
{{% /colour %}}

If only the first k distinct workers are required:

{{% colour "green" %}}
{{< katex display=true >}}
\mathbb{E}[T_{(k)}]=\frac{H_K-H_{K-k}}{\mu},\qquad H_0=0
{{< /katex >}}
{{% /colour %}}

For 64 workers, waiting for all takes about 4.744 times the mean time of one worker under this model. Waiting for roughly 75% gives a large-K approximation of ln(4)/μ, about 1.386/μ.

These estimates assume a fixed per-worker workload and a particular distribution of timing variation. They do not state that real training becomes slower whenever more GPUs are added.

<!-- Source: slide 12, order-statistics analysis and its assumptions. -->

## 7. Gradient Sparsification ☆

Communication can dominate training when a large gradient vector travels between workers and parameter servers. **Sparsification** sends only selected entries, often those with the largest absolute magnitudes.

Selecting the largest 1% of entries reduces the number of transmitted values substantially. However, the receiver also needs their positions, and discarded entries may still matter to learning.

### Error Feedback

Rather than forgetting the discarded part, a worker stores it as a residual and adds it to its next gradient before compression:

{{% colour "green" %}}
{{< katex display=true >}}
u_t=g_t+e_t,\qquad \widehat g_t=\operatorname{Top}_{\rho}(u_t),\qquad e_{t+1}=u_t-\widehat g_t
{{< /katex >}}
{{% /colour %}}

Here ρ is the retained fraction, and the compressed vector has zeros at omitted positions. Residual e carries the compression error forward.

**Example:** retain 25% of an eight-entry vector:

{{% colour "green" %}}
{{< katex display=true >}}
u_t=(0.9,-0.1,0.05,-1.2,0.3,0.02,-0.4,0.08)
{{< /katex >}}
{{% /colour %}}

The two largest magnitudes are 1.2 and 0.9. Their transmitted values are −1.2 and 0.9, with their positions. The six omitted entries become the residual for later steps.

Error feedback reduces information loss over successive steps; it does not guarantee that compression has no effect on convergence.

<!-- Sources: slides 23–24 and transcripts' explanation of retaining large gradients and carrying residuals. The eight-entry vector is an additional worked illustration. -->

## 8. Gradient Quantisation and Transfer Cost ☆

**Quantisation** reduces the number of bits used for each gradient value. Examples include 16-bit or 8-bit values, ternary values, and one-bit representations with scaling and error compensation.

Ternary gradients take three possible values, commonly represented as −1, 0, and +1 multiplied by a scale. Three symbols need at least two fixed-width bits. A one-bit gradient representation usually stores signs together with additional scaling information.

The sources also introduce **MQGrad**, which adapts gradient bit widths using reinforcement learning. The central idea is to change communication precision as training progresses, rather than assuming one precision is optimal throughout.

### Worked Example: 100 Million Gradient Entries

Assume 100 million entries, an ideal **10 Gb/s** one-way link, decimal MB, and no protocol overhead, metadata, compression time, or server delay.

{{% colour "green" %}}
{{< katex display=true >}}
T_{\mathrm{transfer}}=\frac{\text{payload bits}}{\text{link bits per second}}
{{< /katex >}}
{{% /colour %}}

| Representation | Assumed payload | Ideal transfer time |
|---|---:|---:|
| Dense FP32 | 400 MB | 320 ms |
| Dense 8-bit | 100 MB | 80 ms |
| Packed ternary, 2 bits per entry | 25 MB | 20 ms |
| Packed 1-bit signs | 12.5 MB | 10 ms |
| Top 1%, 32-bit value + 32-bit index | 8 MB | 6.4 ms |

The sparse payload includes one million values and one million indices:

{{% colour "green" %}}
{{< katex display=true >}}
D_{\mathrm{sparse}}=10^6(32+32)=64\times10^6\ \text{bits}=8\ \text{MB}
{{< /katex >}}
{{% /colour %}}

For eight workers sharing the **same server ingress link**, sending eight FP32 gradients requires at least 8 × 320 = **2,560 ms** of link service time. Separate links do not remove an aggregate bottleneck at a shared receiver.

Actual savings depend on indexing schemes, scale metadata, packing, reconstruction, hardware support, and convergence. Gradient compression also does not automatically mean that the stored model or the forward/backward computation uses the same precision.

<!-- Sources: slides 23–25. Payload table is an additional calculation using the communication principles and explicitly stated assumptions. -->

## 9. Overlapping Communication and Computation

Without overlap, compute and communication occur one after another. An ideal pipeline can hide some communication behind useful work:

{{% colour "green" %}}
{{< katex display=true >}}
T_{\mathrm{serial}}=T_{\mathrm{compute}}+T_{\mathrm{comm}},\qquad T_{\mathrm{overlap}}\approx\max(T_{\mathrm{compute}},T_{\mathrm{comm}})
{{< /katex >}}
{{% /colour %}}

**Example:** compute takes 60 ms and communication 40 ms. Serial execution takes 100 ms. Ideal steady-state overlap takes approximately 60 ms, giving a speedup of 100/60 ≈ **1.67**.

These are steady-state estimates. Dependencies, pipeline filling and draining, limited network capacity, and server contention can leave some communication exposed.

Two forms of overlap should be distinguished:

- **Within an iteration:** communicate gradients for completed layers while backpropagation continues through other layers. This can preserve the same model-version semantics.
- **Across iterations:** start new computation before preceding communication or updates complete. This may use older parameters and introduce staleness.

Overlap therefore requires a dependency-aware schedule. Starting operations concurrently is useful only when their inputs are available and they can make progress together.

<!-- Source: slide 26 and both transcripts' communication/computation discussion. -->

## 10. Parameter Placement and Access Locality

Parameter sharding splits the model across servers. A simple hash maps a parameter or parameter block to a server:

{{% colour "green" %}}
{{< katex display=true >}}
\operatorname{server}(p)=h(p)\bmod S
{{< /katex >}}
{{% /colour %}}

Here S is the number of servers. Hashing is easy to implement, but a similar number of blocks per server does not imply similar request traffic.

### Worked Example: Equal Block Counts, Unequal Load

Eight parameter blocks have access loads of **40, 25, 10, 8, 6, 5, 3, and 3** units. Assign two blocks to each server:

| Server | Blocks | Access load |
|---|---|---:|
| S1 | p1, p5 | 46 |
| S2 | p2, p6 | 30 |
| S3 | p3, p7 | 13 |
| S4 | p4, p8 | 11 |

The average load is 25, while the busiest server carries 46. Define a simple imbalance ratio as:

{{% colour "green" %}}
{{< katex display=true >}}
\beta=\frac{\max_j L_j}{\frac{1}{S}\sum_j L_j}=\frac{46}{25}=1.84
{{< /katex >}}
{{% /colour %}}

The busiest server has **1.84 times the average access load** despite equal block counts.

### Co-Access Placement

Blocks often requested together can be placed on the same server. If p1, p5, and p7 are needed together, the assignment above contacts three servers. Co-locating them can reduce that request's fan-out to one server.

This may also concentrate hot blocks on one server. Useful placement balances **access locality, memory, and traffic**, using workload measurements rather than block count alone.

Sharding describes ownership of different parameter subsets. Replication is a separate choice. Adding servers can distribute work but does not automatically reduce the total bytes required by a worker that needs every shard.

<!-- Sources: slides 28–30 and transcripts' hash/co-access placement discussion. Load figures are an additional numerical illustration. -->

## 11. Choosing an Optimisation

Begin with a measured baseline. Identify whether the largest cost is computation, communication, data loading, memory, or synchronisation.

| Observed bottleneck | Candidate change | Check afterwards |
|---|---|---|
| Long barrier waits | Partial participation or SSP | Sampling balance and convergence |
| Large gradient payloads | Sparsification or quantisation | Residual behaviour and model quality |
| Exposed communication | Dependency-aware overlap | Remaining network time and staleness |
| Uneven server traffic | Rebalance parameter placement | Hotspots, locality, and memory |
| High utilisation but slow learning | Revisit update policy | Time to the target loss or accuracy |

Use a small controlled run or simulation to compare policies before scaling up. Keep workload, data sampling, and target model quality comparable. Report wall-clock time, useful compute, communication, waiting, cancelled work, and convergence together.

<!-- Practical interpretation drawn from Madhusudhanan transcript, approximately 00:54–00:59; Mithun transcript's policy and timing examples. -->

## 12. Common Mistakes

{{% hint warning %}}
- Counting gradient contributions as model updates under every policy.
- Treating version staleness and worker-iteration lag as the same counter.
- Assuming 100% worker utilisation guarantees the best time to accuracy.
- Ignoring cancelled work and worker/data bias when using only fast results.
- Calculating sparse payloads without indices or quantised payloads without scales.
- Applying the ideal overlap formula to dependent operations without checking their schedule.
- Assuming equal parameter-block counts imply equal access loads.
{{% /hint %}}

## 13. Practice Questions and Short Answers

These questions check the concepts and worked examples on this page. They are not reproduced past-paper questions.

### 1. Why Does BSP Wait for the Slowest Worker?

**Question:** Explain the barrier in BSP and its effect on fast workers.

**Answer:** A BSP update combines all workers' gradients calculated using the same model. Fast workers finish early but wait at the barrier until the final gradient arrives. This preserves fresh, coordinated updates and reduces utilisation when worker times differ.

### 2. Calculate BSP Utilisation

**Question:** Three workers take 2, 5, and 7 time units. Ignoring communication, find cycle time, utilisation, and updates completed by time 15.

**Answer:** Cycle time is 7. Useful worker-time is 14 out of 21, so utilisation is 66.67%. Updates at times 7 and 14 give two model updates using six gradients.

### 3. Calculate Version Staleness

**Question:** A gradient was computed using version 12 and arrives while the server is at version 17. How stale is it?

**Answer:** Staleness is 17 − 12 = **5 server updates**. It describes the age of the gradient's parameter snapshot.

### 4. Distinguish SSP from a Timeout

**Question:** Why is an SSP bound of two not a two-second waiting rule?

**Answer:** SSP bounds worker progress counters. A worker pauses if advancing would put it more than two iterations ahead of the slowest worker. The elapsed waiting time depends on when the slower worker progresses.

### 5. Compare k-BSP and k-batch-BSP

**Question:** What does each policy count, and why can the distinction affect data coverage?

**Answer:** k-BSP counts distinct workers; k-batch-BSP counts mini-batches, including multiple batches from one worker. A fast worker can therefore dominate k-batch-BSP updates. Coverage depends on how batches are sampled and assigned.

### 6. Calculate a Gradient Transfer Time

**Question:** What is the ideal time to send 100 million 8-bit entries over a 10 Gb/s link?

**Answer:** The payload is 800 million bits. Dividing by 10 billion bits/s gives **0.08 s = 80 ms**, excluding metadata and other overheads.

### 7. Explain Error Feedback

**Question:** Why retain omitted gradient values after sparsification?

**Answer:** The worker stores compression error as a residual and adds it to the next gradient before compression. Small omitted contributions can accumulate and be transmitted later instead of being discarded permanently.

### 8. Calculate Ideal Overlap Speedup

**Question:** Computation takes 60 ms and communication 40 ms. Compare serial time with ideal steady-state overlap.

**Answer:** Serial time is 100 ms; ideal overlap is 60 ms. Speedup is approximately **1.67**, assuming dependencies and hardware allow the overlap.

## Key Takeaways

{{% hint success %}}
- BSP trades waiting for fresh coordinated gradients; ASP trades coordination for throughput and stale information.
- SSP limits worker-progress differences, while k-based policies change which contributions trigger an update.
- Compression reduces payload, and overlap can hide communication; both require checking their effect on training.
- Parameter placement must account for access patterns as well as memory balance.
- The useful outcome is less time to the required model quality.
{{% /hint %}}

## Checklist

- [ ] I can calculate BSP waiting and utilisation.
- [ ] I can distinguish gradients, model updates, and worker iterations.
- [ ] I can explain all four k-based policies.
- [ ] I can include values, indices, and link units in transfer calculations.
- [ ] I can explain error feedback and the limits of overlap.
- [ ] I can identify access imbalance despite equal shard counts.

## References

[5] J. Dean et al., "Large scale distributed deep networks," in Proc. NIPS, 2012.
https://research.google/pubs/large-scale-distributed-deep-networks/

[6] J. Chen, X. Pan, R. Monga, S. Bengio and R. Jozefowicz, "Revisiting distributed synchronous SGD," arXiv:1604.00981, 2016.
1604.00981

---
{{< home-link "Home" >}} | {{< section-index >}}
