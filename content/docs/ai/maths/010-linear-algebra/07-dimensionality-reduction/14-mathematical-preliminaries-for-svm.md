---
title: "Mathematical Preliminaries for SVM"
date: 2026-05-28
draft: false
weight: 1740
tags: ["MFML", "SVM", "KKT", "Lagrange Multipliers", "Convex Optimisation", "Kernels"]
categories: ["Mathematical Foundations for Machine Learning"]
---

# Mathematical Preliminaries for SVM

Support Vector Machines use optimisation, geometry and kernels.
Before deriving SVM, we need constrained optimisation, Lagrange multipliers, primal and dual problems, KKT conditions, hyperplanes and kernel functions.

{{% hint info %}}
**Key takeaway:** SVM is built on constrained optimisation.
The hard-margin SVM primal problem is a quadratic optimisation problem with linear inequality constraints.
The dual problem uses Lagrange multipliers and leads naturally to support vectors and kernels.
{{% /hint %}}

- Primal and dual perspectives
- Geometry of margins

---

## Why These Preliminaries Matter

The SVM problem asks for a separating hyperplane with maximum margin.

This becomes an optimisation problem:

- minimise a quadratic function
- subject to inequality constraints
- solve using Lagrange multipliers
- identify active constraints using KKT conditions
- use kernels when data is not linearly separable

---

## General Constrained Optimisation Problem

The standard form is:

{{% colour "green" %}}
\[
\min_x f(x)
\]
{{% /colour %}}

subject to:

{{% colour "green" %}}
\[
g_i(x) \le 0,\quad i=1,2,\ldots,m
\]
{{% /colour %}}

and:

{{% colour "green" %}}
\[
h_j(x)=0,\quad j=1,2,\ldots,p
\]
{{% /colour %}}

Here:

- \(f(x)\) is the objective function
- \(g_i(x)\) are inequality constraints
- \(h_j(x)\) are equality constraints

---

## Lagrangian

The Lagrangian combines the objective and constraints into one expression.

{{% colour "green" %}}
\[
\mathcal{L}(x,\lambda,\nu)
=
f(x)+\sum_{i=1}^{m}\lambda_i g_i(x)+\sum_{j=1}^{p}\nu_j h_j(x)
\]
{{% /colour %}}

where:

- \(\lambda_i\) are Lagrange multipliers for inequality constraints
- \(\nu_j\) are Lagrange multipliers for equality constraints

For inequality constraints:

{{% colour "green" %}}
\[
\lambda_i \ge 0
\]
{{% /colour %}}

For equality constraints, \(\nu_j\) is unrestricted.

---

## Inequality Constraints and Sign Convention

If the constraint is written as:

{{% colour "green" %}}
\[
g_i(x)\le 0
\]
{{% /colour %}}

then the multiplier satisfies:

{{% colour "green" %}}
\[
\lambda_i \ge 0
\]
{{% /colour %}}

This is the convention used in convex optimisation.

In SVM derivations, constraints may be written in different equivalent forms.
Always check the sign before writing the Lagrangian.

---

## Primal Problem

The **primal problem** is the original constrained optimisation problem.

{{% colour "green" %}}
\[
\min_x f(x)
\]
{{% /colour %}}

subject to:

{{% colour "green" %}}
\[
g_i(x)\le0
\]
{{% /colour %}}

The optimisation is performed over the original variables \(x\).
These are called **primal variables**.

---

## Dual Problem

The dual function is:

{{% colour "green" %}}
\[
D(\lambda)=\min_x \mathcal{L}(x,\lambda)
\]
{{% /colour %}}

The dual problem is:

{{% colour "green" %}}
\[
\max_{\lambda} D(\lambda)
\]
{{% /colour %}}

subject to:

{{% colour "green" %}}
\[
\lambda \ge 0
\]
{{% /colour %}}

The variables \(\lambda\) are called **dual variables**.

---

## Weak Duality

Weak duality says:

{{% colour "green" %}}
\[
\text{dual optimum} \le \text{primal optimum}
\]
{{% /colour %}}

For minimisation problems, the dual gives a lower bound on the primal optimum.

In the lecture slides, this appears through the minimax inequality:

{{% colour "green" %}}
\[
\max_y \min_x \phi(x,y)
\le
\min_x \max_y \phi(x,y)
\]
{{% /colour %}}

Weak duality always holds under broad conditions.

---

## Strong Duality

Strong duality says:

{{% colour "green" %}}
\[
\text{dual optimum} = \text{primal optimum}
\]
{{% /colour %}}

This means we can solve the dual problem and get the same optimal value as the primal problem.

