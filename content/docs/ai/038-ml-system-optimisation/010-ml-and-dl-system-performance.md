---
title: "ML and DL System Performance"
draft: false
tags: ["ML System Optimization", "MLSysOps", "Performance", "Complexity Analysis", "Throughput", "Latency", "AI", "ML"]
categories: ["AI", "ML"]
weight: 100
menu: main
---

# ML and DL System Performance

Machine learning system optimisation begins with measurement. Before changing an algorithm, adding processors, or moving work to a GPU, we need to understand **what is slow**, **which resource is limiting performance**, and **how performance changes as the workload grows**.

This page covers:

- time and space complexity
- throughput and latency
- the relationship between workload, throughput, and latency
- the main measurements used to describe system performance

## Learning Objectives

By the end of this page, you should be able to:

- explain why performance must be measured before it is optimised
- distinguish time complexity from measured execution time
- calculate the space used by an algorithm
- distinguish throughput from latency
- use Little's Law to connect system load, arrival rate, and response time
- recognise what happens when a system approaches saturation

## Big Picture

{{< mermaid >}}
flowchart TD
    A["Workload"] --> B["Algorithm"]
    B --> C["Execution Time"]
    B --> D["Memory Use"]
    C --> E["Latency"]
    C --> F["Throughput"]
    D --> G["Scalability"]

    style A fill:#E1F5FE
    style B fill:#C8E6C9
    style C fill:#FFF9C4
    style D fill:#EDE7F6
    style E fill:#E1F5FE
    style F fill:#C8E6C9
    style G fill:#FFF9C4
{{< /mermaid >}}

## 1. What Does System Optimisation Mean?

System optimisation is broader than changing the machine-learning model. Performance can be affected by several layers:

- the **algorithm** and its computational structure
- the **software implementation**
- the **operating system** and its support for concurrency
- the **hardware**, including CPUs, GPUs, memory, and clusters

The objective is not simply to make one operation faster. A useful system must balance execution time, memory consumption, throughput, latency, processor utilisation, and cost.

{{% hint info %}}
A simple way to think about optimisation is: **measure the current behaviour, identify the bottleneck, change one part of the system, and measure again.**
{{% /hint %}}

## 2. Time Complexity ☆

Time complexity describes how the amount of computational work grows as the input size grows. It focuses on the structure of the algorithm rather than the speed of a particular computer.

{{% colour "green" %}}
{{< katex display=true >}}
T(n) = \text{work required to process an input of size } n
{{< /katex >}}
{{% /colour %}}

For example, if doubling the input approximately doubles the work, the algorithm has linear growth. If doubling the input approximately quadruples the work, it has quadratic growth.

### Why Time Complexity Matters

Time complexity helps us:

- predict behaviour for large inputs
- compare algorithms independently of a particular machine
- identify algorithms that will not scale to large datasets
- estimate whether additional parallel hardware is likely to help

### Asymptotic Notation

| Notation | Meaning | Typical Interpretation |
|---|---|---|
| Big O | Asymptotic upper bound | The work grows no faster than the stated order |
| Omega | Asymptotic lower bound | The work grows at least as fast as the stated order |
| Theta | Tight asymptotic bound | The work grows at the stated order |

{{% hint warning %}}
Big O, Omega, and Theta describe growth rates. They do not directly give execution time in seconds. Theta is a tight bound; it does not automatically mean the average case.
{{% /hint %}}

### Common Growth Rates

| Complexity | Name | Example |
|---|---|---|
| `O(1)` | Constant | Reading an array element by index |
| `O(log n)` | Logarithmic | Binary search |
| `O(n)` | Linear | Linear search or summing an array |
| `O(n log n)` | Log-linear | Merge sort |
| `O(n²)` | Quadratic | Bubble sort or insertion sort |
| `O(2ⁿ)` | Exponential | Exploring all subsets |
| `O(n!)` | Factorial | Exploring every possible ordering |

For large inputs, the growth rate matters more than small constant differences. An efficient algorithm on modest hardware can outperform an inefficient algorithm on a faster machine.

### Linear Search

In the worst case, linear search compares the target with every element.

{{% colour "green" %}}
{{< katex display=true >}}
T(n) = n = O(n)
{{< /katex >}}
{{% /colour %}}

