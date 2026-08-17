---
title: "Parallel and Distributed Algorithms"
draft: false
tags: ["ML System Optimization", "MLSysOps", "Parallel Computing", "Amdahl's Law", "Data Parallelism", "Task Parallelism", "AI", "ML"]
categories: ["AI", "ML"]
weight: 200
menu: main
---

# Parallel and Distributed Algorithms

Parallelisation divides computational work into parts that can execute concurrently. The purpose is to reduce completion time or increase throughput, but the gain depends on how much work is genuinely independent and how much overhead is introduced.

This page covers:

- speedup, maximum speedup, and processor efficiency
- Amdahl's Law
- data-level parallelism
- task-level parallelism
- algorithm-specific parallelism
- communication, synchronisation, scheduling, and load-balancing overhead
- parallel merge sort and matrix multiplication

## Learning Objectives

By the end of this page, you should be able to:

- calculate speedup and efficiency
- use Amdahl's Law to estimate the limit of parallel execution
- distinguish data-level, task-level, and algorithm-specific parallelism
- explain critical-path time and load imbalance
- identify the effect of communication and synchronisation overhead
- explain how divide-and-conquer algorithms expose parallel work

## Big Picture

{{< mermaid >}}
flowchart TD
    A["Computational Work"] --> B["Find Independent Parts"]
    B --> C["Data-Level"]
    B --> D["Task-Level"]
    B --> E["Algorithm-Specific"]
    C --> F["Parallel Execution"]
    D --> F
    E --> F
    F --> G["Measure Speedup"]

    style A fill:#E1F5FE
    style B fill:#C8E6C9
    style C fill:#FFF9C4
    style D fill:#EDE7F6
    style E fill:#E1F5FE
    style F fill:#C8E6C9
    style G fill:#FFF9C4
{{< /mermaid >}}

## 1. What Is Parallelisation?

Parallelisation is the decomposition of a computational workload into independent components that can run concurrently on multiple processors, cores, or workers.

Suppose:

- `T_serial` is the time taken on one processor
- `P` is the fraction that can run in parallel
- `1 − P` is the sequential fraction
- `N` is the number of processors

Ignoring overhead, the parallel execution time is:

{{% colour "green" %}}
{{< katex display=true >}}
T_{\text{parallel}}
= (1-P)T_{\text{serial}}
+ \frac{P T_{\text{serial}}}{N}
{{< /katex >}}
{{% /colour %}}

If the entire workload is parallelisable and there is no overhead:

{{% colour "green" %}}
{{< katex display=true >}}
P=1
\quad \Longrightarrow \quad
T_{\text{parallel}} = \frac{T_{\text{serial}}}{N}
{{< /katex >}}
{{% /colour %}}

This is the ideal case of **linear speedup**. In practice, parallel execution also includes overhead:

{{% colour "green" %}}
{{< katex display=true >}}
T_{\text{parallel}}
= (1-P)T_{\text{serial}}
+ \frac{P T_{\text{serial}}}{N}
+ T_{\text{overhead}}
{{< /katex >}}
{{% /colour %}}

Overhead may come from:

- communication between workers
- synchronisation through locks or barriers
- creation and scheduling of tasks or threads
- context switching
- data transfer and memory access
- uneven distribution of work

{{% hint info %}}
Parallelisation tries to transform work of order `T` into work closer to `T/N`, but sequential work and overhead prevent perfect division.
{{% /hint %}}

## 2. Speedup ☆

Speedup measures how much faster the parallel system is than the serial system.

{{% colour "green" %}}
{{< katex display=true >}}
S(N) = \frac{T_{\text{serial}}}{T_{\text{parallel}}}
{{< /katex >}}
{{% /colour %}}

### Worked Example

If a program takes `100` seconds on one processor and `30` seconds on four processors:

{{% colour "green" %}}
{{< katex display=true >}}
S(4) = \frac{100}{30} \approx 3.33
{{< /katex >}}
{{% /colour %}}

The parallel system is approximately `3.33` times faster. Ideal four-processor speedup would be `4`, so the difference indicates overhead or a sequential bottleneck.

