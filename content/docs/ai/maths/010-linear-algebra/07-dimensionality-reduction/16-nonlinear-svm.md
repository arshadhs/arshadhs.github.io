---
title: "Nonlinear SVM"
date: 2026-05-28
draft: false
weight: 1750
tags: ["MFML", "SVM", "Nonlinear SVM", "Kernel Trick", "Kernel Functions", "XOR"]
categories: ["Mathematical Foundations for Machine Learning"]
---

# Nonlinear SVM

A linear SVM works well when the data can be separated by a straight line or hyperplane.
When the data is not linearly separable in the original input space, nonlinear SVM maps the data to a higher-dimensional feature space where a linear separator may exist.

{{% hint info %}}
**Key takeaway:** Nonlinear SVM uses the kernel trick.
Instead of explicitly mapping \(x\) to \(\phi(x)\), we compute inner products in the feature space using a kernel:
\[
K(x_i,x_j)=\phi(x_i)^T\phi(x_j)
\]
{{% /hint %}}

---


## Nonlinear SVM Idea

{{< mermaid >}}
flowchart TD
    A[Nonlinear Data] --> B[Feature Map or Kernel]
    B --> C[Higher-dimensional Feature Space]
    C --> D[Linear Separation]
    D --> E[Classifier in Original Space]
{{< /mermaid >}}


## Why Nonlinear SVM?

Some datasets cannot be separated by a straight line in the original feature space.

Soft-margin SVM helps when the data is almost separable with some noise.

But if the pattern is fundamentally nonlinear, soft margin may not be enough.

Example:

```text
Original 1D/2D space: not linearly separable
Higher-dimensional feature space: linearly separable
```

The idea is:

{{% colour "green" %}}
\[
x \mapsto \phi(x)
\]
{{% /colour %}}

where \(\phi(x)\) maps the input into a higher-dimensional feature space.

---

## Feature Space View

A nonlinear SVM applies a transformation:

{{% colour "green" %}}
\[
\phi:\mathbb{R}^d \rightarrow \mathbb{R}^D
\]
{{% /colour %}}

where often:

{{% colour "green" %}}
\[
D>d
\]
{{% /colour %}}

Then a linear SVM is trained in the transformed feature space.

The separating hyperplane becomes:

{{% colour "green" %}}
\[
w^T\phi(x)+b=0
\]
{{% /colour %}}

Prediction becomes:

{{% colour "green" %}}
\[
\hat{y}=
\operatorname{sign}(w^T\phi(x)+b)
\]
{{% /colour %}}

---

## Problem with Explicit Feature Maps

Explicitly computing \(\phi(x)\) can be expensive.

For example, a polynomial feature map can greatly increase the number of dimensions.

This may lead to:

- high computational cost
- high memory use
- difficult feature construction
- risk of overfitting

The kernel trick avoids explicit computation of \(\phi(x)\).

---

## Kernel Trick

{{< mermaid >}}
flowchart LR
    A[Dot product x_i dot x_j] --> B[Replace with K x_i x_j]
    B --> C[No explicit phi x needed]
    C --> D[Efficient nonlinear SVM]
{{< /mermaid >}}

The linear SVM dual depends on inner products:

{{% colour "green" %}}
\[
x_i^Tx_j
\]
{{% /colour %}}

In feature space, these become:

{{% colour "green" %}}
\[
\phi(x_i)^T\phi(x_j)
\]
{{% /colour %}}

A kernel function computes this directly:

{{% colour "green" %}}
\[
K(x_i,x_j)=\phi(x_i)^T\phi(x_j)
\]
{{% /colour %}}

So we replace:

{{% colour "green" %}}
\[
x_i^Tx_j
\]
{{% /colour %}}

with:

{{% colour "green" %}}
\[
K(x_i,x_j)
\]
{{% /colour %}}

This is called the **kernel trick**.

---

## Kernelised SVM Dual

The hard-margin dual for linear SVM is:

{{% colour "green" %}}
\[
\max_{\alpha}
\sum_i\alpha_i
-
\frac{1}{2}
\sum_i\sum_j
\alpha_i\alpha_jy_iy_jx_i^Tx_j
\]
{{% /colour %}}

For nonlinear SVM, replace \(x_i^Tx_j\) with \(K(x_i,x_j)\):

{{% colour "green" %}}
\[
\max_{\alpha}
\sum_i\alpha_i
-
\frac{1}{2}
\sum_i\sum_j
\alpha_i\alpha_jy_iy_jK(x_i,x_j)
\]
{{% /colour %}}

