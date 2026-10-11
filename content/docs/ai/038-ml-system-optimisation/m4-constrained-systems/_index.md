---
title: "M4: Constrained Systems"
draft: false
tags: ["ML System Optimisation", "MLSysOps", "Constrained Systems", "Model Compression", "Quantisation", "Pruning", "Edge Deployment", "AI", "ML"]
categories: ["AI", "ML"]
weight: 400
menu:
  main:
    identifier: mlsysops_m4
    parent: mlsysops
bookCollapseSection: true
---

# M4: Constrained Systems

This module considers how to make models practical when memory, compute, latency, and energy are limited. It connects model compression with edge deployment and the trade-offs required to retain useful prediction quality.

Module coverage:

- **Model Optimisation Techniques:** model compression, structured and unstructured pruning, post-training quantisation, and quantisation-aware training.
- **Edge Deployment:** TinyML, TensorFlow Lite, ONNX Runtime, and knowledge distillation.
- **Performance Trade-offs:** accuracy versus latency, model size versus throughput, and energy consumption optimisation.

<!-- Coverage follows _COURSE_MLSysOps_ZG516.pdf, Content Structure on page 3; topic boundaries follow the Learning Plan on pages 6–7. -->

## Learning Objectives

- Compare methods for reducing model size, computation, and memory use.
- Distinguish different quantisation and pruning approaches.
- Explain the role of knowledge distillation and deployment frameworks.
- Evaluate accuracy, latency, throughput, model size, and energy together.

## Planned Topics

| Chapter | Topic | Availability |
|---:|---|---|
| 12 | Model Compression Foundations | Planned |
| 13 | Quantisation and Edge Intelligence | Planned |
| 14 | Pruning and Deep Compression | Planned |
| 15 | Optimisation Landscape and Small Language Models | Planned |
| 16 | Energy-Constrained ML | Planned |

This index records the module scope. Chapter pages can be added here as their source material becomes available.

## Topic Outline

### 12. Model Compression Foundations

- GPU implementation of CNNs
- Quantisation, network pruning, and low-rank factorisation
- Storage, memory, and bandwidth savings
- Lossy and lossless compression
- Weight sharing, HashNet, weight clustering, and encoding
- Huffman and arithmetic coding
- Knowledge distillation: teacher–student models, temperature, soft labels, and cost functions

### 13. Quantisation and Edge Intelligence ☆

- Continued model compression and knowledge distillation
- Edge intelligence case studies
- Uniform and non-uniform quantisation
- Post-training quantisation versus quantisation-aware training
- Reduced precision: 16-bit, 8-bit, binary, and ternary arithmetic
- Effects on forward and backward passes
- Hardware support through vector units, bfloat16, and MCU DSP blocks

### 14. Pruning and Deep Compression

- GPU implementation of CNNs and federated learning
- Unstructured magnitude-based pruning and structured filter/channel pruning
- Iterative and one-shot pruning
- Sparsity ratio, FLOPs reduction, and accuracy drop
- Combining pruning, quantisation, and encoding
- Sparse matrix storage formats

### 15. Optimisation Landscape and Small Language Models

- Continued federated learning
- Generic and specific optimisation approaches
- Algorithm-specific and target-platform-specific optimisation
- ML frameworks, libraries, hardware, and compilers
- Small language models and TensorFlow Lite Micro

### 16. Energy-Constrained ML

- Adapting algorithms to energy and resource constraints
- Prediction accuracy, model size, throughput, and response time
- Energy consumption and performance trade-offs

## How This Module Connects

[M1: Foundations]({{< relref "../M1/_index.md" >}}) supplies the performance metrics, while [M3: Scale-Out Systems]({{< relref "../M3/_index.md" >}}) provides the larger-system context. Here the emphasis shifts towards fitting useful models within tighter resource budgets.

[ML System Optimisation overview]({{< relref "../_index.md" >}})

---
{{< home-link "Home" >}} | {{< section-index >}}
