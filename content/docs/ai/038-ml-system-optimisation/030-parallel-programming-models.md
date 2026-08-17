---
title: "Parallel Programming Models"
draft: false
tags: ["ML System Optimization", "MLSysOps", "Multi-core CPU", "GPGPU", "SIMD", "MIMD", "SIMT", "GPU", "TPU", "AI", "ML"]
categories: ["AI", "ML"]
weight: 300
menu: main
---

# Parallel Programming Models

Parallel algorithms need hardware that can execute independent work efficiently. Modern systems therefore combine multiple CPU cores, memory hierarchies, threads, instruction pipelines, GPUs, clusters, and specialised matrix processors.

This page covers:

- multi-core CPU organisation
- cache and memory hierarchy
- processes, threads, scheduling, and synchronisation
- instruction pipelining and clock-cycle time
- SIMD, MIMD, and SIMT execution
- GPGPU architecture and GPU memory behaviour
- CPU-only and GPU-accelerated clusters
- Tensor Processing Units and systolic arrays

## Learning Objectives

By the end of this page, you should be able to:

- describe the main components of a multi-core CPU
- explain the purpose of L1, L2, and L3 cache
- distinguish a process from a thread
- explain scheduling, context switching, synchronisation, lost updates, and deadlock
- describe instruction pipelining and its effect on throughput
- compare SIMD, MIMD, and SIMT execution
- explain how GPU memory access affects performance
- compare CPU-only, GPU-accelerated, and TPU-based computing

## Big Picture

{{< mermaid >}}
flowchart TD
    A["Parallel Work"] --> B["Multi-core CPU"]
    A --> C["GPGPU"]
    A --> D["TPU"]
    B --> E["Threads and MIMD"]
    C --> F["SIMT and High Throughput"]
    D --> G["Matrix Acceleration"]
    E --> H["CPU Cluster"]
    F --> I["GPU Cluster"]
    G --> J["TPU Interconnect"]

    style A fill:#E1F5FE
    style B fill:#C8E6C9
    style C fill:#FFF9C4
    style D fill:#EDE7F6
    style E fill:#E1F5FE
    style F fill:#C8E6C9
    style G fill:#FFF9C4
    style H fill:#EDE7F6
    style I fill:#E1F5FE
    style J fill:#C8E6C9
{{< /mermaid >}}

## 1. Why Multi-core CPUs?

Increasing clock frequency was once a primary way to improve CPU performance. However, higher frequencies lead to greater power consumption, more heat, reduced energy efficiency, and physical design limits.

Modern processors therefore improve performance by placing multiple processing cores on one chip.

A **multi-core CPU** contains two or more independent processing units called cores. Each core can execute its own instruction stream, allowing different programs, processes, threads, or parts of one program to run simultaneously.

This provides:

- better support for multitasking
- higher throughput
- improved responsiveness
- greater energy efficiency than relying only on clock-frequency increases

{{% hint info %}}
More cores create the opportunity for parallel execution, but software must expose independent work before those cores can be used effectively.
{{% /hint %}}

### Typical Core Counts

| Processor Type | Illustrative Core Count | Typical Use |
|---|---:|---|
| Dual-core | 2 | Basic multitasking and light applications |
| Quad-core | 4 | General-purpose and moderate parallel workloads |
| Hexa-core | 6 | Content creation, development, and analysis |
| Octa-core | 8 | Higher-performance and parallel workloads |
| Many-core CPU | 16 or more | Servers, scientific computing, and large workloads |

Core count alone does not determine performance. Clock rate, cache size, memory bandwidth, interconnection design, and workload structure also matter.

## 2. Inside a CPU Core

A CPU core contains several components that cooperate to execute instructions:

- **Arithmetic Logic Unit:** performs arithmetic and logical operations
- **Control Unit:** directs and coordinates instruction execution
- **Registers:** very small, very fast storage for current values and instructions
- **Instruction pipeline:** overlaps stages of multiple instructions
- **L1 cache:** holds frequently used instructions and data close to the core
- **branch-prediction unit:** predicts control-flow decisions to keep the pipeline busy
- **instruction decoder:** converts instructions into internal operations
- **load/store unit:** transfers data between registers and memory
- **floating-point unit:** performs floating-point calculations