### Binary Search

Binary search repeatedly halves a sorted search space.

{{% colour "green" %}}
{{< katex display=true >}}
T(n) = O(\log n)
{{< /katex >}}
{{% /colour %}}

The logarithmic growth makes binary search much more scalable than linear search, provided that the data is sorted.

### Recursive Algorithms

The running time of a recursive algorithm can be expressed using a recurrence relation. Merge sort divides the input into two halves and then merges the results:

{{% colour "green" %}}
{{< katex display=true >}}
T(n) = 2T\left(\frac{n}{2}\right) + O(n)
{{< /katex >}}
{{% /colour %}}

The two recursive calls sort the halves, while the linear term represents the merge operation.

## 3. Space Complexity ☆

Space complexity describes how memory requirements grow with input size. It can include:

- **input space** — memory occupied by the input
- **auxiliary space** — temporary data structures used by the algorithm
- **stack space** — memory used by nested or recursive function calls

{{% colour "green" %}}
{{< katex display=true >}}
S(n) = S_{\text{input}} + S_{\text{auxiliary}} + S_{\text{stack}}
{{< /katex >}}
{{% /colour %}}

Space complexity is important when working with large datasets, many concurrent tasks, accelerators with limited memory, or memory-constrained devices.

### Examples

| Algorithm | Main Additional Memory | Space Complexity |
|---|---|---|
| Linear search | A few scalar variables | `O(1)` auxiliary space |
| Merge sort | Temporary array used while merging | `O(n)` |
| In-place quicksort | Recursive call stack | `O(log n)` on average |
| Matrix multiplication | Output matrix | `O(n²)` for an `n × n` result |

Suppose an algorithm uses:

- `n` input elements
- `k` scalar variables
- recursion depth `d`

Its space can be represented as:

{{% colour "green" %}}
{{< katex display=true >}}
S(n) = O(n + k + d)
{{< /katex >}}
{{% /colour %}}

If `k` is constant and `d` does not grow faster than `n`, the dominant term is `n`:

{{% colour "green" %}}
{{< katex display=true >}}
S(n) = O(n)
{{< /katex >}}
{{% /colour %}}

{{% hint info %}}
Time and memory are different resources. An algorithm may be fast but memory-hungry, or memory-efficient but slow. System optimisation often requires a deliberate trade-off between them.
{{% /hint %}}

## 4. Throughput ☆

Throughput is the amount of work completed per unit time. It describes the productivity of the whole system.

{{% colour "green" %}}
{{< katex display=true >}}
\text{Throughput} = \frac{\text{Number of completed tasks}}{\text{Total time}}
{{< /katex >}}
{{% /colour %}}

### Worked Example

Suppose one task takes `0.25` seconds and the system processes `100` tasks sequentially.

{{% colour "green" %}}
{{< katex display=true >}}
\text{Total time} = 100 \times 0.25 = 25 \text{ seconds}
{{< /katex >}}
{{% /colour %}}

{{% colour "green" %}}
{{< katex display=true >}}
\text{Throughput} = \frac{100}{25} = 4 \text{ tasks per second}
{{< /katex >}}
{{% /colour %}}

In an ideal parallel system with `N` identical workers:

{{% colour "green" %}}
{{< katex display=true >}}
\text{Throughput}_{\text{parallel}}
= N \times \text{Throughput}_{\text{single}}
{{< /katex >}}
{{% /colour %}}

Real systems normally achieve less than this ideal because of communication, synchronisation, memory delays, and uneven work distribution.

## 5. Latency ☆

Latency is the time taken by one task from its start to its completion.

{{% colour "green" %}}
{{< katex display=true >}}
\text{Latency} = \text{Finish time} - \text{Start time}
{{< /katex >}}
{{% /colour %}}

If a task starts at `0` seconds and finishes at `2` seconds, its latency is `2` seconds. A second task that starts at `1` second and finishes at `3` seconds also has a latency of `2` seconds.

For an interactive ML service, latency may mean the time between receiving a request and returning a prediction. For a language model, useful latency measurements include:

- **initial response latency** — time before the first generated token
- **inter-token latency** — delay between successive tokens
- **end-to-end latency** — total time for the complete response

### Throughput Is Not Latency