| Result | Interpretation |
|---|---|
| `S(N) = N` | Ideal linear speedup |
| `S(N) < N` | Overhead, imbalance, or sequential work limits performance |
| `S(N) > N` | Superlinear speedup, occasionally caused by cache effects |

## 3. Amdahl's Law ☆

Amdahl's Law describes how the sequential fraction of a program limits its total speedup.

{{% colour "green" %}}
{{< katex display=true >}}
S(N) = \frac{1}{(1-P)+\frac{P}{N}}
{{< /katex >}}
{{% /colour %}}

The parallel fraction becomes faster as `N` increases, but the sequential fraction must still execute.

### Example: 80% Parallel Work

Suppose `80%` of a program is parallelisable and four processors are used:

{{% colour "green" %}}
{{< katex display=true >}}
\begin{aligned}
P &= 0.8 \\
1-P &= 0.2 \\
N &= 4
\end{aligned}
{{< /katex >}}
{{% /colour %}}

{{% colour "green" %}}
{{< katex display=true >}}
S(4)
= \frac{1}{0.2+\frac{0.8}{4}}
= \frac{1}{0.4}
= 2.5
{{< /katex >}}
{{% /colour %}}

If the original execution time was `100` seconds:

- the sequential `20%` still takes `20` seconds
- the parallel `80%` takes `80/4 = 20` seconds
- total parallel time is `40` seconds
- speedup is `100/40 = 2.5`

Although four processors are available, the speedup is only `2.5`.

### Maximum Speedup

As the number of processors becomes extremely large, the parallel portion approaches zero execution time. The sequential portion remains:

{{% colour "green" %}}
{{< katex display=true >}}
S_{\max} = \lim_{N\to\infty} S(N) = \frac{1}{1-P}
{{< /katex >}}
{{% /colour %}}

For `P = 0.8`:

{{% colour "green" %}}
{{< katex display=true >}}
S_{\max} = \frac{1}{0.2} = 5
{{< /katex >}}
{{% /colour %}}

Even infinitely many processors cannot make this program more than five times faster.

### Example: Video Rendering

A video-rendering task takes `200` minutes on one processor:

- `70%` is parallel frame rendering
- `30%` is final encoding and synchronisation
- `8` processors are used

{{% colour "green" %}}
{{< katex display=true >}}
S(8)
= \frac{1}{0.3+\frac{0.7}{8}}
\approx 2.58
{{< /katex >}}
{{% /colour %}}

{{% colour "green" %}}
{{< katex display=true >}}
T_{\text{parallel}}
= \frac{200}{2.58}
\approx 77.5 \text{ minutes}
{{< /katex >}}
{{% /colour %}}

The `60` minutes of sequential work remain the main bottleneck.

{{% hint success %}}
The most valuable optimisation is often reducing the sequential fraction, not merely adding more processors.
{{% /hint %}}

## 4. Efficiency ☆

Efficiency measures how effectively the available processors contribute to speedup.

{{% colour "green" %}}
{{< katex display=true >}}
E(N) = \frac{S(N)}{N}
{{< /katex >}}
{{% /colour %}}

Using the earlier result, `S(4) = 2.5`:

{{% colour "green" %}}
{{< katex display=true >}}
E(4) = \frac{2.5}{4} = 0.625 = 62.5\%
{{< /katex >}}
{{% /colour %}}

Each processor contributes an average of `62.5%` of its ideal capacity. As processors are added, efficiency normally falls unless almost all the work is parallelisable.

Using Amdahl's Law:

{{% colour "green" %}}
{{< katex display=true >}}
E(N) = \frac{1}{N(1-P)+P}
{{< /katex >}}
{{% /colour %}}

This expression shows why the term containing the sequential fraction grows as `N` increases.

## 5. Data-Level Parallelism ☆

Data-level parallelism applies the **same operation** to different data elements simultaneously.

It is suitable when:

- operations on individual elements are independent
- there is no dependency between elements
- data can be divided reasonably evenly

For vector addition:

{{% colour "green" %}}
{{< katex display=true >}}
C[i] = A[i] + B[i],
\qquad i=1,2,\ldots,n
{{< /katex >}}
{{% /colour %}}

If one addition takes time `t_op`, serial execution takes:

