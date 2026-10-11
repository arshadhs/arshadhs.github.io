---
title: "Distributed Training Strategies"
draft: false
tags: ["ML System Optimization", "MLSysOps", "Distributed Training", "Data Parallelism", "Model Parallelism", "Pipeline Parallelism", "Gradient Checkpointing", "Mixed Precision", "AI", "ML"]
categories: ["AI", "ML"]
weight: 700
menu: main
---

# Distributed Training Strategies

Distributed training combines devices to increase throughput, fit larger models, or shorten time to solution. The central design question is what to partition: examples, model parameters, layers, tensors, or the training schedule.

Course coverage:

1. **Data parallelism**
2. **Model parallelism**
3. **Pipeline parallelism and micro-batches**
4. **Gradient checkpointing**
5. **Mixed-precision training and loss scaling**

## Learning Objectives

By the end of this page, you should be able to:

- compare data, model, and pipeline parallelism
- calculate global batch size, speedup, efficiency, and effective throughput
- explain gradient aggregation through all-reduce
- calculate simple pipeline time and utilisation
- quantify the memory–computation trade-off of checkpointing
- explain mixed precision and loss scaling

## Big Picture

{{< mermaid >}}
flowchart TD
    A["Training Constraint"] --> B["More Data Throughput"]
    A --> C["Model Too Large"]
    A --> D["Activation Memory Too High"]
    B --> E["Data Parallelism"]
    C --> F["Model or Pipeline Parallelism"]
    D --> G["Checkpointing or Mixed Precision"]

    style A fill:#E1F5FE
    style B fill:#C8E6C9
    style C fill:#FFF9C4
    style D fill:#EDE7F6
    style E fill:#E1F5FE
    style F fill:#C8E6C9
    style G fill:#FFF9C4
{{< /mermaid >}}

## 1. Data Parallelism ☆

Each worker holds a complete model replica and processes a different shard of the mini-batch.

For worker `i`:

{{% colour "green" %}}
{{< katex display=true >}}
g_i = \frac{1}{b_i}\sum_{x\in B_i}\nabla\ell(w;x)
{{< /katex >}}
{{% /colour %}}

For equal local batch sizes, gradients are averaged:

{{% colour "green" %}}
{{< katex display=true >}}
g = \frac{1}{p}\sum_{i=1}^{p}g_i
{{< /katex >}}
{{% /colour %}}

Every worker applies the same update and therefore retains the same parameter values.

### Global Batch Size

{{% colour "green" %}}
{{< katex display=true >}}
B_{\text{global}}=pB_{\text{local}}
{{< /katex >}}
{{% /colour %}}

With `8` workers and local batch size `32`, global batch size is `256`.

### All-Reduce

An all-reduce combines gradients across workers and returns the aggregate to every worker. It avoids a single central server but introduces a synchronisation point. Training step time is determined by the slowest worker plus the collective.

{{% colour "green" %}}
{{< katex display=true >}}
T_{\text{step}} \approx \max_i(T_{\text{compute},i}) + T_{\text{all-reduce}}
{{< /katex >}}
{{% /colour %}}

## 2. Model Parallelism ☆

Model parallelism divides model parameters or operations across devices. It is necessary when the model or its intermediate state does not fit on one device.

Two common forms are:

- **layer or stage partitioning:** consecutive groups of layers reside on different devices
- **tensor parallelism:** one large matrix or tensor operation is sliced across devices

Model parallelism communicates activations in the forward pass and activation gradients in the backward pass. It can reduce per-device parameter memory but introduces dependencies between partitions.

### Tensor-Slicing Example

For a matrix multiplication `Y = XW`, split the output columns of `W`:

{{% colour "green" %}}
{{< katex display=true >}}
W=[W_1\;W_2],
\qquad Y=[XW_1\;XW_2]
{{< /katex >}}
{{% /colour %}}

Two devices can compute `XW₁` and `XW₂` concurrently, after which the output slices are concatenated.

## 3. Pipeline Parallelism ☆

Pipeline parallelism assigns consecutive model stages to different devices and splits a training batch into `m` micro-batches. Once the pipeline is full, different stages work on different micro-batches at the same time.

For `s` balanced stages, `m` micro-batches, and per-stage time `t`, ideal forward pipeline time is:

{{% colour "green" %}}
{{< katex display=true >}}
T_{\text{pipeline}}=(m+s-1)t
{{< /katex >}}
{{% /colour %}}