| Metric | Main Question | Example Unit |
|---|---|---|
| Throughput | How much total work is completed? | requests per second |
| Latency | How long does one task take? | milliseconds per request |

Batching may improve throughput because many inputs are processed together, while an individual input may wait longer for the batch to be formed. Therefore, high throughput does not automatically mean low latency.

## 6. Little's Law ☆

Little's Law connects the average number of tasks in a stable system, the average arrival or completion rate, and the average time spent in the system.

{{% colour "green" %}}
{{< katex display=true >}}
L = \lambda W
{{< /katex >}}
{{% /colour %}}

where:

- `L` is the average number of tasks in the system
- `λ` is the average arrival rate, closely related to throughput in a stable system
- `W` is the average time spent in the system, including waiting and processing

Rearranging:

{{% colour "green" %}}
{{< katex display=true >}}
W = \frac{L}{\lambda}
{{< /katex >}}
{{% /colour %}}

### Worked Example

If a service contains an average of `20` requests and completes `10` requests per second, the average time in the system is:

{{% colour "green" %}}
{{< katex display=true >}}
W = \frac{20}{10} = 2 \text{ seconds}
{{< /katex >}}
{{% /colour %}}

This value includes both queueing time and service time.

### Saturation

As the arrival rate approaches the maximum service rate, requests accumulate in the queue. In a simple single-server queue:

{{% colour "green" %}}
{{< katex display=true >}}
W = \frac{1}{\mu - \lambda}
{{< /katex >}}
{{% /colour %}}

where `μ` is the service rate. As `λ` approaches `μ`, the denominator approaches zero and latency grows sharply.

{{% hint warning %}}
Running a system close to 100% utilisation can produce very high queueing delay. A system may report high throughput while its response time becomes unacceptable.
{{% /hint %}}

## 7. Core Performance Measurements

| Measurement | Meaning | What It Reveals |
|---|---|---|
| Time complexity | Growth of algorithmic work | Scalability with input size |
| Space complexity | Growth of memory demand | Memory scalability |
| Throughput | Completed work per unit time | System-wide capacity |
| Latency | Time for one task | Responsiveness |
| Queue length | Work waiting or executing | Degree of congestion |
| Utilisation | Fraction of resource capacity in use | Whether resources are idle or saturated |

No single metric gives a complete picture. The correct optimisation target depends on whether the system is interactive, batch-oriented, memory-limited, compute-limited, or approaching saturation.

## Common Mistakes

{{% hint warning %}}
- Treating Big O as execution time in seconds.
- Assuming a faster processor changes the asymptotic complexity of an algorithm.
- Reporting only average latency and ignoring queueing near saturation.
- Assuming that high throughput always produces low latency.
- Counting only temporary arrays while ignoring recursion-stack memory.
- Optimising a component before measuring whether it is the actual bottleneck.
{{% /hint %}}

## Practice Questions

1. Explain the difference between execution time and time complexity.
2. Compare the time complexities of linear search and binary search.
3. Why does merge sort require additional memory?
4. A system completes `600` requests in `30` seconds. Calculate its throughput.
5. A task starts at `2.5` seconds and completes at `3.1` seconds. Calculate its latency.
6. A stable system contains an average of `48` requests and processes `12` requests per second. Use Little's Law to find the average time in the system.
7. Explain why latency rises sharply when the arrival rate approaches the service rate.
8. Give an example in which batching improves throughput but increases latency.

## Key Takeaways

{{% hint success %}}
- Performance optimisation starts with measurement.
- Time complexity describes growth in computational work; it is not measured wall-clock time.
- Space complexity includes input, auxiliary, and stack memory.
- Throughput measures total productivity, while latency measures the experience of one task.
- Little's Law connects system load, throughput, and average response time.
- Near saturation, queueing delay can increase dramatically.
{{% /hint %}}

## Checklist

- [ ] I can distinguish execution time from time complexity.
- [ ] I can compare common asymptotic growth rates.
- [ ] I can estimate the auxiliary and stack space used by an algorithm.
- [ ] I can calculate throughput and latency.
- [ ] I can apply Little's Law.
- [ ] I can explain why high utilisation may cause high latency.

---
{{< home-link "Home" >}} | {{< section-index >}}
