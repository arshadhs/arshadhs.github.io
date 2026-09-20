---
title: "Communication-Aware Distributed ML"
draft: false
tags: ["ML System Optimization", "MLSysOps", "Distributed Machine Learning", "K-Means", "KNN", "Gradient Descent", "SGD", "Communication", "AI", "ML"]
categories: ["AI", "ML"]
weight: 500
menu: main
---

# Communication-Aware Distributed ML

Distributed machine learning is effective only when saved computation exceeds the cost of moving data and synchronising workers. Algorithm design must therefore account for message size, message frequency, barriers, and parameter placement.

Course coverage:

1. **Communication overhead**, illustrated through distributed k-means
2. **Model parallelism** when parameters do not fit or compute must be divided
3. **Distributed k-nearest neighbours:** partitioning, indexing, and approximate search
4. **Gradient descent, SGD, and mini-batch optimisation**
5. **Synchronous versus asynchronous updates**

## Learning Objectives

By the end of this page, you should be able to:

- model communication using latency and bandwidth
- explain communication in distributed k-means
- compare Lloyd, Elkan, mini-batch, Hartigan–Wong, and parallel k-means
- explain exact and approximate distributed nearest-neighbour search
- derive a data-parallel gradient update
- compare synchronous and asynchronous SGD

## 1. Communication Cost Model ☆

For a message containing `m` bytes:

{{% colour "green" %}}
{{< katex display=true >}}
T_{\text{comm}} = \alpha + \frac{m}{\beta}
{{< /katex >}}
{{% /colour %}}

where `α` is per-message latency and `β` is effective bandwidth. For `q` messages, a simple model is:

{{% colour "green" %}}
{{< katex display=true >}}
T_{\text{total comm}} = q\alpha + \frac{M}{\beta}
{{< /katex >}}
{{% /colour %}}

where `M` is the total number of transferred bytes.

This reveals two optimisation strategies:

- aggregate small messages to reduce the number of latency payments
- reduce total bytes through compact statistics, compression, or less frequent synchronisation

### Worked Numerical: Message Transfer

A worker sends `40 MB` over an effective `1 GB/s` link with `2 ms` start-up latency.

{{% colour "green" %}}
{{< katex display=true >}}
T_{\text{comm}} = 0.002 + \frac{40}{1000}
                  = 0.042\text{ s}
{{< /katex >}}
{{% /colour %}}

Sending the same total data as `40` one-megabyte messages costs:

{{% colour "green" %}}
{{< katex display=true >}}
40(0.002) + \frac{40}{1000} = 0.12\text{ s}
{{< /katex >}}
{{% /colour %}}

The transferred bytes are unchanged, but repeated latency nearly triples the time.

## 2. Communication in Distributed k-Means ☆

Each worker holds a shard of the points and a copy of the `k` centroids. During an iteration:

1. broadcast the current centroids;
2. assign local points to their nearest centroid;
3. produce local sums and counts;
4. reduce the local statistics;
5. compute and redistribute new centroids.

For `p` workers and `d`-dimensional centroids, the communicated model state is proportional to `kd`, not `nd`. This is efficient when `n` is much larger than `k`.

If every centroid coordinate and count uses `b` bytes, one worker's local statistics require approximately:

{{% colour "green" %}}
{{< katex display=true >}}
M_{\text{worker}} = k(d+1)b
{{< /katex >}}
{{% /colour %}}

### Worked Numerical: k-Means Reduction

Let `k = 100`, `d = 64`, `p = 16`, and `b = 4` bytes.

{{% colour "green" %}}
{{< katex display=true >}}
M_{\text{worker}} = 100(64+1)4 = 26000\text{ bytes}
{{< /katex >}}
{{% /colour %}}

Across all workers, raw local statistics total `416,000` bytes per iteration before considering the collective implementation. The cost repeats for every iteration, so convergence rate directly affects communication volume.

## 3. k-Means Optimisation Methods

All variants minimise the within-cluster sum of squares:

{{% colour "green" %}}
{{< katex display=true >}}
J = \sum_{j=1}^{k}\sum_{x_i\in C_j}\lVert x_i-\mu_j\rVert^2
{{< /katex >}}
{{% /colour %}}

| Method | Main Idea | Performance Trade-off |
|---|---|---|
| Lloyd | Full assignment and full centroid update | Simple and exact per iteration, but scans all points |
| Elkan | Triangle-inequality bounds avoid some distance calculations | Faster when bounds prune well; additional bound storage |
| Mini-batch | Update from a small sampled batch | Lower iteration cost and communication; approximate result |
| Hartigan–Wong | Move individual points when the objective improves | Can find strong local solutions; point-wise updates are harder to parallelise |
| Shared-memory parallel Lloyd | Split rows among processes or threads | Accelerates assignment; centroid reduction and process overhead remain |

Mini-batch k-means is communication-aware because smaller sampled updates reduce computation and exchanged statistics. Its result may differ from full-batch Lloyd k-means.

## 4. Model Parallelism

In model parallelism, different workers own different model parameters or layers. It is used when a model is too large for one device or when separate components expose useful computation.

For two consecutive partitions:

{{% colour "green" %}}
{{< katex display=true >}}
h = f_1(x;\theta_1),
\qquad y=f_2(h;\theta_2)
{{< /katex >}}
{{% /colour %}}

The intermediate activation `h` crosses the device boundary in the forward pass, and its gradient crosses back during backpropagation. A poor partition may save parameter memory but create heavy activation communication.

{{% hint info %}}
Data parallelism communicates parameter gradients. Model parallelism communicates activations and activation gradients between partitions. The smaller communication surface depends on the model shape and batch size.
{{% /hint %}}