This is useful in SVM because:

- the dual depends on inner products between data points
- inner products can be replaced by kernels
- the solution depends only on support vectors

---

## Slater's Condition

Slater's condition gives a common case where strong duality holds.

For a convex optimisation problem, Slater's condition holds if:

1. the objective function \(f\) is convex
2. the inequality constraints \(g_i\) are convex
3. the equality constraints \(h_j\) are affine or linear
4. there exists a strictly feasible point \(\bar{x}\) such that:

{{% colour "green" %}}
\[
g_i(\bar{x}) < 0
\]
{{% /colour %}}

and:

{{% colour "green" %}}
\[
h_j(\bar{x}) = 0
\]
{{% /colour %}}

If Slater's condition holds, then strong duality holds.

---

## KKT Conditions

The Karush-Kuhn-Tucker conditions are the main optimality conditions for constrained optimisation.

For:

{{% colour "green" %}}
\[
\min f(x)
\]
{{% /colour %}}

subject to:

{{% colour "green" %}}
\[
g_i(x)\le0,\quad h_j(x)=0
\]
{{% /colour %}}

the KKT conditions are:

### 1. Primal Feasibility

{{% colour "green" %}}
\[
g_i(x^*)\le0
\]
{{% /colour %}}

and:

{{% colour "green" %}}
\[
h_j(x^*)=0
\]
{{% /colour %}}

### 2. Dual Feasibility

{{% colour "green" %}}
\[
\lambda_i^*\ge0
\]
{{% /colour %}}

### 3. Complementary Slackness

{{% colour "green" %}}
\[
\lambda_i^*g_i(x^*)=0
\]
{{% /colour %}}

### 4. Stationarity

{{% colour "green" %}}
\[
\nabla f(x^*)+\sum_{i=1}^{m}\lambda_i^*\nabla g_i(x^*)+\sum_{j=1}^{p}\nu_j^*\nabla h_j(x^*)=0
\]
{{% /colour %}}

---

## Complementary Slackness Intuition

Complementary slackness is very important for SVM.

{{% colour "green" %}}
\[
\lambda_i g_i(x)=0
\]
{{% /colour %}}

This means:

| Case | Meaning |
|---|---|
| \(\lambda_i=0\) | constraint is not active |
| \(g_i(x)=0\) | constraint is active |
| \(\lambda_i>0\) | point lies exactly on the active boundary |

In SVM, points with non-zero Lagrange multipliers become **support vectors**.

---

## Quadratic Programming

SVM optimisation is a quadratic programming problem.

A standard quadratic programme is:

{{% colour "green" %}}
\[
\min_{x\in\mathbb{R}^d}
\frac{1}{2}x^TQx+c^Tx
\]
{{% /colour %}}

subject to:

{{% colour "green" %}}
\[
Ax\le b
\]
{{% /colour %}}

The SVM primal has this structure because it minimises:

{{% colour "green" %}}
\[
\frac{1}{2}\|w\|^2
\]
{{% /colour %}}

subject to linear constraints involving training points.

---

## Hyperplane

A hyperplane is a generalised line or plane.

In two dimensions, it is a line.
In three dimensions, it is a plane.
In \(d\) dimensions, it is a \((d-1)\)-dimensional hyperplane.

The equation is:

{{% colour "green" %}}
\[
w^Tx+b=0
\]
{{% /colour %}}

where:

- \(w\) is the normal vector
- \(b\) is the bias or intercept
- \(x\) is the input vector

---

## Why \(w\) Is Orthogonal to the Hyperplane

Let \(x_a\) and \(x_b\) lie on the hyperplane.

Then:

{{% colour "green" %}}
\[
w^Tx_a+b=0
\]
{{% /colour %}}

and:

{{% colour "green" %}}
\[
w^Tx_b+b=0
\]
{{% /colour %}}

Subtract:

{{% colour "green" %}}
\[
w^T(x_a-x_b)=0
\]
{{% /colour %}}

So \(w\) is orthogonal to any vector lying inside the hyperplane.

This is why \(w\) determines the orientation of the separating hyperplane.

---

## Distance from a Point to a Hyperplane

For the hyperplane:

{{% colour "green" %}}
\[
w^Tx+b=0
\]
{{% /colour %}}

the distance of a point \(x_0\) from the hyperplane is:

{{% colour "green" %}}
\[
D=\frac{|w^Tx_0+b|}{\|w\|}
\]
{{% /colour %}}

This formula is used to derive the SVM margin.

---

## Linear Classifier

A linear classifier predicts using:

{{% colour "green" %}}
\[
f(x)=w^Tx+b
\]
{{% /colour %}}

The predicted class is:

{{% colour "green" %}}
\[
\hat{y}=\operatorname{sign}(w^Tx+b)
\]
{{% /colour %}}

If:

{{% colour "green" %}}
\[
w^Tx+b>0
\]
{{% /colour %}}

predict positive class.

If:

{{% colour "green" %}}
\[
w^Tx+b<0
\]
{{% /colour %}}

predict negative class.

---

## Kernel Function

A kernel is a function:

{{% colour "green" %}}
\[
K(x,y)
\]
{{% /colour %}}

that takes two inputs and returns a real number.

The lecture defines kernels as continuous functions that are symmetric:

{{% colour "green" %}}
\[
K(x,y)=K(y,x)
\]
{{% /colour %}}

A kernel corresponds to an inner product in some feature space:

{{% colour "green" %}}
\[
K(x_i,x_j)=\phi(x_i)^T\phi(x_j)
\]
{{% /colour %}}

where \(\phi(x)\) maps the original data to a higher-dimensional feature space.

---

## Why Kernels Matter for SVM

Linear SVM relies on dot products:

{{% colour "green" %}}
\[
x_i^Tx_j
\]
{{% /colour %}}

If data is mapped into a higher-dimensional space:

{{% colour "green" %}}
\[
x \mapsto \phi(x)
\]
{{% /colour %}}

then dot products become:

{{% colour "green" %}}
\[
\phi(x_i)^T\phi(x_j)
\]
{{% /colour %}}

The kernel trick replaces this with:

{{% colour "green" %}}
\[
K(x_i,x_j)
\]
{{% /colour %}}

So we do not need to explicitly compute \(\phi(x)\).

---

## Mercer's Theorem

The slides state:

> Every positive-semidefinite symmetric function is a kernel function.

This means that if a function is symmetric and positive semidefinite, it can be used as a valid kernel.

---

## Common Kernel Examples

### Linear Kernel

{{% colour "green" %}}
\[
K(x_i,x_j)=x_i^Tx_j
\]
{{% /colour %}}

### Polynomial Kernel

{{% colour "green" %}}
\[
K(x_i,x_j)=(1+x_i^Tx_j)^p
\]
{{% /colour %}}

### Sigmoid Kernel

{{% colour "green" %}}
\[
K(x_i,x_j)=\tanh(\beta_0x_i^Tx_j+\beta_1)
\]
{{% /colour %}}

### RBF-like Distance Kernel

The slides also mention kernels built from distances:

{{% colour "green" %}}
\[
k(x,x')=\exp(-d(x,x'))
\]
{{% /colour %}}

where \(d(x,x')\) is a distance function.

---

## Exam Method: KKT Setup

When asked to write KKT conditions, use this template.

Given:

{{% colour "green" %}}
\[
\min f(x)
\]
{{% /colour %}}

subject to:

{{% colour "green" %}}
\[
g(x)\le0
\]
{{% /colour %}}

write:

### Lagrangian

{{% colour "green" %}}
\[
\mathcal{L}(x,\lambda)=f(x)+\lambda g(x)
\]
{{% /colour %}}

### KKT Conditions

{{% colour "green" %}}
\[
\nabla_x\mathcal{L}(x,\lambda)=0
\]
{{% /colour %}}

{{% colour "green" %}}
\[
g(x)\le0
\]
{{% /colour %}}

{{% colour "green" %}}
\[
\lambda\ge0
\]
{{% /colour %}}

{{% colour "green" %}}
\[
\lambda g(x)=0
\]
{{% /colour %}}

This often gives marks even if the full solution is difficult.

---

## Quick Memory Line

```text
Constrained optimisation → Lagrangian → Dual → KKT → Hyperplane → Kernel
```

---

## Source Focus

This page is based mainly on:

- Lecture 11: constrained optimisation, Lagrange multipliers, primal and dual problems, weak duality, convex optimisation, quadratic programming
- Lecture 14: KKT conditions, hyperplanes, kernel functions and linear classifiers
- Course handout: session 14 and Module 7 SVM preliminaries

---

## Related Pages

- [My Notes: Primal/Dual Perspective for Linear SVM]({{% relref "15-primal-dual-perspective-for-linear-svm.md" %}})
- [My Notes: Nonlinear SVM]({{% relref "16-nonlinear-svm.md" %}})
- [My Notes: Dimensionality Reduction and PCA]({{% relref "12-dimensionality-reduction-and-pca.md" %}})

---

{{< home-link "Home" >}} | {{< section-index >}}

---