### Shared-Memory Organisation

In a typical multi-core system:

- each core has private execution resources and usually private L1 cache
- some cache levels may be private or shared
- a last-level cache is normally shared
- cores communicate with main memory through a memory controller and an interconnection network

This shared-memory organisation makes communication between threads convenient, but it also creates contention and consistency challenges.

## 3. Cache and Memory Hierarchy ☆

Processors are much faster than main memory. A hierarchy of progressively larger but slower storage reduces the time spent waiting for data.

{{< mermaid >}}
flowchart TD
    A["Registers: Fastest and Smallest"] --> B["L1 Cache"]
    B --> C["L2 Cache"]
    C --> D["L3 or Last-Level Cache"]
    D --> E["Main Memory"]
    E --> F["Disk or SSD: Slowest"]

    style A fill:#E1F5FE
    style B fill:#C8E6C9
    style C fill:#FFF9C4
    style D fill:#EDE7F6
    style E fill:#E1F5FE
    style F fill:#C8E6C9
{{< /mermaid >}}

### L1 Cache

L1 is normally:

- located inside each core
- private to that core
- the smallest and fastest cache level
- divided into instruction cache and data cache in many designs

### L2 Cache

L2 is larger and slower than L1, but still much faster than RAM. Depending on the processor, it may be private to one core or shared by a small group of cores.

### L3 Cache and Last-Level Cache

L3 is usually:

- larger than L1 and L2
- shared by multiple cores
- slower than the lower cache levels
- the last cache checked before main memory

The final on-chip cache is called the **last-level cache**. It is commonly L3, although the exact level depends on the architecture.

### Cache Hit and Cache Miss

A **cache hit** occurs when requested data is found in cache. A **cache miss** forces the processor to search a lower cache level or main memory, increasing delay.

{{% colour "green" %}}
{{< katex display=true >}}
H = \frac{\text{Number of cache hits}}
{\text{Total memory accesses}}
{{< /katex >}}
{{% /colour %}}

If `H` is the hit rate, the miss rate is `1 − H`.

An approximate effective memory access time is:

{{% colour "green" %}}
{{< katex display=true >}}
\operatorname{EMAT}
= H T_{\text{cache}}
+ (1-H)T_{\text{memory}}
{{< /katex >}}
{{% /colour %}}

Because main-memory access is much slower, even a modest reduction in hit rate can increase average memory delay and cause processor stalls.

{{% hint success %}}
A fast processor cannot remain fast if it repeatedly waits for data. Cache behaviour and memory bandwidth are therefore central to system performance.
{{% /hint %}}

## 4. Memory Controller and Interconnection Network

### Memory Controller

The memory controller manages communication between the CPU and RAM. Its responsibilities include:

- receiving memory requests from cores
- managing memory addresses
- scheduling reads and writes
- controlling DRAM timing
- improving effective memory bandwidth

Modern CPUs often integrate the memory controller onto the processor package, reducing latency and improving bandwidth.

### Interconnection Network

The interconnection network links cores, caches, memory controllers, and other components. It transfers data, routes memory requests, maintains cache coherence, and supports synchronisation.

| Interconnect | Structure | Main Characteristic |
|---|---|---|
| Ring bus | Components arranged in a ring | Simple for a moderate number of cores |
| Mesh | Components arranged in a grid | Scales better to many cores |
| Crossbar | Direct paths between components | Low latency but increasing hardware complexity |
| Network-on-Chip | Packet-based on-chip network | Scalable communication for many-core designs |

### Main Memory

RAM provides much greater capacity than cache but has higher latency. It is shared by cores and is accessed when data is not found in cache. Multiple concurrent accesses create demand for bandwidth and require memory-consistency and cache-coherence mechanisms.

## 5. Processes and Threads ☆

A **process** is a running program with its own address space and operating-system resources. A **thread** is an execution path within a process.

| Feature | Process | Thread |
|---|---|---|
| Address space | Normally separate | Shared with other threads in the process |
| Resource sharing | Requires explicit communication | Shares process memory and resources |
| Creation cost | Higher | Lower |
| Context switching | More expensive | Usually less expensive |
| Communication | Inter-process communication | Direct through shared memory |
| Failure impact | Often isolated to one process | May affect the whole process |