{{% colour "green" %}}
{{< katex display=true >}}
T_{\text{serial}} = n t_{\text{op}}
{{< /katex >}}
{{% /colour %}}

With `N` processors and an even division of work:

{{% colour "green" %}}
{{< katex display=true >}}
T_{\text{parallel}} = \frac{n}{N}t_{\text{op}}
{{< /katex >}}
{{% /colour %}}

The ideal speedup is:

{{% colour "green" %}}
{{< katex display=true >}}
S(N) = \frac{n t_{\text{op}}}{\frac{n}{N}t_{\text{op}}} = N
{{< /katex >}}
{{% /colour %}}

In complexity terms, ideal parallelisation changes `O(n)` work per processor to `O(n/N)`. If `N = n`, each processor handles one element and the ideal parallel time becomes constant.

### Examples

- applying a blur or edge-detection operation to many pixels
- vector addition
- independent row-column calculations in matrix operations
- GPU threads applying the same computation to many elements
- large-scale deep-learning tensor operations

Practical speedup is smaller than the ideal because of memory access, communication, and synchronisation.

## 6. Task-Level Parallelism ☆

Task-level parallelism executes **different independent tasks** at the same time. The tasks may use the same data or different data, and they may perform different operations.

For `m` tasks with execution times `t₁, t₂, ..., tₘ`, sequential time is:

{{% colour "green" %}}
{{< katex display=true >}}
T_{\text{serial}} = \sum_{i=1}^{m} t_i
{{< /katex >}}
{{% /colour %}}

If every task has its own processor and all tasks start together:

{{% colour "green" %}}
{{< katex display=true >}}
T_{\text{parallel}} = \max(t_1,t_2,\ldots,t_m)
{{< /katex >}}
{{% /colour %}}

The longest task determines completion time. This is the **critical path**.

### Worked Example

Suppose three independent tasks take `5`, `3`, and `2` seconds.

- serial time is `5 + 3 + 2 = 10` seconds
- ideal parallel time is `max(5, 3, 2) = 5` seconds
- speedup is `10/5 = 2`

The processors that finish the shorter tasks must wait for the five-second task.

### Load Balance

Average task time is:

{{% colour "green" %}}
{{< katex display=true >}}
\bar{t} = \frac{1}{m}\sum_{i=1}^{m} t_i
{{< /katex >}}
{{% /colour %}}

If every processor receives equal work, the ideal completion time approaches the average. In an imbalanced system, the most heavily loaded processor determines completion time.

When there are more tasks than processors, tasks must be scheduled. If `A_j` is the set of tasks assigned to processor `j`:

{{% colour "green" %}}
{{< katex display=true >}}
T_{\text{parallel}}
= \max_{j=1,\ldots,p}
\left(\sum_{i\in A_j} t_i\right)
{{< /katex >}}
{{% /colour %}}

With overhead:

{{% colour "green" %}}
{{< katex display=true >}}
T_{\text{parallel}}
= T_{\text{critical path}} + T_{\text{overhead}}
{{< /katex >}}
{{% /colour %}}

Good scheduling attempts to distribute total work evenly without creating excessive coordination cost.

## 7. Algorithm-Specific Parallelism ☆

Algorithm-specific parallelism exploits the mathematical structure of an algorithm. Instead of merely dividing data or assigning unrelated tasks, it looks for concurrency within:

- recurrence relations
- divide-and-conquer structure
- algebraically independent operations
- independent recursive subproblems

A common form is:

{{% colour "green" %}}
{{< katex display=true >}}
T_{\text{parallel}}(n)
= \frac{T_{\text{recursive subproblems}}(n)}{N}
+ T_{\text{combine}}(n)
{{< /katex >}}
{{% /colour %}}

The recursive subproblems can run concurrently, while the combine step may remain sequential and limit speedup.

### Parallel Merge Sort

Serial merge sort follows:

{{% colour "green" %}}
{{< katex display=true >}}
T_{\text{serial}}(n)
= 2T\left(\frac{n}{2}\right)+O(n)
{{< /katex >}}
{{% /colour %}}

The two halves can be sorted concurrently, but merging the sorted halves still requires linear work. Parallelisation reduces the time spent in the recursive branches, while the merge step becomes the limiting sequential component.