Sequential stage execution would take `mst`, so ideal speedup is:

{{% colour "green" %}}
{{< katex display=true >}}
S=\frac{mst}{(m+s-1)t}=\frac{ms}{m+s-1}
{{< /katex >}}
{{% /colour %}}

Ideal stage utilisation is:

{{% colour "green" %}}
{{< katex display=true >}}
U=\frac{m}{m+s-1}
{{< /katex >}}
{{% /colour %}}

The unused slots during pipeline fill and drain form the **pipeline bubble**.

### Worked Numerical: Pipeline Bubble

Let `s = 4`, `m = 8`, and `t = 10 ms`.

{{% colour "green" %}}
{{< katex display=true >}}
T_{\text{pipeline}}=(8+4-1)10=110\text{ ms}
{{< /katex >}}
{{% /colour %}}

{{% colour "green" %}}
{{< katex display=true >}}
S=\frac{8\times4\times10}{110}\approx2.91,
\qquad U=\frac{8}{11}\approx72.7\%
{{< /katex >}}
{{% /colour %}}

Increasing micro-batches reduces the bubble fraction, but very small micro-batches may underutilise the accelerator and increase scheduling overhead.

## 4. Comparing Parallel Strategies

| Strategy | Partitioned Quantity | Main Communication | Best When | Main Limitation |
|---|---|---|---|---|
| Data parallel | Training examples | Gradients | Model fits on each device | Gradient synchronisation and global batch growth |
| Tensor parallel | Large tensor operations | Partial activations/results | Individual layers are too large | Frequent fine-grained communication |
| Pipeline parallel | Consecutive layer groups | Activations between stages | Model has balanced sequential stages | Pipeline bubbles and stage imbalance |
| Hybrid | Data, tensor, and stages | Several collectives | Very large models and clusters | Complex placement and tuning |

## 5. Gradient Checkpointing ☆

Backpropagation normally stores intermediate activations from the forward pass. For a deep model, activation memory may exceed parameter memory.

Gradient checkpointing stores only selected activations. During the backward pass, discarded activations are recomputed from the nearest saved checkpoint.

If a model has `L` equally sized layer activations of size `M_layer`, storing all activations costs:

{{% colour "green" %}}
{{< katex display=true >}}
M_{\text{full}}=L M_{\text{layer}}
{{< /katex >}}
{{% /colour %}}

If only `C` checkpoints are stored:

{{% colour "green" %}}
{{< katex display=true >}}
M_{\text{checkpoint}}\approx C M_{\text{layer}}
{{< /katex >}}
{{% /colour %}}

### Worked Numerical: Activation Memory

A `48`-layer model produces `20 MB` of saved activation per layer.

{{% colour "green" %}}
{{< katex display=true >}}
M_{\text{full}}=48\times20=960\text{ MB}
{{< /katex >}}
{{% /colour %}}

Storing `8` checkpoints uses approximately:

{{% colour "green" %}}
{{< katex display=true >}}
M_{\text{checkpoint}}=8\times20=160\text{ MB}
{{< /katex >}}
{{% /colour %}}

The approximate activation-memory reduction is `800 MB`, but the forward operations between checkpoints must be recomputed during backpropagation.

More checkpoints use more memory and less recomputation. Fewer checkpoints save more memory and require more recomputation.

## 6. Mixed-Precision Training ☆

Mixed precision uses lower precision for suitable tensor operations while retaining higher precision where numerical range or accumulation accuracy is important.

A common arrangement is:

- FP16 or BF16 for forward and backward tensor operations
- FP32 for master weights or sensitive accumulations
- conversion between representations around the optimiser update

Potential benefits include:

- lower parameter, gradient, and activation memory
- higher accelerator throughput
- lower memory-bandwidth demand

### Memory Numerical

One billion FP32 values require:

{{% colour "green" %}}
{{< katex display=true >}}
10^9\times4\text{ bytes}=4\text{ GB}
{{< /katex >}}
{{% /colour %}}

The same number of FP16 values require `2 GB`. Actual training memory includes weights, gradients, optimiser states, and activations, so the complete reduction may not be exactly one half.

## 7. Loss Scaling

FP16 has limited dynamic range. Very small gradients can underflow to zero. Loss scaling multiplies the loss by scale `S` before backpropagation:

{{% colour "green" %}}
{{< katex display=true >}}
L'=SL,
\qquad \nabla L'=S\nabla L
{{< /katex >}}
{{% /colour %}}

Before the optimiser update, gradients are divided by `S`:

{{% colour "green" %}}
{{< katex display=true >}}
g=\frac{g'}{S}
{{< /katex >}}
{{% /colour %}}

A scale that is too small may not prevent underflow; a scale that is too large may cause overflow. Dynamic loss scaling adjusts the scale after checking for invalid values.

### Worked Numerical: Loss Scaling

If a gradient is `3 × 10⁻⁸` and `S = 1024`, the scaled gradient is:

{{% colour "green" %}}
{{< katex display=true >}}
3\times10^{-8}\times1024=3.072\times10^{-5}
{{< /katex >}}
{{% /colour %}}

After safe computation, dividing by `1024` restores the original mathematical gradient.

## 8. Speedup, Efficiency, and Overhead

{{% colour "green" %}}
{{< katex display=true >}}
S_p=\frac{T_1}{T_p},
\qquad E_p=\frac{S_p}{p}
{{< /katex >}}
{{% /colour %}}

Parallel overhead expressed in processor-time units is:

{{% colour "green" %}}
{{< katex display=true >}}
T_o=pT_p-T_1
{{< /katex >}}
{{% /colour %}}

### Worked Numerical: Training Run

A training job takes `480` minutes on one GPU and `75` minutes on eight GPUs.

{{% colour "green" %}}
{{< katex display=true >}}
S_8=\frac{480}{75}=6.4,
\qquad E_8=\frac{6.4}{8}=0.8
{{< /katex >}}
{{% /colour %}}

{{% colour "green" %}}
{{< katex display=true >}}
T_o=8(75)-480=120\text{ GPU-minutes}
{{< /katex >}}
{{% /colour %}}

The run achieves `80%` efficiency and incurs `120 GPU-minutes` of parallel overhead.

## 9. Strategy Selection

Use data parallelism when the model fits on one device and throughput is the goal. Use tensor or layer partitioning when individual operations or the model exceed one device. Add pipelining when sequential layer groups can be balanced. Use checkpointing when activation memory is the immediate constraint, and mixed precision when the hardware supports efficient lower-precision arithmetic.

These techniques can be combined, but every added dimension of parallelism creates another communication and scheduling surface.

## Common Mistakes

{{% hint warning %}}
- Calling data parallelism model parallelism because several model replicas exist.
- Forgetting that global batch size grows with worker count.
- Assuming pipeline speedup equals the number of stages without accounting for bubbles.
- Treating checkpointing as free memory reduction; it adds recomputation.
- Assuming every training value can safely use FP16.
- Applying loss scaling without unscaling gradients before the update.
{{% /hint %}}

## Practice Questions

1. Sixteen workers use local batch size `24`. Find global batch size.
2. Explain how an all-reduce keeps data-parallel replicas consistent.
3. Compare tensor parallelism with pipeline parallelism.
4. For `s = 6`, `m = 18`, and `t = 5 ms`, find ideal forward pipeline time and utilisation.
5. A model has `60` layers with `12 MB` of activation per layer. Compare full storage with `10` checkpoints.
6. Why are master weights often retained in FP32?
7. A job takes `900` seconds on one GPU and `140` seconds on eight GPUs. Calculate speedup, efficiency, and parallel overhead.
8. What trade-off is controlled by the number of pipeline micro-batches?

## Key Takeaways

{{% hint success %}}
- Data parallelism partitions examples and synchronises gradients.
- Model parallelism partitions parameters or tensor operations.
- Pipeline parallelism overlaps micro-batches across layer stages but creates bubbles.
- Gradient checkpointing exchanges activation memory for recomputation.
- Mixed precision reduces memory and may increase throughput, but numerical range must be managed.
- Speedup alone is incomplete; efficiency and parallel overhead show resource cost.
{{% /hint %}}

## Checklist

- [ ] I can calculate global batch size.
- [ ] I can compare data, tensor, and pipeline parallelism.
- [ ] I can calculate ideal pipeline time and utilisation.
- [ ] I can quantify checkpoint memory savings.
- [ ] I can explain mixed precision and loss scaling.
- [ ] I can calculate speedup, efficiency, and parallel overhead.

---
{{< home-link "Home" >}} | {{< section-index >}}
