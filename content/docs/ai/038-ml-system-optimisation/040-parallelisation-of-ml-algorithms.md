---
title: "Parallelisation of ML Algorithms"
draft: false
tags: ["ML System Optimization", "MLSysOps", "K-Means", "Decision Trees", "Random Forest", "SVM", "Parallel Machine Learning", "AI", "ML"]
categories: ["AI", "ML"]
weight: 400
menu: main
---

# Parallelisation of ML Algorithms

Machine-learning algorithms expose different kinds of parallel work. The correct decomposition depends on whether independent work occurs across records, features, trees, clusters, kernel entries, or optimisation steps.

Course coverage:

1. **Problem decomposition** for parallel machine learning
2. **Ensemble methods and XGBoost-style tree ensembles**
3. **Parallel k-means** assignment and centroid reduction
4. **Distributed decision trees and random forests**
5. **Support vector machine parallelisation** using block and MapReduce-style computation

## Learning Objectives

By the end of this page, you should be able to:

- locate independent work in common ML algorithms
- calculate the computational cost of k-means, tree building, random forests, and kernel methods
- explain why random forests are naturally parallel
- distinguish tree-, data-, and feature-level parallelism
- identify reduction, communication, and load-balancing costs
- estimate ideal and practical training speedup

## Big Picture

{{< mermaid >}}
flowchart TD
    A["ML Algorithm"] --> B["Decompose Work"]
    B --> C["Data Parallel"]
    B --> D["Feature or Task Parallel"]
    B --> E["Model Parallel"]
    C --> F["Local Computation"]
    D --> F
    E --> F
    F --> G["Combine Results"]

    style A fill:#E1F5FE
    style B fill:#C8E6C9
    style C fill:#FFF9C4
    style D fill:#EDE7F6
    style E fill:#E1F5FE
    style F fill:#C8E6C9
    style G fill:#FFF9C4
{{< /mermaid >}}

## 1. Problem Decomposition ☆

Parallelisation begins by identifying:

- the unit of independent work
- the data required by each worker
- dependencies between stages
- the partial result produced by each worker
- the operation that combines partial results

A useful performance model is:

{{% colour "green" %}}
{{< katex display=true >}}
T_p = T_{\text{local compute}}
    + T_{\text{communication}}
    + T_{\text{reduction}}
    + T_{\text{imbalance}}
{{< /katex >}}
{{% /colour %}}

An algorithm is a strong candidate for parallelisation when local computation is large, dependencies are few, and partial results are small.

## 2. Parallel k-Means ☆

For `n` points, `k` centroids, `d` features, and `i` iterations, standard Lloyd k-means has approximate time complexity:

{{% colour "green" %}}
{{< katex display=true >}}
T_{\text{k-means}} = O(nkdi)
{{< /katex >}}
{{% /colour %}}

Each iteration contains two stages.

### Assignment Stage

Each point is assigned to its nearest centroid. Point-to-centroid distances are independent, so the data can be divided across workers.

For Euclidean distance:

{{% colour "green" %}}
{{< katex display=true >}}
d(x,c_j) = \sqrt{\sum_{r=1}^{d}(x_r-c_{jr})^2}
{{< /katex >}}
{{% /colour %}}

Ignoring overhead, `p` workers reduce assignment cost to approximately:

{{% colour "green" %}}
{{< katex display=true >}}
O\left(\frac{nkdi}{p}\right)
{{< /katex >}}
{{% /colour %}}

### Centroid-Update Stage

Each worker produces a local sum and count for every cluster. The local statistics are reduced:

{{% colour "green" %}}
{{< katex display=true >}}
c_j = \frac{\sum_{x \in C_j}x}{|C_j|}
{{< /katex >}}
{{% /colour %}}

Only the sums and counts need to be combined; moving all raw points is unnecessary. Nevertheless, each iteration requires a barrier before new centroids can be used.

### Worked Numerical: Assignment Cost

Suppose `n = 120,000`, `k = 8`, `d = 10`, and k-means needs `20` iterations.

{{% colour "green" %}}
{{< katex display=true >}}
nkdi = 120000 \times 8 \times 10 \times 20
     = 192000000
{{< /katex >}}
{{% /colour %}}

With eight perfectly balanced workers, ideal assignment work per worker is `24,000,000` feature-distance operations. If every iteration adds the equivalent of `3,000,000` operations in communication and reduction, the effective work is `27,000,000`, giving approximate speedup:

{{% colour "green" %}}
{{< katex display=true >}}
S = \frac{192000000}{27000000} \approx 7.11
{{< /katex >}}
{{% /colour %}}

## 3. Decision Trees

A decision tree repeatedly chooses a feature and split that best separates the data.

For classification, common impurity measures are:

{{% colour "green" %}}
{{< katex display=true >}}
H(S) = -\sum_{c}p_c\log_2 p_c
{{< /katex >}}
{{% /colour %}}

{{% colour "green" %}}
{{< katex display=true >}}
G(S) = 1-\sum_c p_c^2
{{< /katex >}}
{{% /colour %}}

Information gain for a split is:

{{% colour "green" %}}
{{< katex display=true >}}
IG = H(S) - \sum_v \frac{|S_v|}{|S|}H(S_v)
{{< /katex >}}
{{% /colour %}}

Candidate features or candidate split points can be evaluated concurrently. At deeper levels, independent tree nodes can also be processed in parallel. The upper levels contain few nodes, so they provide less parallel work and may become a bottleneck.

### Worked Numerical: Gini Impurity

A node contains `60` positive and `40` negative examples.