Threads are useful because they communicate efficiently through shared memory. The same shared state also makes correct synchronisation essential.

### From Program to CPU Execution

The execution flow is:

1. Source code is compiled into a program.
2. The executable is stored on disk.
3. The operating system loads it into memory.
4. The running program becomes a process.
5. The process creates one or more threads.
6. Runnable threads enter the ready queue.
7. The scheduler selects a thread for a CPU core.
8. The CPU fetches, decodes, and executes its instructions.
9. A time-slice expiry or higher-priority thread may cause pre-emption.
10. During a context switch, the current state is saved and another thread's state is restored.

## 6. Thread Scheduling and Lifecycle

On a single-core CPU, only one thread executes at an instant. Time slicing creates concurrency by switching rapidly between threads. On a multi-core CPU, different threads can execute simultaneously on different cores.

If runnable threads outnumber cores, the system is **oversubscribed**. The scheduler must alternate threads through context switching and attempt to balance work across cores.

### Thread Lifecycle

{{< mermaid >}}
flowchart TD
    A["New"] --> B["Ready"]
    B --> C["Running"]
    C -->|Pre-empted| B
    C -->|Wait| D["Waiting"]
    D -->|Resource available| B
    C --> E["Terminated"]

    style A fill:#E1F5FE
    style B fill:#C8E6C9
    style C fill:#FFF9C4
    style D fill:#EDE7F6
    style E fill:#E1F5FE
{{< /mermaid >}}

A running thread may enter a waiting or blocked state while it waits for memory, input, a lock, or another thread. When the required resource becomes available, it returns to the ready state.

### Shared Data and Lost Updates

Suppose two threads read and update the same memory location. If their operations overlap without correct coordination, one update may overwrite the other. This is a **lost update** caused by a race condition.

Common coordination mechanisms include:

- locks
- mutexes
- semaphores
- atomic operations
- barriers

These mechanisms protect correctness, but waiting and coordination reduce performance.

### Deadlock

Deadlock occurs when threads wait indefinitely for one another. For example:

- Thread 1 holds resource A and waits for resource B.
- Thread 2 holds resource B and waits for resource A.

Neither thread can continue.

{{% hint warning %}}
Parallel execution introduces both performance overhead and correctness risks. Synchronisation must prevent races without creating excessive waiting or deadlock.
{{% /hint %}}

## 7. Instruction Pipelining ☆

Instruction pipelining overlaps different stages of multiple instructions. Instead of waiting for one complete instruction before starting another, the processor keeps different hardware units busy at the same time.

A common five-stage pipeline is:

1. **IF — Instruction Fetch**
2. **ID — Instruction Decode**
3. **EX — Execute**
4. **MEM — Memory Access**
5. **WB — Write Back**

{{< mermaid >}}
flowchart TD
    A["Fetch"] --> B["Decode"]
    B --> C["Execute"]
    C --> D["Memory Access"]
    D --> E["Write Back"]

    style A fill:#E1F5FE
    style B fill:#C8E6C9
    style C fill:#FFF9C4
    style D fill:#EDE7F6
    style E fill:#E1F5FE
{{< /mermaid >}}

Without pipelining, each instruction completes all stages before the next begins. This leaves functional units idle. With pipelining, one instruction may be decoded while another executes and a third accesses memory.

Pipeline registers sit between stages. They store intermediate values, synchronise data transfer, and allow the stages to operate independently.

### Pipeline Filling and Draining

For a five-stage pipeline:

- the first cycles fill the pipeline
- once full, all stages operate concurrently
- ideally, one instruction completes during every clock cycle
- the final cycles drain the remaining instructions

Pipelining primarily improves **instruction throughput**. It does not reduce every individual instruction to a single stage.

### Clock Cycle Time

Clock-cycle time is the duration of one processor clock period:

{{% colour "green" %}}
{{< katex display=true >}}
T_{\text{cycle}} = \frac{1}{f}
{{< /katex >}}
{{% /colour %}}

For a `2 GHz` processor:

{{% colour "green" %}}
{{< katex display=true >}}
T_{\text{cycle}}
= \frac{1}{2\times10^9}
= 0.5\times10^{-9}\text{ seconds}
= 0.5\text{ nanoseconds}
{{< /katex >}}
{{% /colour %}}

Clock frequency alone does not determine system performance. Instructions may need several cycles, pipelines may stall, and cache misses may leave the processor waiting for data.

## 8. SIMD and MIMD ☆

### SIMD — Single Instruction, Multiple Data

SIMD applies the same instruction to multiple data elements in lockstep.

It is well suited to:

- vector and matrix operations
- image and audio processing
- scientific computation
- regular data-parallel workloads

For four additions, scalar execution uses four addition instructions, while a four-lane SIMD instruction can process all four pairs together.

{{% colour "green" %}}
{{< katex display=true >}}
\text{Ideal SIMD speedup}
= \frac{\text{Number of scalar operations}}
{\text{Number of SIMD instructions}}
{{< /katex >}}
{{% /colour %}}

For four elements, the ideal instruction-count speedup is `4/1 = 4`.

SIMD works best when operations are regular and branches do not force elements to follow different control paths.

### MIMD — Multiple Instruction, Multiple Data

MIMD allows different processors or cores to execute different instructions on different data. It is suited to flexible task-parallel workloads such as operating systems, web servers, and multithreaded applications.

### Comparison

| Aspect | SIMD | MIMD |
|---|---|---|
| Instruction streams | One | Multiple |
| Data streams | Multiple | Multiple |
| Execution style | Lockstep | Independent |
| Control flow | Less flexible | More flexible |
| Best suited to | Regular data parallelism | Diverse task parallelism |
| Common hardware | Vector units and GPU-style lanes | Multi-core CPUs and clusters |

## 9. Multi-core Performance Model

Theoretical speedup still follows Amdahl's Law, but practical execution time also contains coordination and memory costs:

{{% colour "green" %}}
{{< katex display=true >}}
T_{\text{parallel}}
= T_{\text{compute}}
+ T_{\text{synchronisation}}
+ T_{\text{memory}}
{{< /katex >}}
{{% /colour %}}

where:

- `T_compute` is useful computational work
- `T_synchronisation` includes barriers, locks, coordination, and waiting
- `T_memory` includes cache-coherence traffic, shared-memory conflicts, and bandwidth limitations

Multi-core CPUs are particularly effective for:

- task-level parallelism
- moderate data parallelism
- branch-heavy calculations
- general-purpose workloads

Their scalability depends on the parallel fraction, synchronisation cost, memory behaviour, and cache efficiency.

## 10. GPGPUs and SIMT ☆

A **General-Purpose GPU** is designed for massive parallel throughput. It contains many lightweight processing units organised into groups such as streaming multiprocessors.

Compared with a CPU, a GPU generally devotes more hardware to parallel arithmetic and less to complex control logic and large low-latency caches.

### SIMT — Single Instruction, Multiple Threads

GPUs commonly use a SIMT execution model. Many threads execute the same instruction sequence, with each thread processing a different data element.

For vector addition:

{{% colour "green" %}}
{{< katex display=true >}}
C[i] = A[i] + B[i]
{{< /katex >}}
{{% /colour %}}

Each thread calculates one index. This works well because the elements are independent and require the same operations.

{{% hint info %}}
SIMD describes one instruction operating on several data lanes. SIMT presents many software threads that are scheduled in groups and often execute the same instruction together.
{{% /hint %}}

## 11. GPU Memory Hierarchy ☆

GPU performance depends strongly on how threads access memory.

| Memory Type | Scope | Relative Behaviour |
|---|---|---|
| Registers | Private to a thread | Fastest and smallest |
| Shared memory | Shared within a thread block | Fast and programmer-managed |
| Global memory | Accessible across the device | Large but high latency |
| Constant/texture memory | Specialised cached access | Useful for particular access patterns |

### Memory Coalescing

When neighbouring threads access neighbouring memory addresses, their accesses can be combined into fewer memory transactions. This is called **coalesced access**.

Poorly arranged access patterns require more transactions and reduce effective bandwidth.