subject to:

{{% colour "green" %}}
\[
\alpha_i\ge0
\]
{{% /colour %}}

and:

{{% colour "green" %}}
\[
\sum_i\alpha_i y_i=0
\]
{{% /colour %}}

---

## Kernelised Classifier

The classifier becomes:

{{% colour "green" %}}
\[
\hat{y}
=
\operatorname{sign}
\left(
\sum_i\alpha_i y_iK(x_i,x)+b
\right)
\]
{{% /colour %}}

Only support vectors have non-zero \(\alpha_i\), so practically:

{{% colour "green" %}}
\[
\hat{y}
=
\operatorname{sign}
\left(
\sum_{i\in SV}\alpha_i y_iK(x_i,x)+b
\right)
\]
{{% /colour %}}

where \(SV\) is the set of support vectors.

---

## Kernel Matrix

For training points \(x_1,\ldots,x_n\), the kernel matrix is:

{{% colour "green" %}}
\[
K_{ij}=K(x_i,x_j)
\]
{{% /colour %}}

This matrix stores all pairwise kernel values.

The lecture steps for nonlinear SVM are:

1. Select a kernel function
2. Compute pairwise kernel values between labelled examples
3. Use the kernel matrix to solve for support vectors and \(\alpha\) weights
4. Classify a new point using kernel values with the support vectors

---

## Properties of Kernel Functions

A kernel function:

{{% colour "green" %}}
\[
K(x,y)
\]
{{% /colour %}}

takes two inputs and returns a real value.

It is symmetric:

{{% colour "green" %}}
\[
K(x,y)=K(y,x)
\]
{{% /colour %}}

It corresponds to an inner product in some feature space:

{{% colour "green" %}}
\[
K(x_i,x_j)=\phi(x_i)^T\phi(x_j)
\]
{{% /colour %}}

According to Mercer's theorem, every positive-semidefinite symmetric function is a valid kernel.

---

## Common Kernels

Kernels allow inner products in high-dimensional feature spaces without explicit mapping.

### Linear Kernel

{{% colour "green" %}}
\[
K(x_i,x_j)=x_i^Tx_j
\]
{{% /colour %}}

This gives ordinary linear SVM.

---

### Polynomial Kernel

{{% colour "green" %}}
\[
K(x_i,x_j)=(1+x_i^Tx_j)^p
\]
{{% /colour %}}

where \(p\) is the polynomial degree.

This kernel can model polynomial decision boundaries.

---

### Sigmoid Kernel

{{% colour "green" %}}
\[
K(x_i,x_j)=\tanh(\beta_0x_i^Tx_j+\beta_1)
\]
{{% /colour %}}

This resembles the activation function used in neural networks.

---

### Exponential Distance Kernel

The slides mention kernels of the form:

{{% colour "green" %}}
\[
k(x,x')=\exp(-d(x,x'))
\]
{{% /colour %}}

