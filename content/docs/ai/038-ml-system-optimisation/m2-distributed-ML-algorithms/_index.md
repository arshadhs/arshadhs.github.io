---
title: "M2: Distributed ML Algorithms"
draft: false
tags: ["ML System Optimisation", "MLSysOps", "Distributed Machine Learning", "Parallel Algorithms", "Distributed Training", "AI", "ML"]
categories: ["AI", "ML"]
weight: 200
menu:
  main:
    identifier: mlsysops_m2
    parent: mlsysops
bookCollapseSection: true
---

# M2: Distributed ML Algorithms

This module applies parallel and distributed computing to ML algorithms. It examines how to divide computation, combine results, and balance communication against useful work during training.

Module coverage:

- **Parallel ML Algorithms:** CNNs, gradient descent and SGD, SVM, k-means, k-nearest neighbours, decision trees, and random forests.
- **Distributed Training Strategies:** data parallelism, model parallelism, pipeline parallelism, gradient checkpointing, and mixed-precision training.

<!-- Coverage follows _COURSE_MLSysOps_ZG516.pdf, Content Structure on pages 2–3; topic boundaries follow the Learning Plan on pages 4–5. -->

## Learning Objectives

- Decompose ML algorithms into independent and dependent work.
- Explain how local results are aggregated and where communication is required.
- Compare distributed execution through Hadoop and Spark.
- Choose training strategies that address compute and memory constraints.

## Pages

| Chapter | Page | Main focus |
|---:|---|---|
| 4 | [Parallelisation of ML Algorithms]({{< relref "040-parallelisation-of-ml-algorithms.md" >}}) | Problem decomposition, ensembles, XGBoost, k-means, trees, forests, and SVM |
| 5 | [Communication-Aware Distributed ML]({{< relref "050-communication-aware-distributed-ml.md" >}}) | Communication overhead, distributed k-means and k-NN, model parallelism, and SGD |
| 6 | [Clusters, Hadoop, and Spark]({{< relref "060-clusters-hadoop-and-spark.md" >}}) | Cluster frameworks, MapReduce k-means, distributed CNNs, and communication reduction |
| 7 | [Distributed Training Strategies]({{< relref "070-distributed-training-strategies.md" >}}) | Data, model, and pipeline parallelism, checkpointing, and mixed precision |

## Topic Outline

### 4. Parallelisation of ML Algorithms

- Problem decomposition and parallelisation
- Ensemble methods and XGBoost on CPUs
- k-means
- Distributed decision trees and random forests
- Distributed implementations using Spark ML and XGBoost
- Parallel SVM kernel computation using block-wise and MapReduce-style methods

### 5. Communication-Aware Distributed ML

- Communication overhead in distributed algorithms
- Distributed k-means and model parallelism
- k-NN: data partitioning, locality-sensitive hashing, and distributed indexing
- KD-tree and Ball-tree indexing versus brute-force MapReduce
- Accuracy–latency trade-offs in approximate k-NN
- Gradient descent and SGD: mini-batch, synchronous, and asynchronous variants

### 6. Clusters, Hadoop, and Spark

- Cluster computing with Hadoop and Spark
- Lloyd's k-means algorithm in MapReduce
- Mini-batch k-means and streaming variants
- Convergence criteria and communication reduction through compressed updates
- CNN data and model parallelism, including layer splitting and tensor slicing
- Mixed precision, pipeline parallelism, and distributed training frameworks

### 7. Distributed Training Strategies ☆

- Data parallelism
- Model parallelism
- Pipeline parallelism
- Gradient checkpointing
- Mixed-precision training

## How This Module Connects

Use [M1: Foundations]({{< relref "../M1/_index.md" >}}) to interpret complexity and speedup. Continue to [M3: Scale-Out Systems]({{< relref "../M3/_index.md" >}}) for the platforms and communication mechanisms that support larger training runs.

[ML System Optimisation overview]({{< relref "../_index.md" >}})

---
{{< home-link "Home" >}} | {{< section-index >}}