### Occupancy

Occupancy describes how many threads are active compared with the hardware's supported maximum:

{{% colour "green" %}}
{{< katex display=true >}}
\text{Occupancy}
= \frac{\text{Active threads}}
{\text{Maximum supported threads}}
{{< /katex >}}
{{% /colour %}}

Higher occupancy can help hide memory latency by allowing the GPU to run other threads while some are waiting.

### Memory Bandwidth

Memory bandwidth is the maximum rate at which data can move between memory and processing units. High arithmetic capacity is ineffective when data cannot arrive quickly enough.

GPUs perform best when:

- the dataset is large
- the parallel fraction is high
- control flow has little branching
- memory access is regular and coalesced

### Memory-Optimisation Practices

For multi-core CPUs:

- use locality of reference
- use cache-aware data structures
- avoid false sharing between threads

For GPUs:

- align accesses for coalescing
- reuse data in shared memory
- minimise global-memory access
- avoid shared-memory bank conflicts

For both:

- balance computation with memory traffic
- use pipelining and tiling where appropriate

## 12. CPU and GPU Clusters

A cluster combines multiple machines, called nodes, to increase compute capacity, memory, or throughput.

### CPU-Only Clusters

CPU-only clusters consist of multi-core CPU nodes. They are suitable for:

- control-heavy applications
- large in-memory databases
- distributed file systems
- ETL and general data-processing pipelines

They normally provide more memory per node and broad support for traditional distributed software, but offer lower throughput for highly regular matrix-heavy computation.

### GPU-Accelerated Clusters

GPU-accelerated clusters contain CPUs together with one or more GPUs per node. The CPU manages general control flow, while GPUs accelerate large parallel calculations.

They are suitable for:

- training large neural networks
- image and video processing
- simulations
- matrix multiplication and convolution

Challenges include:

- limited memory on each GPU
- data transfer between CPU and GPU
- communication between GPUs and nodes
- specialised libraries and programming models
- higher hardware, power, and cooling requirements

### Why Scaling Is Not Perfectly Linear

If one GPU takes `100` hours, an ideal calculation might suggest:

- two GPUs take `50` hours
- four GPUs take `25` hours
- eight GPUs take `12.5` hours

Real scaling is slower because GPUs exchange data, synchronise results, and wait for communication. The amount of model data that fits in each GPU also affects how work must be distributed.

### Comparison

| Feature | CPU-Only Cluster | GPU-Accelerated Cluster |
|---|---|---|
| Compute units | Multi-core CPUs | CPUs with dedicated GPUs |
| Memory per node | Often higher | GPU memory is more limited |
| Best workload | Control-heavy or general-purpose | Highly parallel numerical computation |
| Programming | Threads, processes, and distributed frameworks | GPU libraries and accelerator-aware frameworks |
| Strength | Flexibility and capacity | Matrix throughput |
| Main cost | Lower arithmetic throughput | Communication, hardware, power, and complexity |

## 13. Tensor Processing Units and Systolic Arrays

A **Tensor Processing Unit** is a specialised accelerator designed around the tensor and matrix operations used heavily in neural networks.

Unlike a general-purpose CPU, a TPU dedicates substantial hardware to repeated matrix multiplication and accumulation. It is less suited to irregular branch-heavy logic, but can be highly efficient for supported neural-network workloads.

### Systolic Array

A systolic array is a grid of processing elements through which data flows in a regular pattern. Partial results move between neighbouring elements while multiply-accumulate operations are performed repeatedly.

{{< mermaid >}}
flowchart TD
    A["Input Activations"] --> B["Matrix Multiply Unit"]
    C["Model Weights"] --> B
    B --> D["Accumulated Results"]
    D --> E["Neural-Network Output"]

    style A fill:#E1F5FE
    style B fill:#C8E6C9
    style C fill:#FFF9C4
    style D fill:#EDE7F6
    style E fill:#E1F5FE
{{< /mermaid >}}

This regular data movement reduces repeated trips to external memory and keeps the matrix unit busy.

Multiple TPU devices can be connected through a high-speed interconnect to form larger accelerator systems. Hardware generations may also be tuned differently for training and inference.