## 5. Distributed k-Nearest Neighbours

k-nearest neighbours stores training examples rather than learning a compact parametric model. For one query, brute-force exact search over `n` points with `d` features costs approximately `O(nd)`.

With `p` partitions:

1. broadcast the query to every partition;
2. each worker finds its local top `k` neighbours;
3. send only local candidates to a coordinator;
4. merge at most `pk` candidates into the global top `k`.

This avoids moving entire partitions for each query, but every worker still scans its data unless an index is used.

### Exact and Approximate Search

| Approach | Benefit | Cost |
|---|---|---|
| Exact partition scan | Exact global neighbours | High latency and full-partition work |
| Tree or space index | Prunes search in suitable dimensions | Index build, storage, and weaker pruning in high dimensions |
| Locality-sensitive hashing | Fast approximate candidate retrieval | May miss true neighbours |

Approximate search trades some recall or accuracy for lower latency and reduced work.

## 6. Gradient Descent and Mini-Batch SGD ☆

For parameters `w`, learning rate `η`, and objective `L(w)`:

{{% colour "green" %}}
{{< katex display=true >}}
w_{t+1}=w_t-\eta\nabla L(w_t)
{{< /katex >}}
{{% /colour %}}

In data-parallel training, worker `i` processes local mini-batch `B_i`:

{{% colour "green" %}}
{{< katex display=true >}}
g_i = \frac{1}{|B_i|}\sum_{x\in B_i}\nabla\ell(w_t;x)
{{< /katex >}}
{{% /colour %}}

For equal batch sizes, the global gradient is:

{{% colour "green" %}}
{{< katex display=true >}}
g = \frac{1}{p}\sum_{i=1}^{p}g_i
{{< /katex >}}
{{% /colour %}}

Each worker then applies the same update `w_{t+1}=w_t-ηg`.

### Global Batch Size

If each of `p` workers uses local batch size `b`:

{{% colour "green" %}}
{{< katex display=true >}}
B_{\text{global}}=pb
{{< /katex >}}
{{% /colour %}}

For `p = 8` and `b = 64`, the global batch size is `512`. Increasing worker count without adjusting `b` changes the optimisation behaviour as well as system throughput.

## 7. Synchronous and Asynchronous SGD ☆

### Synchronous SGD

Every worker computes a gradient for the same model version. Training waits for all workers, aggregates gradients, and performs one update.

- deterministic model version per step
- straightforward averaging
- vulnerable to a slow worker, called a **straggler**

### Asynchronous SGD

Workers send gradients and receive parameters without a global barrier. Fast workers do not wait, but a gradient may have been computed using an older parameter vector.

If a gradient used version `w_{t-\tau}`, its staleness is `τ` updates:

{{% colour "green" %}}
{{< katex display=true >}}
w_{t+1}=w_t-\eta g(w_{t-\tau})
{{< /katex >}}
{{% /colour %}}

| Property | Synchronous | Asynchronous |
|---|---|---|
| Barrier | Every step | No global step barrier |
| Straggler effect | High | Lower |
| Parameter consistency | Current shared version | Gradients may be stale |
| Convergence reasoning | Simpler | Learning rate and staleness require care |

## 8. Compute-to-Communication Ratio

A useful scalability indicator is:

{{% colour "green" %}}
{{< katex display=true >}}
R = \frac{T_{\text{compute}}}{T_{\text{communication}}}
{{< /katex >}}
{{% /colour %}}

Large `R` means communication is relatively easy to hide or amortise. Small `R` indicates that adding workers is unlikely to help without changing batch size, message frequency, compression, or partitioning.

## Common Mistakes

{{% hint warning %}}
- Counting only transferred bytes while ignoring message latency.
- Sending raw k-means points when sums and counts are sufficient.
- Treating mini-batch k-means as numerically identical to full-batch Lloyd k-means.
- Ignoring activation transfer in model parallelism.
- Increasing data-parallel workers without noticing the changed global batch size.
- Assuming asynchronous SGD removes communication cost or always converges faster.
{{% /hint %}}

## Practice Questions

1. A `100 MB` message uses a `2 GB/s` link with `5 ms` latency. Find transfer time.
2. Explain why many small messages can be slower than one large message with the same total bytes.
3. For `k = 50`, `d = 32`, and four-byte values, calculate one worker's k-means sum-and-count payload.
4. Compare Lloyd, Elkan, and mini-batch k-means.
5. Explain how a distributed exact k-NN query combines local results.
6. Eight workers each use local batch size `128`. Find global batch size.
7. Define gradient staleness and explain its effect.
8. A step spends `80 ms` computing and `20 ms` communicating. Find its compute-to-communication ratio and the fraction of step time spent communicating.

## Key Takeaways

{{% hint success %}}
- Communication cost contains both latency and bandwidth terms.
- Distributed k-means exchanges compact sums and counts at every iteration.
- Mini-batches reduce work and communication at the cost of approximation.
- Model parallelism can replace parameter-memory pressure with activation communication.
- Distributed k-NN merges small local candidate sets, but exact search can remain expensive.
- Synchronous SGD waits for all workers; asynchronous SGD accepts stale gradients.
{{% /hint %}}

## Checklist

- [ ] I can calculate message-transfer time.
- [ ] I can describe one distributed k-means iteration.
- [ ] I can compare the principal k-means variants.
- [ ] I can calculate global batch size.
- [ ] I can compare synchronous and asynchronous SGD.

---
{{< home-link "Home" >}} | {{< section-index >}}
