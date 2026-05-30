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


## SVM Preliminaries Map

{{< mermaid >}}
flowchart TD
    A[SVM Preliminaries] --> B[Constrained Optimisation]
    A --> C[Primal and Dual]
    A --> D[KKT Conditions]
    A --> E[Hyperplanes]
    A --> F[Kernel Functions]
    style A fill:#E1F5FE,stroke:#78909C,stroke-width:1px,color:#263238
    style B fill:#C8E6C9,stroke:#78909C,stroke-width:1px,color:#263238
    style C fill:#FFF9C4,stroke:#78909C,stroke-width:1px,color:#263238
    style D fill:#EDE7F6,stroke:#78909C,stroke-width:1px,color:#263238
    style E fill:#E1F5FE,stroke:#78909C,stroke-width:1px,color:#263238
    style F fill:#C8E6C9,stroke:#78909C,stroke-width:1px,color:#263238
{{< /mermaid >}}


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
{{< katex display=true >}}
\min_x f(x)
{{< /katex >}}
{{% /colour %}}

subject to:

{{% colour "green" %}}
{{< katex display=true >}}
g_i(x) \le 0,\quad i=1,2,\ldots,m
{{< /katex >}}
{{% /colour %}}

and:

{{% colour "green" %}}
{{< katex display=true >}}
h_j(x)=0,\quad j=1,2,\ldots,p
{{< /katex >}}
{{% /colour %}}

Here:

- {{< katex >}}f(x){{< /katex >}} is the objective function
- {{< katex >}}g_i(x){{< /katex >}} are inequality constraints
- {{< katex >}}h_j(x){{< /katex >}} are equality constraints

---

## Lagrangian

The Lagrangian combines the objective and constraints into one expression.

{{% colour "green" %}}
{{< katex display=true >}}
\mathcal{L}(x,\lambda,\nu)
=
f(x)+\sum_{i=1}^{m}\lambda_i g_i(x)+\sum_{j=1}^{p}\nu_j h_j(x)
{{< /katex >}}
{{% /colour %}}

where:

- {{< katex >}}\lambda_i{{< /katex >}} are Lagrange multipliers for inequality constraints
- {{< katex >}}\nu_j{{< /katex >}} are Lagrange multipliers for equality constraints

For inequality constraints:

{{% colour "green" %}}
{{< katex display=true >}}
\lambda_i \ge 0
{{< /katex >}}
{{% /colour %}}

For equality constraints, {{< katex >}}\nu_j{{< /katex >}} is unrestricted.

---

## Inequality Constraints and Sign Convention

If the constraint is written as:

{{% colour "green" %}}
{{< katex display=true >}}
g_i(x)\le 0
{{< /katex >}}
{{% /colour %}}

then the multiplier satisfies:

{{% colour "green" %}}
{{< katex display=true >}}
\lambda_i \ge 0
{{< /katex >}}
{{% /colour %}}

This is the convention used in convex optimisation.

In SVM derivations, constraints may be written in different equivalent forms.
Always check the sign before writing the Lagrangian.

---

## Primal Problem

The **primal problem** is the original constrained optimisation problem.

{{% colour "green" %}}
{{< katex display=true >}}
\min_x f(x)
{{< /katex >}}
{{% /colour %}}

subject to:

{{% colour "green" %}}
{{< katex display=true >}}
g_i(x)\le0
{{< /katex >}}
{{% /colour %}}

The optimisation is performed over the original variables {{< katex >}}x{{< /katex >}}.
These are called **primal variables**.

---

## Dual Problem

The dual function is:

{{% colour "green" %}}
{{< katex display=true >}}
D(\lambda)=\min_x \mathcal{L}(x,\lambda)
{{< /katex >}}
{{% /colour %}}

The dual problem is:

{{% colour "green" %}}
{{< katex display=true >}}
\max_{\lambda} D(\lambda)
{{< /katex >}}
{{% /colour %}}

subject to:

{{% colour "green" %}}
{{< katex display=true >}}
\lambda \ge 0
{{< /katex >}}
{{% /colour %}}

The variables {{< katex >}}\lambda{{< /katex >}} are called **dual variables**.

---

## Weak Duality

Weak duality says:

{{% colour "green" %}}
{{< katex display=true >}}
\text{dual optimum} \le \text{primal optimum}
{{< /katex >}}
{{% /colour %}}

For minimisation problems, the dual gives a lower bound on the primal optimum.

In the lecture slides, this appears through the minimax inequality:

{{% colour "green" %}}
{{< katex display=true >}}
\max_y \min_x \phi(x,y)
\le
\min_x \max_y \phi(x,y)
{{< /katex >}}
{{% /colour %}}

Weak duality always holds under broad conditions.

---

## Strong Duality

Strong duality says:

{{% colour "green" %}}
{{< katex display=true >}}
\text{dual optimum} = \text{primal optimum}
{{< /katex >}}
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

