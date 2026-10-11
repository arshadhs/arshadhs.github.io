---
title: "M1: Foundations"
draft: false
tags: ["ML System Optimisation", "MLSysOps", "Foundations", "Performance", "Parallel Computing", "AI", "ML"]
categories: ["AI", "ML"]
weight: 100
menu:
  main:
    identifier: mlsysops_m1
    parent: mlsysops
bookCollapseSection: true
---

# M1: Foundations

This module establishes how to measure ML system performance, identify bottlenecks, and decide which work can run in parallel. It connects algorithmic complexity with the capabilities and limits of CPUs, GPUs, and clusters.

Module coverage:

- **Performance Metrics and Complexity Analysis:** time and space complexity, throughput, latency, and speedup through Amdahl's Law.
- **Parallelisation Paradigms:** data-level, task-level, and algorithm-specific parallelism.
- **Modern Hardware Architectures:** multi-core CPUs, GPGPUs, GPU/CPU clusters, and resource management.

<!-- Coverage follows _COURSE_MLSysOps_ZG516.pdf, Content Structure on page 2; topic boundaries follow the Learning Plan on page 4. -->

## Learning Objectives

- Distinguish complexity from measured running time, throughput, and latency.
- Explain the limits of speedup and the cost of communication.
- Choose between data, task, and request parallelism for a workload.
- Relate memory hierarchy, bandwidth, and hardware organisation to performance.

## Pages

| Chapter | Page | Main focus |
|---:|---|---|
| 1 | [ML and DL System Performance]({{< relref "010-ml-and-dl-system-performance.md" >}}) | Complexity, performance metrics, scaling, tuning, training and deployment |
| 2 | [Parallel and Distributed Algorithms]({{< relref "020-parallel-and-distributed-algorithms.md" >}}) | Speedup, Amdahl's Law, parallelism types, scale-up and scale-out |
| 3 | [Parallel Programming Models]({{< relref "030-parallel-programming-models.md" >}}) | MapReduce, multi-core CPUs, GPGPUs, memory hierarchy, and clusters |

## Topic Outline

### 1. ML and DL System Performance

- Time complexity of algorithms and running time
- Performance scaling and tuning
- Training versus deployment
- Distributed and cloud environments, embedded devices, and mobile systems
- Throughput and response time

### 2. Parallel and Distributed Algorithms

- Systems and performance
- Speedup: approaches, issues, and Amdahl's Law ☆
- Data parallelism, task parallelism, and request parallelism
- Scale-out clusters: communication cost and its impact on speedup
- Scale-up versus scale-out

### 3. Parallel Programming Models

- Parallel programming models and the MapReduce pattern
- Task-parallel and request-parallel execution
- Multi-core and GPGPU parallelism: SIMD versus MIMD
- Memory hierarchies and bandwidth considerations
- CPU-only and GPU-accelerated distributed clusters
- Horizontal and vertical data sharding
- Parameter-server and all-reduce paradigms

## How This Module Connects

Performance measurement and parallel execution provide the basis for [M2: Distributed ML Algorithms]({{< relref "../M2/_index.md" >}}). Communication and memory limits remain relevant throughout the later modules.

[ML System Optimisation overview]({{< relref "../_index.md" >}})

---
{{< home-link "Home" >}} | {{< section-index >}}
