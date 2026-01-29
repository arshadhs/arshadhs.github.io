---
title: "Convex Combination"
date: 2026-01-29
draft: false
tags: ["Mathematics", "Linear Algebra", "Convexity"]
categories: ["AI", "ML"]
weight: 1170
---

# Convex Combination of Two Points

A **convex combination** describes how to form a point between two points using weighted averages.

It is a fundamental building block in several advanced fields:

* **Linear Algebra & Geometry**
* **Optimization Theory**
* **Machine Learning** (Specifically in SVMs, clustering, and data interpolation)

---

Given two points (or vectors) $\mathbf{x}_1, \mathbf{x}_2 \in \mathbb{R}^n$, a convex combination of these points is defined as:

$$\mathbf{x} = \lambda \mathbf{x}_1 + (1 - \lambda)\mathbf{x}_2$$

**Where:**
* $\lambda \in [0, 1]$
* The weights are non-negative.
* The weights sum to exactly $1$.

### Conditions for Convexity
For the combination to be strictly convex, the following must hold:
1.  $\lambda \ge 0$
2.  $1 - \lambda \ge 0$
3.  $\lambda + (1 - \lambda) = 1$

{{% hint info %}}
**Key Insight:** Convex combinations always stay within the "boundary" or the region defined by the original points.
{{% /hint %}}

---

## Geometric Intuition

A convex combination of two points represents the **straight line segment** connecting them.

* **If $\lambda = 1$:** The result is exactly $\mathbf{x}_1$.
* **If $\lambda = 0$:** The result is exactly $\mathbf{x}_2$.
* **If $0 < \lambda < 1$:** The result is a point resting strictly between $\mathbf{x}_1$ and $\mathbf{x}_2$.

---

## Worked Example

Let's calculate a point between two coordinates in 2D space:

**Given:**
$$\mathbf{x}_1 = \begin{bmatrix} 0 \\ 0 \end{bmatrix}, \quad \mathbf{x}_2 = \begin{bmatrix} 2 \\ 2 \end{bmatrix}$$

**Choose $\lambda = 0.25$:**
$$\mathbf{x} = 0.25 \begin{bmatrix} 0 \\ 0 \end{bmatrix} + 0.75 \begin{bmatrix} 2 \\ 2 \end{bmatrix} = \begin{bmatrix} 1.5 \\ 1.5 \end{bmatrix}$$

This resulting point $\mathbf{x}$ lies $75\%$ of the way toward $\mathbf{x}_2$ along the segment.

---

## Convex vs. Linear Combination

| Combination Type | Constraints on Weights | Resulting Space |
| :--- | :--- | :--- |
| **Linear** | Any real numbers | Anywhere in the infinite space |
| **Convex** | Non-negative ($\ge 0$) and sum to $1$ | Only on the line segment between points |

{{% hint warning %}}
Think of a convex combination as a **restricted** linear combination.
{{% /hint %}}

---

## Applications in Machine Learning

* **Support Vector Machines (SVM):** The decision boundaries and margins are derived from combinations of support vectors.
* **Centroid-based Clustering:** In K-Means, the cluster centroid is a convex combination (the mean) of the points assigned to it.
* **Optimization:** Most ML loss functions aim to be "convex," meaning any convex combination of two points on the function lies above or on the graph.
* **Probability:** Mixture models (like GMMs) use weights that sum to 1 to combine different probability distributions.

> A convex combination is a weighted average that ensures the new point never "escapes" the gap between the originals.

---
{{< home-link "Home" >}} | {{< section-index >}}