where \(d(x,x')\) is a distance function.

A common related form is the Gaussian or RBF kernel:

{{% colour "green" %}}
\[
K(x_i,x_j)=
\exp
\left(
-\frac{\|x_i-x_j\|^2}{2\sigma^2}
\right)
\]
{{% /colour %}}

---

## Nonlinear SVM Workflow

{{< mermaid >}}
flowchart LR
    A[Choose Kernel] --> B[Compute Kernel Matrix]
    B --> C[Solve Dual]
    C --> D[Find Support Vectors]
    D --> E[Classify New Point]
{{< /mermaid >}}

---

## XOR Example

The XOR pattern is a classic nonlinear classification problem.

A typical XOR dataset is:

| Point | \(x_1\) | \(x_2\) | Label |
|---|---:|---:|---:|
| \(x_1\) | -1 | -1 | -1 |
| \(x_2\) | -1 | +1 | +1 |
| \(x_3\) | +1 | -1 | +1 |
| \(x_4\) | +1 | +1 | -1 |

This cannot be separated by a single straight line in the original 2D space.

However, using a nonlinear transformation or kernel, it can become separable.

---

## Feature Map for XOR Intuition

A useful feature for XOR is the product:

{{% colour "green" %}}
\[
z=x_1x_2
\]
{{% /colour %}}

For the XOR-style labels above:

| \(x_1\) | \(x_2\) | \(x_1x_2\) | Label |
|---:|---:|---:|---:|
| -1 | -1 | +1 | -1 |
| -1 | +1 | -1 | +1 |
| +1 | -1 | -1 | +1 |
| +1 | +1 | +1 | -1 |

Now the data can be separated based on the transformed feature \(x_1x_2\).

This shows why nonlinear feature spaces help.

---

## Linear vs Nonlinear SVM

| Aspect | Linear SVM | Nonlinear SVM |
|---|---|---|
| Decision boundary | Straight hyperplane | Curved in original space |
| Uses kernel? | Usually no, or linear kernel | Yes |
| Good for | Linearly separable data | Nonlinear patterns |
| Main formula | \(x_i^Tx_j\) | \(K(x_i,x_j)\) |
| Example | simple two-class separation | XOR, circular patterns |

---

## Soft Margin with Nonlinear SVM

Nonlinear SVM can also use soft margins.

The soft-margin kernelised dual is:

{{% colour "green" %}}
\[
\max_{\alpha}
\sum_i\alpha_i
-
\frac{1}{2}
\sum_i\sum_j
\alpha_i\alpha_jy_iy_jK(x_i,x_j)
\]
{{% /colour %}}

subject to:

{{% colour "green" %}}
\[
0\le\alpha_i\le C
\]
{{% /colour %}}

and:

{{% colour "green" %}}
\[
\sum_i\alpha_i y_i=0
\]
{{% /colour %}}

The upper bound \(C\) comes from soft-margin slack variables.

---

## How to Classify a New Example

Suppose \(x_*\) is a new point.

### Step 1: Compute Kernel Values

For each support vector \(x_i\):

{{% colour "green" %}}
\[
K(x_i,x_*)
\]
{{% /colour %}}

### Step 2: Compute Decision Score

{{% colour "green" %}}
\[
s=
\sum_{i\in SV}\alpha_i y_iK(x_i,x_*)+b
\]
{{% /colour %}}

### Step 3: Predict Class

{{% colour "green" %}}
\[
\hat{y}=\operatorname{sign}(s)
\]
{{% /colour %}}

If \(s>0\), predict \(+1\).
If \(s<0\), predict \(-1\).

---

## Exam Template: Kernel SVM Classifier

If the question gives \(\alpha_i\), \(y_i\), support vectors, kernel values and \(b\), use:

{{% colour "green" %}}
\[
f(x)=
\sum_{i\in SV}\alpha_i y_iK(x_i,x)+b
\]
{{% /colour %}}

Then:

{{% colour "green" %}}
\[
\hat{y}=\operatorname{sign}(f(x))
\]
{{% /colour %}}

Do not calculate \(w\) explicitly unless the question asks for it.

---

## Exam Template: Kernel Matrix

If asked to compute a kernel matrix:

### Step 1: List Points

{{% colour "green" %}}
\[
x_1,x_2,\ldots,x_n
\]
{{% /colour %}}

### Step 2: Choose Kernel

For example, polynomial:

{{% colour "green" %}}
\[
K(x_i,x_j)=(1+x_i^Tx_j)^p
\]
{{% /colour %}}

### Step 3: Fill Matrix

{{% colour "green" %}}
\[
K=
\begin{bmatrix}
K(x_1,x_1) & K(x_1,x_2) & \cdots \\
K(x_2,x_1) & K(x_2,x_2) & \cdots \\
\vdots & \vdots & \ddots
\end{bmatrix}
\]
{{% /colour %}}

The matrix is symmetric.

---

## Common Exam Mistakes

| Mistake | Correction |
|---|---|
| Explicitly computing \(\phi(x)\) when not needed | Use the kernel directly |
| Forgetting support vectors only | Sum over support vectors with \(\alpha_i>0\) |
| Replacing \(x_i^Tx_j\) incorrectly | Replace every dot product with \(K(x_i,x_j)\) |
| Confusing linear and polynomial kernels | Linear: \(x_i^Tx_j\), polynomial: \((1+x_i^Tx_j)^p\) |
| Forgetting \(b\) in prediction | Always add \(b\) before taking sign |

---

## Quick Memory Line

```text
Nonlinear data → feature map φ → kernel K → dual uses K → classify by weighted support-vector kernels
```

---

## Source Focus

This page is based mainly on:

- Lecture 16: nonlinear SVM, kernel trick, feature spaces, XOR example and nonlinear SVM steps
- Lecture 14: kernel function definition, Mercer theorem and kernel examples
- Lecture 15: soft-margin SVM and hinge loss background
- Course handout: session 16 and Module 7 nonlinear SVM kernels

---

{{< home-link "Home" >}} | {{< section-index >}}

---