1. the objective function {{< katex >}}f{{< /katex >}} is convex
2. the inequality constraints {{< katex >}}g_i{{< /katex >}} are convex
3. the equality constraints {{< katex >}}h_j{{< /katex >}} are affine or linear
4. there exists a strictly feasible point {{< katex >}}\bar{x}{{< /katex >}} such that:

{{% colour "green" %}}
{{< katex display=true >}}
g_i(\bar{x}) < 0
{{< /katex >}}
{{% /colour %}}

and:

{{% colour "green" %}}
{{< katex display=true >}}
h_j(\bar{x}) = 0
{{< /katex >}}
{{% /colour %}}

If Slater's condition holds, then strong duality holds.

---

## KKT Conditions

The Karush-Kuhn-Tucker conditions are the main optimality conditions for constrained optimisation.

For:

{{% colour "green" %}}
{{< katex display=true >}}
\min f(x)
{{< /katex >}}
{{% /colour %}}

subject to:

{{% colour "green" %}}
{{< katex display=true >}}
g_i(x)\le0,\quad h_j(x)=0
{{< /katex >}}
{{% /colour %}}

the KKT conditions are:

### 1. Primal Feasibility

{{% colour "green" %}}
{{< katex display=true >}}
g_i(x^*)\le0
{{< /katex >}}
{{% /colour %}}

and:

{{% colour "green" %}}
{{< katex display=true >}}
h_j(x^*)=0
{{< /katex >}}
{{% /colour %}}

### 2. Dual Feasibility

{{% colour "green" %}}
{{< katex display=true >}}
\lambda_i^*\ge0
{{< /katex >}}
{{% /colour %}}

### 3. Complementary Slackness

{{% colour "green" %}}
{{< katex display=true >}}
\lambda_i^*g_i(x^*)=0
{{< /katex >}}
{{% /colour %}}

### 4. Stationarity

{{% colour "green" %}}
{{< katex display=true >}}
\nabla f(x^*)+\sum_{i=1}^{m}\lambda_i^*\nabla g_i(x^*)+\sum_{j=1}^{p}\nu_j^*\nabla h_j(x^*)=0
{{< /katex >}}
{{% /colour %}}

---

## Complementary Slackness Intuition

Complementary slackness is very important for SVM.

{{% colour "green" %}}
{{< katex display=true >}}
\lambda_i g_i(x)=0
{{< /katex >}}
{{% /colour %}}

This means:

| Case | Meaning |
|---|---|
| {{< katex >}}\lambda_i=0{{< /katex >}} | constraint is not active |
| {{< katex >}}g_i(x)=0{{< /katex >}} | constraint is active |
| {{< katex >}}\lambda_i>0{{< /katex >}} | point lies exactly on the active boundary |

In SVM, points with non-zero Lagrange multipliers become **support vectors**.

---

## Quadratic Programming

SVM optimisation is a quadratic programming problem.

A standard quadratic programme is:

{{% colour "green" %}}
{{< katex display=true >}}
\min_{x\in\mathbb{R}^d}
\frac{1}{2}x^TQx+c^Tx
{{< /katex >}}
{{% /colour %}}

subject to:

{{% colour "green" %}}
{{< katex display=true >}}
Ax\le b
{{< /katex >}}
{{% /colour %}}

The SVM primal has this structure because it minimises:

{{% colour "green" %}}
{{< katex display=true >}}
\frac{1}{2}\|w\|^2
{{< /katex >}}
{{% /colour %}}

subject to linear constraints involving training points.

---

## Hyperplane

A hyperplane is a generalised line or plane.

In two dimensions, it is a line.
In three dimensions, it is a plane.
In {{< katex >}}d{{< /katex >}} dimensions, it is a {{< katex >}}(d-1){{< /katex >}}-dimensional hyperplane.

The equation is:

{{% colour "green" %}}
{{< katex display=true >}}
w^Tx+b=0
{{< /katex >}}
{{% /colour %}}

where:

- {{< katex >}}w{{< /katex >}} is the normal vector
- {{< katex >}}b{{< /katex >}} is the bias or intercept
- {{< katex >}}x{{< /katex >}} is the input vector

---

## Why {{< katex >}}w{{< /katex >}} Is Orthogonal to the Hyperplane

Let {{< katex >}}x_a{{< /katex >}} and {{< katex >}}x_b{{< /katex >}} lie on the hyperplane.

Then:

{{% colour "green" %}}
{{< katex display=true >}}
w^Tx_a+b=0
{{< /katex >}}
{{% /colour %}}

and:

{{% colour "green" %}}
{{< katex display=true >}}
w^Tx_b+b=0
{{< /katex >}}
{{% /colour %}}

Subtract:

{{% colour "green" %}}
{{< katex display=true >}}
w^T(x_a-x_b)=0
{{< /katex >}}
{{% /colour %}}

So {{< katex >}}w{{< /katex >}} is orthogonal to any vector lying inside the hyperplane.

This is why {{< katex >}}w{{< /katex >}} determines the orientation of the separating hyperplane.

---

## Distance from a Point to a Hyperplane

For the hyperplane:

{{% colour "green" %}}
{{< katex display=true >}}
w^Tx+b=0
{{< /katex >}}
{{% /colour %}}

