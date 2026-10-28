---
title: "Distributed SGD and All-Reduce"
draft: false
tags: ["ML System Optimisation", "MLSysOps", "Decentralised SGD", "All-Reduce", "Ring All-Reduce", "Communication", "AI", "ML"]
categories: ["AI", "ML"]
weight: 950
menu:
  main:
    identifier: mlsysops_ch095
    parent: mlsysops_m3
---

# Distributed SGD and All-Reduce

Workers can train towards one shared objective while combining their information in different ways. **Global gradient aggregation** keeps their models aligned at each step; **neighbour averaging** lets separate local models move towards agreement.

Relevant topics from the content outline:

- Decentralised SGD and all-reduce
- Ring versus tree-based reduction
- Bandwidth and latency considerations
- System optimisation techniques for neural networks

<!-- Content alignment: _COURSE_MLSysOps_ZG516.pdf, page 6, Distributed Deep Learning and All-reduce Paradigm. Primary material: 11_MLSO_Session_11.pdf, slides 4–33; 11 - MLSysOps.vtt. The transcript develops the formulation and centralised/decentralised distinction; the later collective algorithms come from the slides. -->

This continues Chapter 9. For BSP, ASP, SSP, gradient compression, and overlap, see [Synchronisation and Communication]({{< relref "090-distributed-deep-learning.md" >}}). For model-state storage and batch-size scaling, see [ML Platforms and Frameworks]({{< relref "080-ml-platforms-and-frameworks.md" >}}).

## Learning Objectives

- Form the global objective from unequal data shards.
- Distinguish decentralised optimisation from an all-reduce implementation of synchronous SGD.
- Calculate neighbour-averaging steps and interpret consensus.
- Explain reduce-scatter and all-gather.
- Compare collective algorithms using latency, payload, and topology.
- Calculate communication costs with consistent units and assumptions.

## 1. One Objective, Distributed Data ☆

Let w be a vector of d learnable parameters. Training seeks a parameter vector that minimises the global loss:

{{% colour "green" %}}
{{< katex display=true >}}
\min_{w\in\mathbb{R}^{d}} F(w)
{{< /katex >}}
{{% /colour %}}

Worker k holds data shard Sₖ containing nₖ samples. With m workers:

{{% colour "green" %}}
{{< katex display=true >}}
N=\sum_{k=1}^{m}n_k,\qquad F_k(w)=\frac{1}{n_k}\sum_{i=1}^{n_k}\ell(w;Z_{k,i})
{{< /katex >}}
{{% /colour %}}

Here ℓ is the loss for one example, such as cross-entropy. The local loss averages over one worker's samples; the global loss averages over all N samples.

{{% colour "green" %}}
{{< katex display=true >}}
F(w)=\sum_{k=1}^{m}\frac{n_k}{N}F_k(w),\qquad
\nabla F(w)=\sum_{k=1}^{m}\frac{n_k}{N}\nabla F_k(w)
{{< /katex >}}
{{% /colour %}}

The same parameters w appear in every term of this identity. Averaging gradients evaluated at different local models does not generally reproduce the gradient at one common model.

### Worked Example: Unequal Shards

| Worker | Samples | Local mean loss | Data weight | Weighted contribution |
|---|---:|---:|---:|---:|
| W1 | 600 | 0.20 | 0.6 | 0.12 |
| W2 | 300 | 0.50 | 0.3 | 0.15 |
| W3 | 100 | 0.90 | 0.1 | 0.09 |

The global mean loss is **0.36**. A plain average of the three local losses gives approximately **0.533**, giving the smallest shard the same influence as the largest.

These weights describe the full-data objective. For a particular synchronous mini-batch step, combine local mean gradients according to the samples represented in that step. Equal local mini-batches can use equal weights; unequal mini-batches require sample weighting. The sampling scheme determines whether the estimate represents the intended full-data objective.

### Data Shards and Model Shards

**Data parallelism** gives each worker a full model and different examples. **Model parallelism** places different parts of the model on different devices. Unequal data-shard sizes do not imply that the neural network's layers have been divided.

IID shards approximately reflect the same data distribution. Non-IID shards can represent different users, regions, or classes, so their gradients may disagree even at the same parameters.

<!-- Sources: slides 4–6; transcript 01:06:28–01:15:18. Data partitioning is kept distinct from model partitioning. -->

