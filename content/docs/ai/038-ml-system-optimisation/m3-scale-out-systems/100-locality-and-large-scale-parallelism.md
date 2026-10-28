---
title: "Locality and Large-Scale Parallelism"
draft: false
tags: ["MLSysOps", "Parallelism", "GPU", "AI", "ML"]
categories: ["AI", "ML"]
weight: 1000
menu:
  main:
    identifier: mlsysops_ch100
    parent: mlsysops_m3
---

# Locality and Large-Scale Parallelism

Large models need efficient use of memory and coordinated computation across devices. This introductory overview covers:

- Locality-aware programs, massive multithreading, and GPGPUs
- Model parallelism and cross-device communication
- Pipeline parallelism, micro-batches, scheduling, and bubbles
- Large-scale GPU clusters and memory-efficient checkpointing

<!-- Introductory definitions following the supplied Chapter 10 outline; expand as detailed material becomes available. -->

## 1. Locality and GPU Execution

- **Locality-aware programs:** arrange computation to reuse nearby data before fetching it again from slower memory. Tiling a matrix operation helps reuse values held in cache or shared memory.
- **Massive multithreading:** keeps many lightweight threads available so a GPU can execute other ready work while some threads wait for data. Performance still depends on memory access and available resources.
- **GPGPU:** general-purpose computing on graphics processing units uses GPUs for tasks beyond graphics, including matrix operations and neural-network training.

## 2. Model Parallelism ☆

- **Model parallelism:** splits one model's computation or state across devices when one device is insufficient or a different execution layout is beneficial.
- **Layer slicing:** assigns different layers or groups of layers to different devices; intermediate activations move between those devices.
- **Tensor splitting:** divides an individual layer's large tensors and operations across devices, such as distributing parts of a matrix multiplication.
- **Cross-device communication:** transfers activations, gradients, or parameter information between devices. Its cost can limit the benefit of splitting the model.

## 3. Pipeline Parallelism

- **Pipeline parallelism:** divides the model into stages and overlaps different micro-batches across those stages, like an assembly line.
- **Micro-batches:** smaller pieces of a training batch that flow through the pipeline separately; their gradients can accumulate into a larger effective batch.
- **Scheduling:** decides when each stage performs forward and backward work. Examples include completing forward passes first or interleaving forward and backward passes.
- **Bubble overhead:** idle stage time during pipeline filling, draining, or imbalance. More balanced stages and suitable micro-batching can reduce it.

## 4. GPU Clusters and Checkpointing

- **Large-scale neural-network training:** coordinates GPUs across machines, often combining data, tensor, and pipeline parallelism. Network capacity, memory, and load balance all affect scaling.
- **Memory-efficient activation checkpointing:** saves selected intermediate activations and recomputes others during backpropagation, trading extra computation for lower training memory.
- **Recovery checkpoints:** save model, optimiser, and related training state so a run can resume after interruption; this serves a different purpose from activation checkpointing.

## References and Further Reading

- [NVIDIA: CUDA Programming Guide](https://docs.nvidia.com/cuda/cuda-programming-guide/)
- [PyTorch: pipeline parallelism](https://docs.pytorch.org/docs/stable/distributed.pipelining.html)
- [PyTorch: activation checkpointing](https://docs.pytorch.org/docs/stable/checkpoint.html)

---
{{< home-link "Home" >}} | {{< section-index >}}