{{% colour "green" %}}
{{< katex display=true >}}
G = 1-(0.6^2+0.4^2)=1-(0.36+0.16)=0.48
{{< /katex >}}
{{% /colour %}}

Workers may calculate candidate-split impurities for different features, after which the best split is selected by a reduction.

## 4. Random Forest Parallelisation ☆

A random forest trains `T` decision trees on bootstrap samples and random feature subsets. Because one tree does not require the parameters of another tree, **tree-level parallelism** is natural.

If one tree takes time `t`, serial training time is approximately:

{{% colour "green" %}}
{{< katex display=true >}}
T_{\text{serial}} = Tt
{{< /katex >}}
{{% /colour %}}

With `p` processors and evenly distributed trees:

{{% colour "green" %}}
{{< katex display=true >}}
T_{\text{parallel}} \approx \left\lceil\frac{T}{p}\right\rceil t + T_{\text{merge}}
{{< /katex >}}
{{% /colour %}}

Prediction is also parallel: each tree predicts independently, followed by majority voting for classification or averaging for regression.

| Technique | Parallel Unit | Advantage | Limitation |
|---|---|---|---|
| Tree-level | Independent trees | Coarse-grained and little communication | Unequal tree times can cause imbalance |
| Data-level | Dataset partitions | Scales beyond one machine's memory | Global model combination is required |
| Feature-level | Candidate features or splits | Accelerates expensive split search | Fine-grained scheduling overhead |

### Worked Numerical: Forest Training

A forest contains `100` trees, each taking `12` seconds. Ten workers each train ten trees, and model collection takes `8` seconds.

{{% colour "green" %}}
{{< katex display=true >}}
T_{\text{serial}} = 100 \times 12 = 1200\text{ s}
{{< /katex >}}
{{% /colour %}}

{{% colour "green" %}}
{{< katex display=true >}}
T_{\text{parallel}} = 10 \times 12 + 8 = 128\text{ s}
{{< /katex >}}
{{% /colour %}}

{{% colour "green" %}}
{{< katex display=true >}}
S = \frac{1200}{128}=9.375,
\qquad E=\frac{9.375}{10}=0.9375
{{< /katex >}}
{{% /colour %}}

The efficiency is `93.75%`.

## 5. Boosted Trees and XGBoost

Boosted trees differ from random forests because each new tree corrects errors made by the current ensemble. Trees are therefore sequential across boosting rounds. Parallel work still exists **within** a round:

- evaluate features and split candidates concurrently
- build histograms over data partitions
- reduce local gradient and Hessian statistics
- process compatible nodes at the same depth concurrently

This distinction is important: random forests expose independence across trees, while boosted trees mainly expose independence within construction of one tree.

## 6. Support Vector Machines

For a kernel SVM, building an `n × n` kernel matrix requires approximately `O(n²d)` work and `O(n²)` storage. Each kernel entry can be computed independently:

{{% colour "green" %}}
{{< katex display=true >}}
K_{ij}=K(x_i,x_j)
{{< /katex >}}
{{% /colour %}}

The matrix can be divided into blocks. Workers compute blocks locally, then the optimisation stage combines the required results. A MapReduce-style design maps blocks to workers and reduces partial support-vector contributions.

{{% hint warning %}}
Parallel kernel computation does not remove quadratic storage. For very large `n`, communication and memory may dominate even when arithmetic scales well.
{{% /hint %}}

## 7. Choosing a Parallel Strategy

| Algorithm | Strongest Source of Parallelism | Synchronisation Point |
|---|---|---|
| k-means | Points and distance calculations | Centroid update each iteration |
| Decision tree | Features, split candidates, nodes | Best-split selection |
| Random forest | Independent trees | Voting or model collection |
| Boosted trees | Features and histograms within a round | End of every boosting round |
| Kernel SVM | Kernel-matrix blocks | Optimisation updates |

## Common Mistakes

{{% hint warning %}}
- Dividing an algorithm without identifying the result-combination step.
- Assuming random-forest tree times are identical.
- Treating boosted trees as independent across boosting rounds.
- Ignoring the centroid-reduction barrier in k-means.
- Claiming that parallel kernel computation removes the SVM memory bottleneck.
{{% /hint %}}

## Practice Questions

1. Why is the k-means assignment stage data parallel?
2. Derive the `O(nkdi)` cost of Lloyd k-means.
3. A forest has `240` trees and `12` workers. Each tree takes `5` seconds and collection takes `10` seconds. Find serial time, parallel time, speedup, and efficiency.
4. Calculate Gini impurity for a node with class proportions `0.75` and `0.25`.
5. Compare tree-level and feature-level random-forest parallelism.
6. Why are boosting rounds less parallel than random-forest trees?
7. A kernel matrix uses `50,000` training points. How many entries does it contain, and why is storage a concern?

## Key Takeaways

{{% hint success %}}
- Problem decomposition must identify both independent work and result combination.
- k-means parallelises distance calculations but synchronises on centroid updates.
- tree split candidates can be evaluated concurrently.
- random forests are naturally parallel across independent trees.
- boosted trees retain sequential dependence across boosting rounds.
- SVM kernel blocks parallelise well, but quadratic memory remains a limit.
{{% /hint %}}

## Checklist

- [ ] I can derive k-means computational complexity.
- [ ] I can calculate entropy or Gini impurity.
- [ ] I can calculate random-forest speedup and efficiency.
- [ ] I can compare tree-, data-, and feature-level parallelism.
- [ ] I can explain why SVM kernel matrices are expensive.

---
{{< home-link "Home" >}} | {{< section-index >}}