## 2. Centralised SGD: One Global Update

In synchronous **centralised SGD**, every worker starts a step from the same model. Workers compute local gradients, combine them into one global gradient, and apply the same update.

For equal local mini-batches with mean gradients:

{{% colour "green" %}}
{{< katex display=true >}}
\bar g^{(t)}=\frac{1}{m}\sum_{k=1}^{m}g_k^{(t)},\qquad
w^{(t+1)}=w^{(t)}-\eta\bar g^{(t)}
{{< /katex >}}
{{% /colour %}}

The global average is a **logical operation**. It can use a parameter server or a collective operation distributed across workers.

| Implementation | How the common gradient is obtained |
|---|---|
| Parameter server | Workers push gradients to server processes and obtain updated parameters |
| All-reduce | Workers collectively combine gradients; every participant obtains the result |

Consequently, a ring-based all-reduce can implement centralised SGD **without a central server**. It still produces a common global update. Neighbour-based decentralised SGD has different update semantics.

Equal updates preserve equal models when workers also start with matching parameters and compatible optimiser state. Floating-point reduction order can still affect numerical reproducibility.

Global synchronisation introduces waiting for required contributions. All-reduce distributes communication work, but it does not automatically remove stragglers or recover from a failed rank.

<!-- Sources: slides 8 and 13; transcript 01:17:05–01:25:08. Batch size, learning-rate scaling, and warm-up are covered in 080 rather than repeated here. -->

## 3. Decentralised SGD: Learn Locally, Mix with Neighbours ☆

In **decentralised SGD**, each worker maintains its own local model and exchanges information with neighbours in a communication graph. Workers need not hold identical models after every step.

The version used here first computes a local gradient, mixes model values, and subtracts that previously computed gradient:

{{% colour "green" %}}
{{< katex display=true >}}
g_k^{(t)}=\nabla F_k\!\left(w_k^{(t)}\right),\qquad
\widetilde w_k^{(t)}=\sum_{j\in\mathcal N_k}a_{kj}w_j^{(t)}
{{< /katex >}}
{{% /colour %}}

{{% colour "green" %}}
{{< katex display=true >}}
w_k^{(t+1)}=\widetilde w_k^{(t)}-\eta g_k^{(t)},\qquad
a_{kj}\geq0,\quad\sum_j a_{kj}=1
{{< /katex >}}
{{% /colour %}}

The neighbourhood includes worker k itself. In practice the gradient usually comes from a mini-batch. Other decentralised algorithms can order mixing and gradient evaluation differently; specify the convention before calculating.

### Worked Example: One Local Update

A worker has value **6**, its neighbours have **3** and **12**, and all three receive weight one-third. The mixed value is **7**.

If its gradient, evaluated at the original local model, is **2** and the learning rate is **0.5**, the next value is **7 − 0.5 × 2 = 6**. Without mixing, the local step would have produced **5**.

### What the Graph Changes

| Graph | Communication pattern | Main trade-off |
|---|---|---|
| Ring | Two neighbours per worker | Low degree; information travels through several hops |
| Two-dimensional torus | Usually four neighbours | More communication, potentially faster mixing |
| Exponential-style graph | Longer-range neighbour connections | Faster propagation with additional links |

With a fixed neighbour count, each worker's exchange volume need not grow directly with the total worker count. However, the number of rounds required for agreement can grow.

Removing a global collective does not make every decentralised algorithm asynchronous: a synchronous neighbour exchange can still wait for its peers. Failure recovery also needs explicit handling and adequate remaining connectivity.

For unequal data shards, plain uniform-worker mixing generally targets an equal-worker objective. Preserving the sample-weighted objective requires an appropriate weighting or algorithm; it does not follow from connectivity alone.

<!-- Sources: slides 9–11 and 13; the transcript introduces the distinction at 01:17–01:25. The neighbour-update equations and graph examples come from the slides. -->

## 4. Consensus: How Local Models Approach Agreement

To isolate mixing, temporarily switch off gradient updates. Every worker repeatedly averages its own model with its neighbours' models.

For a four-worker ring, use equal one-third weights for self and the two neighbours. Start with scalar values **(0, 3, 6, 9)**:

| Mixing round | W1 | W2 | W3 | W4 | Mean |
|---:|---:|---:|---:|---:|---:|
| 0 | 0 | 3 | 6 | 9 | 4.5 |
| 1 | 4 | 3 | 6 | 5 | 4.5 |
| 2 | 4 | 4.333 | 4.667 | 5 | 4.5 |

For example, W1's first value is (9 + 0 + 3)/3 = **4**. Each round uses the previous round's values simultaneously.

Let A be the mixing matrix. Row sums of one make each output a weighted average. If column sums are also one, A is **doubly stochastic**, so pure mixing preserves the arithmetic mean.

For a fixed symmetric mixing matrix whose non-consensus eigenvalues have magnitude below one:

{{% colour "green" %}}
{{< katex display=true >}}
\left\|w^{(r)}-\bar w\mathbf1\right\|_2
\leq \rho^{r}\left\|w^{(0)}-\bar w\mathbf1\right\|_2,
\qquad \rho=\max_{i\geq2}|\lambda_i(A)|<1
{{< /katex >}}
{{% /colour %}}

Here r counts mixing rounds, and w stacks the workers' scalar values. The four-worker example has ρ = 1/3; disagreement norms are approximately **6.708, 2.236, and 0.745**.

A 100-fold reduction requires at least:

{{% colour "green" %}}
{{< katex display=true >}}
r\geq\frac{\ln100}{\ln(1/\rho)}
{{< /katex >}}
{{% /colour %}}

Round upwards: approximately **five rounds** for ρ = 1/3. This is a mixing-only result. During training, local gradients continually introduce differences, especially with non-IID data.

<!-- Source: slide 12. The bound requires the stated matrix conditions; pure consensus and optimisation convergence are distinguished. -->

## 5. What All-Reduce Produces ☆

**All-reduce** combines one tensor from every participant using an element-wise reduction and leaves the result on every participant.

For scalar contributions **1, 2, and 3**, a sum all-reduce returns **6** to all three workers. Dividing by three produces the mean **2**. Summation and division should remain explicit when checking gradient normalisation.

Two useful decompositions are:

| Decomposition | First phase | Second phase |
|---|---|---|
| Reduce + broadcast | Form the reduced tensor at a root | Distribute the result |
| Reduce-scatter + all-gather | Each worker obtains a fully reduced chunk | Share those chunks so everyone obtains the full tensor |

A naïve implementation sends every full tensor directly to one root. A tree can distribute the reduction and broadcast work. Ring implementations use chunked reduce-scatter and all-gather.

Frameworks can group gradients into buckets and start collectives as backpropagation produces them. One training step therefore need not correspond to exactly one all-reduce call.

<!-- Sources: slides 15–16. Existing parameter-server architecture and communication overlap are linked rather than reproduced. -->

## 6. Ring All-Reduce ☆

Arrange p workers in a logical ring. Each sends to its successor and receives from its predecessor. Split an n-byte tensor into p equal chunks.

1. **Reduce-scatter:** take p − 1 rounds to accumulate contributions. Each worker ends with one fully reduced chunk.
2. **All-gather:** take another p − 1 rounds to circulate those completed chunks. Received chunks are copied, rather than added again.

Thus each worker sends:

{{% colour "green" %}}
{{< katex display=true >}}
V_{\mathrm{ring,send}}=2(p-1)\frac{n}{p}
{{< /katex >}}
{{% /colour %}}

It receives the same volume. Counting sent plus received bytes doubles this figure; keep the accounting convention clear.

For p = 4 and n = **100 MB**, each chunk is **25 MB**. Six rounds send **150 MB per worker**, with workers communicating concurrently. The sent volume approaches 2n as p grows, while the number of sequential rounds grows linearly.

### Ring All-Reduce Versus a Gossip Ring

Both use ring-shaped communication, but their results differ. Ring all-reduce finishes a collective with the same complete reduction at every worker. One gossip round only mixes neighbouring models; global agreement develops over repeated rounds.

### Worked Example: Ring Versus a Naïve Root

Assume decimal MB and GB, usable bandwidth **12.5 GB/s**, negligible start-up latency, and a naïve root that serialises receipt and redistribution of full tensors.

| Workers | Ring bytes sent per worker | Ideal ring time | Root-link total bytes | Serial root time |
|---:|---:|---:|---:|---:|
| 4 | 150 MB | 12 ms | 600 MB | 48 ms |
| 16 | 187.5 MB | 15 ms | 3,000 MB | 240 ms |