For an array of eight elements:

1. Divide it into two subarrays of four elements.
2. Sort the two subarrays concurrently.
3. Continue the recursive division within each subarray.
4. Merge the sorted results.

This is Amdahl's Law appearing inside a recursive algorithm: the merge operation limits the overall gain.

### Matrix Multiplication

Matrices can be divided into blocks. Many block products are independent and can execute concurrently.

{{% colour "green" %}}
{{< katex display=true >}}
C = AB,
\qquad
C_{ij} = \sum_k A_{ik}B_{kj}
{{< /katex >}}
{{% /colour %}}

Each output block can be formed from independent block multiplications followed by a combination of partial results.

### Parallel Strassen Algorithm

Strassen's algorithm reduces the number of recursive matrix multiplications from eight to seven:

{{% colour "green" %}}
{{< katex display=true >}}
T_{\text{serial}}(n)
= 7T\left(\frac{n}{2}\right)+O(n^2)
{{< /katex >}}
{{% /colour %}}

If recursive multiplications are distributed across `N` processors:

{{% colour "green" %}}
{{< katex display=true >}}
T_{\text{parallel}}(n)
= \frac{7T\left(\frac{n}{2}\right)}{N}+O(n^2)
{{< /katex >}}
{{% /colour %}}

The recursive multiplications benefit from parallelism, while combining the results remains a limiting component.

## 8. Comparing the Three Paradigms

| Paradigm | What Is Divided? | Operations | Typical Example | Main Limitation |
|---|---|---|---|---|
| Data-level | Data elements | Same operation | Vector addition or image filtering | Dependencies and memory access |
| Task-level | Independent tasks | Different operations are possible | Independent modules or services | Critical path and load imbalance |
| Algorithm-specific | Algorithm structure | Determined by the algorithm | Merge sort or blocked matrix multiplication | Sequential combine step |

The paradigms can be combined. For example, independent tasks may run concurrently while each task uses data-level parallelism internally.

## Common Mistakes

{{% hint warning %}}
- Assuming that `N` processors always produce `N` times speedup.
- Ignoring the sequential fraction when estimating scalability.
- Ignoring communication and synchronisation overhead.
- Confusing data-level parallelism with task-level parallelism.
- Distributing tasks evenly by count while ignoring differences in task duration.
- Assuming that a recursive algorithm is fully parallel because its recursive calls are independent; the combine step may remain sequential.
{{% /hint %}}

## Practice Questions

1. A program takes `120` seconds serially and `35` seconds in parallel. Calculate its speedup.
2. If the result above uses six processors, calculate processor efficiency.
3. Use Amdahl's Law to calculate the speedup when `P = 0.9` and `N = 8`.
4. Find the maximum speedup when `15%` of a program is sequential.
5. Explain why adding processors may reduce efficiency.
6. Compare data-level and task-level parallelism using one example of each.
7. Tasks take `8`, `5`, `4`, and `3` seconds. Find the serial time and the ideal parallel time using four processors.
8. Why does the merge step limit the speedup of parallel merge sort?
9. Explain how matrix multiplication exposes algorithm-specific parallelism.
10. Why can communication overhead make a smaller number of processors faster than a larger number?

## Key Takeaways

{{% hint success %}}
- Parallelisation requires independent work; processors alone do not create parallelism.
- Speedup compares serial and parallel execution times.
- Amdahl's Law shows that the sequential fraction limits maximum speedup.
- Efficiency normally decreases as processors are added.
- Data-level parallelism applies one operation to many data elements.
- Task-level parallelism executes different tasks concurrently.
- Algorithm-specific parallelism extracts concurrency from the mathematical structure of an algorithm.
- Communication, synchronisation, scheduling, and load imbalance determine practical performance.
{{% /hint %}}

## Checklist

- [ ] I can calculate speedup and efficiency.
- [ ] I can apply Amdahl's Law and find maximum speedup.
- [ ] I can distinguish the three parallelisation paradigms.
- [ ] I can identify the critical path in a group of tasks.
- [ ] I can explain load imbalance and parallel overhead.
- [ ] I can describe parallelism in merge sort and matrix multiplication.

---
{{< home-link "Home" >}} | {{< section-index >}}
