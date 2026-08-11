---
title: "ML System Optimisation"
draft: false
tags: ["ML System Optimisation", "MLSysOps", "Distributed Machine Learning", "AI", "ML"]
categories: ["AI", "ML"]
weight: 900
menu: main
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

## Modular Structure

### 1. Foundations

- Machine learning and deep learning workloads
- Performance metrics
  - Time and space complexity
  - Running time
  - Throughput
  - Latency and response time
- Performance scaling and tuning
- Training versus deployment environments
- Distributed, cloud, embedded, and mobile systems
- Parallel and distributed algorithms
- Speedup and Amdahl's Law ☆
- Scale-up versus scale-out
- Data, task, and request parallelism
- Parallel programming models
- MapReduce pattern
- Multi-core and GPGPU parallelism
- SIMD versus MIMD execution
- Memory hierarchy and bandwidth
- CPU and GPU clusters
- Data sharding
- Parameter-server and all-reduce paradigms

### 2. Distributed ML Algorithms

- Problem decomposition
- Parallelisation of machine learning algorithms
- Ensemble methods and XGBoost
- Distributed decision trees and random forests
- Parallel support vector machines
- Communication overhead
- Distributed k-means
- Distributed k-nearest neighbours
- Gradient descent and stochastic gradient descent
- Synchronous versus asynchronous SGD ☆
- Hadoop and Spark
- Distributed convolutional neural networks
- Data, model, and pipeline parallelism ☆
- Gradient checkpointing
- Mixed-precision training

### 3. Scale-Out Systems

- ML platforms and frameworks
- Parameter Server model ☆
- TensorFlow distributed architecture
- Distributed training strategies
- Batch and micro-batch sizing
- I/O and shuffling overhead
- Decentralised SGD
- Ring-based and tree-based all-reduce ☆
- Asynchronous parallelism and parameter staleness
- Local SGD
- Locality-aware programs
- Massive multithreading and GPGPUs
- Model and pipeline parallelism
- Micro-batch pipelining and pipeline bubbles
- Large-scale neural-network training
- GPU architecture and threading model
- Parallel matrix operations
- Federated learning
- Non-IID data, privacy, and secure aggregation
- Decentralised federated learning

### 4. Constrained Systems

- GPU implementation of convolutional neural networks
- Model compression
- Quantisation ☆
- Network pruning ☆
- Low-rank factorisation
- Weight sharing and encoding
- Knowledge distillation ☆
- Edge intelligence
- Reduced-precision arithmetic
- Hardware support for efficient inference
- Structured and unstructured pruning
- Sparsity, FLOPs reduction, and accuracy loss
- Deep compression pipeline
- Small language models
- TensorFlow Lite Micro
- Hardware- and compiler-aware optimisation
- Energy-aware algorithms
- Accuracy, model size, throughput, latency, and energy trade-offs ☆

---

## 16-Topic Learning Path

| # | Topic | Main Focus | Module |
|---:|---|---|---|
| 1 | ML and DL System Performance | Complexity, running time, throughput, response time, scaling, tuning, training and deployment environments | Foundations |
| 2 | Parallel and Distributed Algorithms | Systems performance, speedup, Amdahl's Law, parallelism types, communication cost, scale-up and scale-out | Foundations |
| 3 | Parallel Programming Models | MapReduce, task and request parallelism, multi-core and GPGPU execution, memory hierarchy, sharding, parameter server and all-reduce | Foundations |
| 4 | Parallelisation of ML Algorithms | Problem decomposition, ensemble methods, XGBoost, k-means, distributed trees, random forests, and SVM | Distributed ML Algorithms |
| 5 | Communication-Aware Distributed ML | Communication overhead, distributed k-means, model parallelism, distributed k-NN, synchronous and asynchronous SGD | Distributed ML Algorithms |
| 6 | Clusters, Hadoop, and Spark | Cluster computing, MapReduce k-means, streaming variants, communication reduction, distributed CNNs, and mixed precision | Distributed ML Algorithms |
| 7 | Distributed Training Strategies | Data, model, and pipeline parallelism, gradient checkpointing, and mixed-precision training | Distributed ML Algorithms |
| 8 | ML Platforms and Frameworks | Parameter Server model, SGD, TensorFlow architecture, distributed strategies, batching, model compression, I/O, and shuffling | Scale-Out Systems |
| 9 | Distributed Deep Learning | Decentralised SGD, all-reduce, asynchronous parallelism, Hogwild!, parameter staleness, local SGD, and matrix multiplication | Scale-Out Systems |
| 10 | Locality and Large-Scale Parallelism | Locality-aware programs, GPGPUs, model parallelism, pipeline scheduling, GPU clusters, and memory-efficient checkpointing | Scale-Out Systems |
| 11 | GPU Architecture and Federated Learning | GPU threading, parallel matrix operations, non-IID data, privacy, secure aggregation, and decentralised federated learning | Scale-Out Systems |
| 12 | Model Compression Foundations | CNNs on GPUs, quantisation, pruning, low-rank factorisation, weight sharing, encoding, and knowledge distillation | Constrained Systems |
| 13 | Quantisation and Edge Intelligence | Edge case studies, quantisation methods, reduced-precision arithmetic, and hardware support | Constrained Systems |
| 14 | Pruning and Deep Compression | Structured and unstructured pruning, sparsity metrics, FLOPs reduction, accuracy loss, and the deep compression pipeline | Constrained Systems |
| 15 | Optimisation Landscape and Small Language Models | Generic and platform-specific optimisation, frameworks, hardware, compilers, federated learning, and TensorFlow Lite Micro | Constrained Systems |
| 16 | Energy-Constrained ML | Adapting algorithms for constrained devices and balancing accuracy, model size, throughput, response time, and energy | Constrained Systems |

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