the distance of a point {{< katex >}}x_0{{< /katex >}} from the hyperplane is:

{{% colour "green" %}}
{{< katex display=true >}}
D=\frac{|w^Tx_0+b|}{\|w\|}
{{< /katex >}}
{{% /colour %}}

This formula is used to derive the SVM margin.

---

## Linear Classifier

A linear classifier predicts using:

{{% colour "green" %}}
{{< katex display=true >}}
f(x)=w^Tx+b
{{< /katex >}}
{{% /colour %}}

The predicted class is:

{{% colour "green" %}}
{{< katex display=true >}}
\hat{y}=\operatorname{sign}(w^Tx+b)
{{< /katex >}}
{{% /colour %}}

If:

{{% colour "green" %}}
{{< katex display=true >}}
w^Tx+b>0
{{< /katex >}}
{{% /colour %}}

predict positive class.

If:

{{% colour "green" %}}
{{< katex display=true >}}
w^Tx+b<0
{{< /katex >}}
{{% /colour %}}

predict negative class.

---

## Kernel Function

A kernel is a function:

{{% colour "green" %}}
{{< katex display=true >}}
K(x,y)
{{< /katex >}}
{{% /colour %}}

that takes two inputs and returns a real number.

The lecture defines kernels as continuous functions that are symmetric:

{{% colour "green" %}}
{{< katex display=true >}}
K(x,y)=K(y,x)
{{< /katex >}}
{{% /colour %}}

A kernel corresponds to an inner product in some feature space:

{{% colour "green" %}}
{{< katex display=true >}}
K(x_i,x_j)=\phi(x_i)^T\phi(x_j)
{{< /katex >}}
{{% /colour %}}

where {{< katex >}}\phi(x){{< /katex >}} maps the original data to a higher-dimensional feature space.

---

## Why Kernels Matter for SVM

Linear SVM relies on dot products:

{{% colour "green" %}}
{{< katex display=true >}}
x_i^Tx_j
{{< /katex >}}
{{% /colour %}}

If data is mapped into a higher-dimensional space:

{{% colour "green" %}}
{{< katex display=true >}}
x \mapsto \phi(x)
{{< /katex >}}
{{% /colour %}}

then dot products become:

{{% colour "green" %}}
{{< katex display=true >}}
\phi(x_i)^T\phi(x_j)
{{< /katex >}}
{{% /colour %}}

The kernel trick replaces this with:

{{% colour "green" %}}
{{< katex display=true >}}
K(x_i,x_j)
{{< /katex >}}
{{% /colour %}}

So we do not need to explicitly compute {{< katex >}}\phi(x){{< /katex >}}.

---

## Mercer's Theorem

The slides state:

> Every positive-semidefinite symmetric function is a kernel function.

This means that if a function is symmetric and positive semidefinite, it can be used as a valid kernel.

---

## Common Kernel Examples

### Linear Kernel

{{% colour "green" %}}
{{< katex display=true >}}
K(x_i,x_j)=x_i^Tx_j
{{< /katex >}}
{{% /colour %}}

### Polynomial Kernel

{{% colour "green" %}}
{{< katex display=true >}}
K(x_i,x_j)=(1+x_i^Tx_j)^p
{{< /katex >}}
{{% /colour %}}

### Sigmoid Kernel

{{% colour "green" %}}
{{< katex display=true >}}
K(x_i,x_j)=\tanh(\beta_0x_i^Tx_j+\beta_1)
{{< /katex >}}
{{% /colour %}}

### RBF-like Distance Kernel

The slides also mention kernels built from distances:

{{% colour "green" %}}
{{< katex display=true >}}
k(x,x')=\exp(-d(x,x'))
{{< /katex >}}
{{% /colour %}}

where {{< katex >}}d(x,x'){{< /katex >}} is a distance function.

---

## Exam Method: KKT Setup

When asked to write KKT conditions, use this template.

Given:

{{% colour "green" %}}
{{< katex display=true >}}
\min f(x)
{{< /katex >}}
{{% /colour %}}

subject to:

{{% colour "green" %}}
{{< katex display=true >}}
g(x)\le0
{{< /katex >}}
{{% /colour %}}

write:

### Lagrangian

{{% colour "green" %}}
{{< katex display=true >}}
\mathcal{L}(x,\lambda)=f(x)+\lambda g(x)
{{< /katex >}}
{{% /colour %}}

### KKT Conditions

{{% colour "green" %}}
{{< katex display=true >}}
\nabla_x\mathcal{L}(x,\lambda)=0
{{< /katex >}}
{{% /colour %}}

{{% colour "green" %}}
{{< katex display=true >}}
g(x)\le0
{{< /katex >}}
{{% /colour %}}

{{% colour "green" %}}
{{< katex display=true >}}
\lambda\ge0
{{< /katex >}}
{{% /colour %}}

{{% colour "green" %}}
{{< katex display=true >}}
\lambda g(x)=0
{{< /katex >}}
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

{{< home-link "Home" >}} | {{< section-index >}}15-primal

---
