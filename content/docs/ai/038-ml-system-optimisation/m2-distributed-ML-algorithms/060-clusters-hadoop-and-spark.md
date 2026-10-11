---
title: "Clusters, Hadoop, and Spark"
draft: false
tags: ["ML System Optimization", "MLSysOps", "Clusters", "Hadoop", "Spark", "MapReduce", "Distributed K-Means", "Distributed CNN", "AI", "ML"]
categories: ["AI", "ML"]
weight: 600
menu: main
---

# Clusters, Hadoop, and Spark

Scale-out frameworks distribute data and computation across networked machines. Their value comes from aggregate compute and storage, while their main costs are communication, scheduling, serialisation, and repeated synchronisation.

Course coverage:

1. **Cluster computing with Hadoop and Spark**
2. **MapReduce k-means**, mini-batch and streaming variants
3. **Convergence and communication reduction**
4. **Distributed convolutional neural networks:** data and model parallelism
5. **Layer or tensor slicing, mixed precision, pipelining, and distributed execution frameworks**

## Learning Objectives

By the end of this page, you should be able to:

- compare Hadoop MapReduce with Spark-style in-memory execution
- map iterative k-means onto distributed stages
- explain why iteration and shuffle costs matter
- identify strong and weak scaling behaviour
- locate parallelism inside a convolution and across CNN training examples
- estimate communication and cluster efficiency

## 1. Cluster Computing

A cluster contains multiple networked machines or **nodes**. Each node has private processors and memory; communication occurs through the network.

| Component | Role |
|---|---|
| Distributed storage | Partitions and replicates datasets across nodes |
| Scheduler | Assigns tasks close to data and balances resources |
| Worker process | Runs map, reduce, training, or inference tasks |
| Shuffle or collective layer | Exchanges intermediate records, gradients, or statistics |
| Driver or coordinator | Constructs the job and tracks progress |

### Strong and Weak Scaling ☆

**Strong scaling** keeps total problem size fixed while increasing processors. Ideal time is:

{{% colour "green" %}}
{{< katex display=true >}}
T_p \approx \frac{T_1}{p}
{{< /katex >}}
{{% /colour %}}

**Weak scaling** increases total problem size in proportion to processors, keeping work per processor roughly constant. Ideal execution time remains approximately constant.

### Worked Numerical: Strong Scaling

A job takes `800` seconds on one node and `125` seconds on eight nodes.

{{% colour "green" %}}
{{< katex display=true >}}
S_8=\frac{800}{125}=6.4,
\qquad E_8=\frac{6.4}{8}=0.8
{{< /katex >}}
{{% /colour %}}

The eight-node efficiency is `80%`.

## 2. Hadoop MapReduce and Spark

Hadoop MapReduce is designed for robust batch processing over distributed storage. Intermediate stages are commonly materialised, which supports recovery but adds disk and serialisation cost.

Spark retains reusable working data in memory when possible and represents a computation as a directed acyclic graph of transformations and actions. It is therefore well suited to iterative ML workloads that repeatedly access the same dataset.

| Property | Hadoop MapReduce | Spark-Style Execution |
|---|---|---|
| Main orientation | Durable batch jobs | General dataflow and iterative analytics |
| Intermediate data | Frequently materialised | Can be cached in memory |
| Iterative algorithms | Repeated job and storage overhead | Reuses cached partitions |
| Recovery | Re-execute or reload materialised stages | Recompute lost partitions from lineage |
| Main cost | Disk I/O and shuffle | Memory pressure, shuffle, and cluster cost |

{{% hint info %}}
Spark does not remove network communication. It mainly avoids unnecessary repeated storage I/O when data can remain cached.
{{% /hint %}}

## 3. MapReduce k-Means ☆

One k-means iteration can be expressed as:

1. distribute current centroids;
2. **map:** assign local points and emit partial sums and counts keyed by cluster;
3. **shuffle:** group partial statistics by cluster identifier;
4. **reduce:** add sums and counts, then compute new centroids;
5. test convergence and repeat.

{{< mermaid >}}
flowchart TD
    A["Data Partitions"] --> B["Map: Assign Points"]
    B --> C["Shuffle: Group by Cluster"]
    C --> D["Reduce: Sums and Counts"]
    D --> E["New Centroids"]
    E -->|"Not converged"| B

    style A fill:#E1F5FE
    style B fill:#C8E6C9
    style C fill:#FFF9C4
    style D fill:#EDE7F6
    style E fill:#E1F5FE
{{< /mermaid >}}

### Communication Reduction

A combiner can aggregate all points assigned to a cluster within one mapper before the shuffle. Instead of emitting every point, it emits one sum and count per non-empty cluster.

If a mapper processes `r` points of `d` dimensions, raw emission is proportional to `rd`, while combined emission is proportional to `kd`.

### Worked Numerical: Combiner Benefit

A mapper processes `1,000,000` points with `d = 20` and `k = 100`. Ignoring keys and counts:

{{% colour "green" %}}
{{< katex display=true >}}
\frac{rd}{kd}=\frac{1000000\times20}{100\times20}=10000
{{< /katex >}}
{{% /colour %}}

Local aggregation can reduce the number of transmitted numeric values by a factor of about `10,000` for that mapper.

## 4. Mini-Batch and Streaming k-Means

