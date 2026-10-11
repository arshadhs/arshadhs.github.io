---
title: "M3: Scale-Out Systems"
draft: false
tags: ["ML System Optimisation", "MLSysOps", "Scale-Out Systems", "Parameter Server", "Distributed Deep Learning", "Federated Learning", "AI", "ML"]
categories: ["AI", "ML"]
weight: 300
menu:
  main:
    identifier: mlsysops_m3
    parent: mlsysops
bookCollapseSection: true
---

# M3: Scale-Out Systems

This module examines the platforms, communication patterns, and hardware organisation used to train across larger systems. It extends distributed algorithms into coordinated training services and federated settings.

Module coverage:

- **ML Platforms and Frameworks:** the Parameter Server model, Spark MLlib, and TensorFlow/PyTorch distributed training.
- **Distributed Deep Learning:** decentralised SGD, all-reduce, asynchronous parallelism, and large-scale neural networks.
- **Federated Learning:** training that preserves data privacy, communication efficiency, and hardware utilisation.

<!-- Coverage follows _COURSE_MLSysOps_ZG516.pdf, Content Structure on page 3; topic boundaries follow the Learning Plan on pages 5–6. -->

## Learning Objectives

- Explain how training platforms organise model state, workers, and updates.
- Compare centralised parameter services with collective communication.
- Assess the effects of bandwidth, latency, locality, and stale updates.
- Relate federated training to data distribution, privacy goals, and communication limits.

## Pages and Planned Topics

| Chapter | Topic | Availability |
|---:|---|---|
| 8 | [ML Platforms and Frameworks]({{< relref "080-ml-platforms-and-frameworks.md" >}}) | Page available; currently focuses on the Parameter Server model |
| 9 | [Distributed Deep Learning: Synchronisation and Communication]({{< relref "090-distributed-deep-learning.md" >}}) | Page available; parameter-server synchronisation, compression, overlap, and placement |
| 10 | Locality and Large-Scale Parallelism | Planned |
| 11 | GPU Architecture and Federated Learning | Planned |

The outline below describes the full module scope. Planned topics have no chapter page yet. Chapter 9 currently covers parameter-server execution policies and communication optimisation; collective communication, matrix multiplication, and Local SGD remain to be added from their source material.

## Topic Outline

### 8. ML Platforms and Frameworks

- Implementation issues and the Parameter Server model ☆
- Stochastic gradient descent
- TensorFlow architecture: master, worker, and parameter-server roles
- Graph mode, eager execution, and distributed training strategies
- Batch and micro-batch sizing
- Model compression, quantisation, I/O, and shuffling overhead

### 9. Distributed Deep Learning

- Decentralised SGD, all-reduce, and asynchronous parallelism
- System and program optimisation for neural networks
- Matrix multiplication
- Ring-based and tree-based reduction, bandwidth, and latency
- Hogwild!, parameter staleness, convergence, and variance
- Local SGD, periodic model averaging, and communication–computation trade-offs

### 10. Locality and Large-Scale Parallelism

- Locality-aware programs, massive multithreading, and GPGPUs
- Model parallelism: layer slicing, tensor splitting, and cross-device communication
- Pipeline parallelism: micro-batches, scheduling, and bubble overhead
- Large-scale neural-network training on GPU clusters
- Memory-efficient checkpointing

### 11. GPU Architecture and Federated Learning

- GPU architecture and threading models
- Matrix addition and multithreaded matrix multiplication
- Federated learning: non-IID data and privacy goals
- Secure aggregation
- Decentralised federated learning, gossip, and blockchain-based architectures

## How This Module Connects

Build on [M2: Distributed ML Algorithms]({{< relref "../m2-distributed-ML-algorithms/_index.md" >}}). For model optimisation and deployment under tighter resource limits, continue to [M4: Constrained Systems]({{< relref "../m4-constrained-systems/_index.md" >}}).

[ML System Optimisation overview]({{< relref "../_index.md" >}})

---
{{< home-link "Home" >}} | {{< section-index >}}