### CPU, GPU, and TPU

| Processor | Main Strength | Execution Character | Suitable Work |
|---|---|---|---|
| CPU | Flexible general-purpose control | Few powerful cores, strong branching | Operating systems, services, irregular computation |
| GPU | Massive parallel throughput | Many lightweight threads using SIMT | Training, graphics, simulation, large tensor operations |
| TPU | Specialised tensor computation | Matrix-oriented dataflow | Supported neural-network training and inference |

{{% hint warning %}}
No processor is universally best. The correct choice depends on control flow, data size, parallel fraction, memory requirements, communication cost, and the operations used by the workload.
{{% /hint %}}

## 14. Choosing the Appropriate Hardware

| Workload Characteristic | More Natural Starting Point |
|---|---|
| Branch-heavy, irregular control flow | CPU |
| Many independent but different tasks | Multi-core CPU or CPU cluster |
| Same operation over a large regular dataset | SIMD-capable CPU or GPU |
| Large neural-network tensor operations | GPU or TPU |
| Model exceeds one accelerator's memory | Multi-accelerator or clustered system |
| Performance limited by memory access | Improve locality and memory behaviour before adding compute |

The decision should be based on measured bottlenecks rather than core count or peak floating-point performance alone.

## Common Mistakes

{{% hint warning %}}
- Assuming that more CPU cores automatically make a sequential program faster.
- Treating a high clock frequency as a complete measure of processor speed.
- Ignoring cache misses, memory bandwidth, and processor stalls.
- Confusing concurrency on one core with true simultaneous execution on multiple cores.
- Sharing mutable data across threads without correct synchronisation.
- Assuming that locks have no performance cost.
- Treating pipelining as if every instruction finishes in one stage.
- Confusing SIMD with MIMD or SIMT.
- Assuming that every workload is suitable for a GPU.
- Expecting linear scaling when additional GPUs must exchange and synchronise data.
{{% /hint %}}

## Practice Questions

1. Why did processor design move from frequency scaling towards multi-core architectures?
2. Compare L1, L2, and L3 cache.
3. Calculate the effective memory access time when `H = 0.95`, cache access takes `2 ns`, and memory access takes `100 ns`.
4. Compare a process and a thread.
5. Explain how a lost update can occur and name two mechanisms that can prevent it.
6. Describe a two-thread deadlock scenario.
7. List the five instruction-pipeline stages and explain how pipelining increases throughput.
8. Calculate the clock-cycle time of a `3 GHz` processor.
9. Compare SIMD and MIMD.
10. Explain the relationship between SIMD and SIMT.
11. Why are coalesced GPU memory accesses important?
12. Explain how occupancy helps hide memory latency.
13. Compare CPU-only and GPU-accelerated clusters.
14. Why does adding GPUs not normally provide perfectly linear speedup?
15. What is the purpose of a systolic array in a TPU?

## Key Takeaways

{{% hint success %}}
- Multi-core CPUs improve performance through concurrent execution rather than clock frequency alone.
- Cache hierarchy bridges the speed gap between processors and main memory.
- Threads are lightweight execution paths that share process memory, requiring careful synchronisation.
- Pipelining increases instruction throughput by overlapping execution stages.
- SIMD applies one instruction to multiple data elements; MIMD executes independent instruction streams; GPUs commonly expose SIMT threads.
- GPU performance depends on parallelism, memory bandwidth, coalescing, and occupancy.
- Cluster scaling is limited by communication, synchronisation, and memory placement.
- TPUs specialise in tensor and matrix computation using structures such as systolic arrays.
{{% /hint %}}

## Checklist

- [ ] I can describe a multi-core CPU and its memory hierarchy.
- [ ] I can explain cache hits, misses, and effective memory access time.
- [ ] I can distinguish processes and threads.
- [ ] I can explain scheduling, races, lost updates, and deadlock.
- [ ] I can describe instruction pipelining and clock-cycle time.
- [ ] I can compare SIMD, MIMD, and SIMT.
- [ ] I can explain GPU memory hierarchy and coalescing.
- [ ] I can compare CPU, GPU, and TPU systems.

---
{{< home-link "Home" >}} | {{< section-index >}}