The ring distributes transfers across links. The root handles every other worker's full tensor in both phases. These timings compare the stated communication models; pipelining, concurrent root transfers, contention, and topology can change the observed ratio.

<!-- Sources: slides 18–20. A direct root bottleneck is distinguished from tree reduction and actual full-duplex schedules. -->

## 7. Model Latency and Bandwidth Costs ☆

A simple message model separates the fixed cost of starting a transfer from the time needed to move its payload:

{{% colour "green" %}}
{{< katex display=true >}}
T_{\mathrm{message}}=\alpha+n\beta
{{< /katex >}}
{{% /colour %}}

Here α is start-up latency, n is bytes, and β is seconds per byte—the inverse of usable byte bandwidth.

Assuming the basic power-of-two schedules where relevant, ignoring reduction computation and contention:

{{% colour "green" %}}
{{< katex display=true >}}
\begin{aligned}
T_{\mathrm{ring}}&=2(p-1)\alpha+2\frac{p-1}{p}n\beta\\
T_{\mathrm{tree}}&=2\log_2p\,(\alpha+n\beta)\\
T_{\mathrm{RD}}&=\log_2p\,(\alpha+n\beta)\\
T_{\mathrm{Rab}}&=2\log_2p\,\alpha+2\frac{p-1}{p}n\beta
\end{aligned}
{{< /katex >}}
{{% /colour %}}

The tree expression models a basic **unpipelined full-tensor reduce and broadcast along the critical path**. Optimised trees can behave differently.

| Algorithm | Sequential start-ups | Payload term divided by nβ |
|---|---:|---:|
| Ring | 2(p − 1) | 2(p − 1)/p |
| Basic tree | 2 log₂p | 2 log₂p |
| Recursive doubling | log₂p | log₂p |
| Rabenseifner | 2 log₂p | 2(p − 1)/p |

Small messages favour fewer start-ups. Large messages favour lower payload terms. Large worker counts can make a ring's start-up cost significant even for sizeable messages.

<!-- Source: slide 17, equations 12–15, visually checked against the slide. -->

## 8. Trees, Recursive Exchanges, and Rabenseifner

### Tree All-Reduce

In a tree reduction, parents combine children's tensors with their own and forward the result upwards. Broadcast distributes the result downwards.

The basic schedule takes approximately **2 log₂p** levels: about **20** for 1,024 workers, compared with a ring's **2,046** rounds. Actual trees use integer depths; the exact schedule depends on participant count and branching.

Chunk pipelining overlaps work at different levels. Double-tree designs split the payload between complementary trees to distribute internal-node responsibilities. The basic full-tensor tree formula should not be treated as the cost of every optimised implementation.

### Recursive Doubling

With a power-of-two participant count, ranks exchange full buffers with partners that differ in one rank bit: XOR 1, then XOR 2, then XOR 4, and so on. Each exchange doubles the set of contributions represented.

For eight workers, there are **three exchanges**, sending **3n bytes per worker**. This saves start-ups but repeatedly moves the full tensor.

### Recursive Halving

Recursive halving implements reduce-scatter. The active buffer halves each round, so eight workers send **n/2 + n/4 + n/8 = 7n/8** each. Each finishes with a fully reduced chunk.

### Rabenseifner's Algorithm

Combine recursive-halving reduce-scatter with a reverse, growing-chunk all-gather. This combines logarithmic start-ups with the ring's payload term.

For **eight workers and a 64 MB tensor**:

| Phase | Successive payload sizes | Total sent per worker |
|---|---|---:|
| Reduce-scatter | 32, 16, 8 MB | 56 MB |
| All-gather | 8, 16, 32 MB | 56 MB |
| Combined | Six rounds | 112 MB |

A ring sends the same **112 MB**, but needs **14 rounds**. Basic recursive schedules assume a power-of-two participant count; other counts require adaptations.

<!-- Sources: slides 22 and 29–30. Growing-chunk all-gather is distinguished from full-buffer recursive-doubling all-reduce. -->

## 9. Worked Example: Choose by Message Size

