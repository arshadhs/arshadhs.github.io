---
title: "ML System Optimisation"
draft: false
tags: ["ML System Optimisation", "MLSysOps", "Distributed Machine Learning", "AI", "ML"]
categories: ["AI", "ML"]
weight: 900
menu:
  main:
    identifier: mlsysops
bookCollapseSection: true
---

# ML System Optimisation

ML System Optimisation studies how to make machine learning workloads **faster, more scalable, more memory-efficient, and suitable for different hardware platforms**.

The subject connects machine learning algorithms with the systems that train and deploy them: multi-core CPUs, GPUs, distributed clusters, cloud platforms, edge devices, and embedded systems.

{{% hint info %}}
**ML system optimisation = model quality + computational efficiency + hardware awareness + scalability**
{{% /hint %}}

The learning path begins with performance measurement and parallel computing, progresses through distributed machine learning and scale-out platforms, and concludes with model compression and resource-constrained deployment.

---

## Big Picture

{{< mermaid >}}
flowchart TD
    A["Measure Performance"] --> B["Parallelise Work"]
    B --> C["Distribute Training"]
    C --> D["Scale Across Systems"]
    D --> E["Optimise Models"]
    E --> F["Deploy on Constrained Devices"]

    style A fill:#E1F5FE
    style B fill:#C8E6C9
    style C fill:#FFF9C4
    style D fill:#EDE7F6
    style E fill:#C8E6C9
    style F fill:#E1F5FE
{{< /mermaid >}}

---

## Modules

The learning path follows four modules. Each module has its own overview, topic outline, and chapter links.

| Module | Chapters | Focus |
|---|---:|---|
| [M1: Foundations]({{< relref "m1-foundations/_index.md" >}}) | 1–3 | Performance metrics, complexity, parallelisation, and hardware |
| [M2: Distributed ML Algorithms]({{< relref "m2-distributed-ML-algorithms/_index.md" >}}) | 4–7 | Parallel ML algorithms and distributed training strategies |
| [M3: Scale-Out Systems]({{< relref "m3-scale-out-systems/_index.md" >}}) | 8–11 | Platforms, distributed deep learning, and federated learning |
| [M4: Constrained Systems]({{< relref "m4-constrained-systems/_index.md" >}}) | 12–16 | Compression, edge deployment, and performance trade-offs |

Read the modules in order for a continuous learning path, or use a module index to find a particular topic. The indexes distinguish existing chapter pages from planned topics.

<!-- Module names and boundaries follow _COURSE_MLSysOps_ZG516.pdf, pages 2–7. -->

---

## Core Optimisation Trade-offs

| Optimisation Goal | Typical Benefit | Possible Cost |
|---|---|---|
| Reduce latency | Faster prediction or response | Greater hardware usage or lower batching efficiency |
| Increase throughput | More work completed per unit time | Higher latency for individual requests |
| Reduce model size | Lower storage and memory requirements | Possible loss of accuracy |
| Reduce communication | Better distributed scalability | Stale updates or slower convergence |
| Use lower precision | Faster computation and lower memory use | Numerical instability or accuracy loss |
| Increase parallelism | Faster training | Communication, synchronisation, and scheduling overhead |
| Reduce energy use | Longer battery life and lower operating cost | Lower throughput or reduced model capacity |

{{% hint warning %}}
Optimisation is not simply about making a model faster. A useful solution balances **speed, memory, scalability, accuracy, convergence stability, generalisation, and energy consumption**.
{{% /hint %}}

---

## Book References

### Primary References

1. John L. Hennessy and David A. Patterson, *Computer Architecture: A Quantitative Approach*, Sixth Edition, Morgan Kaufmann, 2017.
2. Yuan Tang, *Distributed Machine Learning Patterns*, Manning, 2023.

### Additional Reading Areas

- Distributed machine learning algorithms
- Quantisation and model compression
- Network pruning and deep compression
- TensorFlow Lite programming
- Evaluation of compression techniques

---

## Key Takeaways

{{% hint success %}}
- Performance must be measured using appropriate metrics before it can be improved.
- Parallelism can accelerate ML workloads, but communication and synchronisation limit speedup.
- Distributed training requires careful choices among data, model, and pipeline parallelism.
- Scale-out systems connect algorithms with clusters, distributed frameworks, GPUs, and federated architectures.
- Compression techniques make models smaller and faster for edge and constrained devices.
- Every optimisation involves trade-offs among performance, model quality, memory, scalability, and energy.
{{% /hint %}}

---

{{< home-link "Home" >}} | {{< section-index >}}