Mini-batch k-means processes a small sample rather than the full dataset in every update. Streaming variants update centroids as data arrive.

Advantages include:

- lower per-update latency
- fewer processed records per update
- suitability for data streams or datasets that exceed memory
- more frequent, smaller updates

Trade-offs include noisier centroids, sensitivity to batch order, and an approximate final solution. A smaller batch is not automatically faster in a distributed setting if it leaves workers underutilised or makes synchronisation too frequent.

## 5. Distributed Random Forests in Data Frameworks

Random forests map naturally to clusters because trees are largely independent. Frameworks can distribute tree groups, bootstrap samples, or feature evaluation.

A MapReduce interpretation is:

- **map:** train trees or compute candidate-split statistics on partitions;
- **reduce:** collect trees, merge statistics, or aggregate predictions.

Spark-style execution is useful when training repeatedly reuses cached data and split statistics. Hadoop-style execution is useful when durability and offline batch processing are more important than low iteration latency.

## 6. Convolutional Neural Network Parallelism

For a two-dimensional convolution with output height `H_o`, output width `W_o`, input channels `C_i`, output channels `C_o`, and kernel size `K_h × K_w`, the multiply-accumulate count is approximately:

{{% colour "green" %}}
{{< katex display=true >}}
H_oW_oC_oC_iK_hK_w
{{< /katex >}}
{{% /colour %}}

Each output position and output channel exposes regular parallel work, making convolutions well suited to GPUs.

### Worked Numerical: Convolution Work

For `H_o = W_o = 28`, `C_i = 32`, `C_o = 64`, and a `3 × 3` kernel:

{{% colour "green" %}}
{{< katex display=true >}}
28\times28\times64\times32\times3\times3
=14450688
{{< /katex >}}
{{% /colour %}}

The layer performs about `14.45 million` multiply-accumulate operations per example.

## 7. Distributed CNN Strategies

### Data Parallelism

Each device stores the complete model and processes a different mini-batch shard. Gradients are aggregated after backpropagation.

- simple when the model fits on one device
- increases global batch size
- communicates model-sized gradients each step

### Model, Layer, and Tensor Parallelism

The model is partitioned across devices. A partition may contain different layers, output channels, or slices of a weight tensor.

- enables models that exceed one device's memory
- communicates activations between partitions
- requires careful balancing because slow partitions stall the rest

### Pipeline Execution

Layer partitions form stages, and a batch is divided into micro-batches. Different stages process different micro-batches concurrently. Pipeline bubbles occur while stages fill, drain, or wait for an imbalanced neighbour.

### Mixed Precision

Lower-precision arithmetic reduces tensor memory and may increase accelerator throughput. Selected values, such as master weights or accumulation, remain in higher precision. Loss scaling helps prevent small FP16 gradients from underflowing.

## 8. End-to-End Performance Model

One distributed iteration can be modelled as:

{{% colour "green" %}}
{{< katex display=true >}}
T_{\text{iter}} = T_{\text{input}}
 + T_{\text{forward}}
 + T_{\text{backward}}
 + T_{\text{communication}}
 + T_{\text{update}}
 + T_{\text{waiting}}
{{< /katex >}}
{{% /colour %}}

Optimising only accelerator arithmetic may have little effect if input, shuffle, or collective communication dominates.

## Common Mistakes

{{% hint warning %}}
- Assuming that a cluster behaves like one shared-memory machine.
- Treating Spark as communication-free because data are cached.
- Emitting every k-means point instead of compact local sums and counts.
- Ignoring repeated shuffle cost across iterative algorithms.
- Confusing intra-layer CNN parallelism with splitting complete examples across devices.
- Using a global batch size that changes optimisation behaviour without retuning.
{{% /hint %}}

## Practice Questions

1. Distinguish strong scaling from weak scaling.
2. A job takes `600` seconds on one node and `90` seconds on eight nodes. Calculate speedup and efficiency.
3. Why can Spark execute iterative ML workloads more efficiently than disk-oriented MapReduce?
4. Describe map, shuffle, and reduce in one k-means iteration.
5. Explain how a combiner reduces k-means communication.
6. Calculate convolution work for `H_o = W_o = 14`, `C_i = 64`, `C_o = 128`, and a `3 × 3` kernel.
7. Compare data parallelism with tensor or layer parallelism for CNN training.
8. What causes a pipeline bubble?

## Key Takeaways

{{% hint success %}}
- Cluster speedup is limited by network, storage, scheduling, and imbalance.
- Hadoop MapReduce emphasises durable batch stages; Spark can reuse cached partitions.
- Distributed k-means maps assignment to map tasks and centroid aggregation to reduce tasks.
- Combiners reduce shuffle volume by sending local statistics rather than raw points.
- CNNs expose massive regular parallelism across output positions, channels, and examples.
- End-to-end performance includes input, compute, communication, update, and waiting time.
{{% /hint %}}

## Checklist

- [ ] I can calculate cluster speedup and efficiency.
- [ ] I can compare Hadoop MapReduce and Spark-style execution.
- [ ] I can explain a distributed k-means iteration.
- [ ] I can calculate convolution work.
- [ ] I can compare distributed CNN strategies.

---
{{< home-link "Home" >}} | {{< section-index >}}