Use **16 workers**, α = **5 μs**, and β = **0.08 ns/byte**. This β corresponds to 12.5 GB/s, or 100 Gb/s. Use binary message sizes: **8 KiB = 8,192 bytes** and **64 MiB = 67,108,864 bytes**.

| Algorithm | 8 KiB | 64 MiB |
|---|---:|---:|
| Ring | 151.23 μs | 10.216 ms |
| Basic tree | 45.24 μs | 42.990 ms |
| Recursive doubling | 22.62 μs | 21.495 ms |
| Rabenseifner | 41.23 μs | 10.106 ms |

For the small message, recursive doubling wins because it pays only four start-ups. For the large message, ring and Rabenseifner move fewer bytes; Rabenseifner also uses fewer start-ups.

### Show the Ring Calculation

{{% colour "green" %}}
{{< katex display=true >}}
T_{\mathrm{ring}}=30(5\times10^{-6})+
1.875(8192)(0.08\times10^{-9})
\approx151.23\ \mu\mathrm{s}
{{< /katex >}}
{{% /colour %}}

### Find a Crossover

At p = 16, recursive doubling costs 4α + 4nβ; Rabenseifner costs 8α + 1.875nβ. Equating them gives:

{{% colour "green" %}}
{{< katex display=true >}}
n_* = \frac{4\alpha}{2.125\beta}\approx117{,}647\ \mathrm{bytes}
{{< /katex >}}
{{% /colour %}}

The predicted crossover is about **118 kB**, or **115 KiB**. It belongs to these parameters and cost models, rather than being a universal switching threshold.

For 1,024 workers and 64 MiB under the same model, the ring takes about **20.96 ms** and Rabenseifner about **10.83 ms**. The ring's many start-ups now add materially to the large-message cost.

<!-- Source: slide 31; values recomputed without mixing decimal and binary units. -->

## 10. Match Communication to Physical Topology

A logical communication graph runs over physical links. GPUs within one node may communicate much faster than GPUs in different nodes. Shared network uplinks may become bottlenecks even when individual device links are fast.

### Hierarchical All-Reduce

Reduce inside each node, perform an inter-node collective, and distribute the result locally. This uses fast local links before slower network links. A multi-lane version first reduce-scatters locally, exchanges separate slices across nodes, and all-gathers locally.

Fewer inter-node participants can reduce start-up costs. It does not imply that network bytes fall by exactly the number of GPUs per node; volume and time depend on the schedule and available links.

### Topology-Aware Variants

| Technique | Target setting | Main idea |
|---|---|---|
| FlexReduce | Asymmetric network hierarchy | Allocate tensor ranges according to subtree structure |
| Multi-lane all-reduce | Nodes with independent network lanes | Exchange separate tensor slices concurrently |
| Resilient All-Reduce Tree, RAT | Oversubscribed rack uplinks | Aggregate within a region before crossing shared links |

### FlexReduce: Unequal Groups

The illustrated hierarchy has two workers in one subtree and three in another. For a 12-entry tensor, the first subtree reduces **six entries per worker**, and the second reduces **four entries per worker**.

The later responsibility split assigns half the tensor to each subtree: the two-worker subtree owns **three entries each**, while the three-worker subtree owns **two entries each**. Workers fetch the other subtree's partial sums for their target ranges, then an all-gather reconstructs the full result.

In the illustrated cross-subtree reduction exchange, six partial-sum entries cross in each direction. This count excludes the later all-gather. The fractions reflect this particular hierarchy.

### Multi-Lane: Parallel Paths

With four independent lanes, a 1 GB payload can split into **250 MB per lane**. At 12.5 GB/s per lane, ideal transfer service time falls from **80 ms** on one lane to **20 ms** across four simultaneous lanes.

This isolates the inter-node payload transfer. End-to-end all-reduce also includes local collectives, start-ups, and any contention. Four lanes help only if their bandwidth can be sustained concurrently.

### RAT: Reduce Before Crossing the Core

For the illustrated topology of two racks with four workers each, compare bytes crossing the shared core:

| Illustrated scheme | Cross-core traffic |
|---|---:|
| Parameter server in rack 1 | 8n |
| Eight-worker ring with two rack crossings | 3.5n |
| Rack-local aggregation with RAT | 2n |

RAT sends one rack aggregate across and returns the result. These counts describe the illustrated layout, not every deployment. Resilience also depends on the algorithm's failure-handling mechanisms.

