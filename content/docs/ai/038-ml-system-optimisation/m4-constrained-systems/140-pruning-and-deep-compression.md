---
title: "Pruning and Deep Compression"
draft: false
tags: ["MLSysOps", "Pruning", "Deep Compression", "AI", "ML"]
categories: ["AI", "ML"]
weight: 1400
menu:
  main:
    identifier: mlsysops_ch140
    parent: mlsysops_m4
---

# Pruning and Deep Compression

Pruning removes selected parts of a network, while deep compression combines multiple ways to store it more compactly. This introductory overview covers:

- GPU CNN execution and federated learning
- Structured and unstructured pruning
- Pruning schedules and evaluation metrics
- Compression pipelines and sparse storage

## 1. Execution Context

- **GPU implementation of CNNs:** uses parallel kernels for convolution and other operations. Irregular sparsity helps execution only if the kernels and hardware can exploit it.
- **Federated learning:** trains across participants holding local data; smaller models or updates can reduce communication, but compression may affect convergence and participant compatibility.

## 2. Pruning Methods ☆

- **Unstructured magnitude-based pruning:** zeros individual weights with small absolute values. It produces irregular sparsity without necessarily changing tensor dimensions.
- **Structured filter/channel pruning:** removes whole filters, channels, or other groups, potentially producing smaller dense operations that ordinary hardware can execute efficiently.
- **One-shot pruning:** applies the chosen pruning operation in one stage, usually followed by fine-tuning to recover quality.
- **Iterative pruning:** alternates smaller pruning steps with retraining or fine-tuning, giving the model opportunities to adapt gradually.

## 3. Evaluation Metrics

- **Sparsity ratio:** the fraction of weights that are zero. For example, 800 zero weights out of 1,000 means 80% sparsity.
- **FLOPs reduction:** the decrease in estimated floating-point operations. State whether the estimate counts dense operations or assumes sparse operations are skipped.
- **Accuracy drop:** the reduction in prediction accuracy after compression; a change from 90% to 88% is a drop of two percentage points.

Parameter count and FLOPs do not determine actual runtime alone. Measure latency, memory, and model quality on the intended hardware.

## 4. Deep Compression and Sparse Storage

- **Combining pruning, quantisation, and encoding:** pruning removes connections, quantisation reduces distinct values or precision, and entropy encoding compresses the resulting representation.
- **Fine-tuning:** updates the retained parameters after a transformation, aiming to recover quality lost during compression.
- **Sparse matrix formats:** store nonzero values with location information instead of every zero. COO records coordinates; CSR stores values, column indices, and row boundaries.
- **Storage overhead:** sparse indices consume space, so sparse storage is advantageous only when the saved zeros outweigh its metadata and format costs.

{{< mermaid >}}
flowchart TD
    A["Trained network"] --> B["Prune and fine-tune"]
    B --> C["Quantise or share weights"]
    C --> D["Encode for storage"]
    D --> E["Check size and quality"]
    style A fill:#E1F5FE
    style B fill:#FFF9C4
    style C fill:#C8E6C9
    style D fill:#EDE7F6
    style E fill:#E1F5FE
{{< /mermaid >}}

This illustrates the classic compression sequence; deployment still needs a supported execution representation.

## References and Further Reading

- [Han et al.: Deep Compression](https://arxiv.org/abs/1510.00149)
- [TensorFlow: weight pruning](https://www.tensorflow.org/model_optimization/guide/pruning)
- [PyTorch: sparse tensor formats](https://docs.pytorch.org/docs/stable/sparse.html)
- [McMahan et al.: federated learning](https://arxiv.org/abs/1602.05629)

---
{{< home-link "Home" >}} | {{< section-index >}}