<!-- Sources: slides 21 and 24–28. Topology-dependent traffic is not presented as a universal performance guarantee. -->

## 11. Choosing and Evaluating a Strategy

| Situation | Candidate to investigate | What to measure |
|---|---|---|
| Tightly connected workers needing common updates | Synchronous SGD with all-reduce | Step time, stragglers, and model quality |
| Expensive global communication | Neighbour-based decentralised SGD | Mixing speed, data heterogeneity, and time to quality |
| Small gradient messages | Recursive doubling or a low-latency tree | Start-up overhead |
| Large tensors on regular links | Ring or Rabenseifner | Achieved bandwidth and worker count |
| Irregular or shared links | Hierarchical or topology-aware methods | Congestion and traffic over bottleneck links |

Map the workflow before tuning. Measure compute, data loading, communication, and waiting separately where possible; a fully busy CPU does not reveal which activity is useful training work.

Compare configurations with a clearly specified workload and batch-size regime. Inspect loss and validation quality alongside runtime, and distinguish warm-up or cold-start behaviour from steady-state performance. The best result is less time to the required quality at an acceptable resource cost.

<!-- Practical interpretation: transcript 00:38–00:46, 00:56–01:02, 01:11–01:12, and 01:27–01:30. Administrative instructions and assignment-specific requirements are excluded. -->

## 12. Common Mistakes

{{% hint warning %}}
- Confusing data shards with model partitions.
- Averaging unequal local mean losses without considering sample weights.
- Calling ring all-reduce decentralised SGD just because it has no parameter server.
- Adding chunks again during all-gather.
- Treating the basic tree model as a prediction for every pipelined tree.
- Mixing seconds, microseconds, bits, bytes, MB, and MiB in one calculation.
- Assuming a serverless collective automatically survives participant failure.
- Equating consensus of model values with convergence to an optimum.
{{% /hint %}}

## Practice Questions and Short Answers

These are concept checks and adaptations of the worked examples above, rather than past-paper questions.

1. **Three shards have sizes 600, 300, and 100, with mean losses 0.20, 0.50, and 0.90. Find the full-data mean loss.**
   - Answer: 0.6 × 0.20 + 0.3 × 0.50 + 0.1 × 0.90 = **0.36**.
2. **Why can all-reduce implement centralised SGD without a central server?**
   - Answer: the workers collectively obtain one common global gradient and apply the same update. Centralised optimisation describes the common-update semantics, not a mandatory physical server.
3. **Average values 3, 6, and 12, then apply a gradient of 2 with learning rate 0.5.**
   - Answer: the mixed value is 7, then the updated value is **6**.
4. **How many ring rounds are needed for eight workers?**
   - Answer: seven reduce-scatter rounds plus seven all-gather rounds: **14**.
5. **For eight workers and a 64 MB tensor, how many bytes does each worker send in ring all-reduce?**
   - Answer: 2 × 7/8 × 64 = **112 MB**. It also receives 112 MB.
6. **What advantage does Rabenseifner offer for the same example?**
   - Answer: the same 112 MB sent per worker in **six rounds**, rather than fourteen, under the basic power-of-two schedule.
7. **Why does recursive doubling win the 8 KiB example?**
   - Answer: start-up latency dominates; it uses four exchanges instead of the ring's thirty.
8. **Does a connected gossip graph guarantee immediate identical models?**
   - Answer: no. Neighbour averaging propagates information gradually, and local gradient steps can keep introducing disagreement.

## Key Takeaways

{{% hint success %}}
- The intended objective determines how local losses and gradients should be weighted.
- A common gradient update and neighbour-based model mixing are different training strategies.
- Ring all-reduce limits payload per worker; recursive methods can reduce sequential start-ups.
- Message size and physical topology determine which communication method is attractive.
- Validate communication improvements against runtime and model quality together.
{{% /hint %}}

## Checklist

- [ ] I can calculate a weighted global loss.
- [ ] I can distinguish gossip mixing from ring all-reduce.
- [ ] I can calculate one consensus and gradient update.
- [ ] I can explain reduce-scatter and all-gather.
- [ ] I can use the four α–β cost formulas with consistent units.
- [ ] I can state the assumptions behind a topology or speedup comparison.

---
{{< home-link "Home" >}} | {{< section-index >}}
