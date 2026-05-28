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
Instead of explicitly mapping {{< katex >}}x{{< /katex >}} to {{< katex >}}\phi(x){{< /katex >}}, we compute inner products in the feature space using a kernel:
{{< katex display=true >}}
K(x_i,x_j)=\phi(x_i)^T\phi(x_j)
{{< /katex >}}
{{% /hint %}}

---

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
{{< katex display=true >}}
x \mapsto \phi(x)
{{< /katex >}}
{{% /colour %}}

where {{< katex >}}\phi(x){{< /katex >}} maps the input into a higher-dimensional feature space.

---

## Feature Space View

A nonlinear SVM applies a transformation:

{{% colour "green" %}}
{{< katex display=true >}}
\phi:\mathbb{R}^d \rightarrow \mathbb{R}^D
{{< /katex >}}
{{% /colour %}}

where often:

{{% colour "green" %}}
{{< katex display=true >}}
D>d
{{< /katex >}}
{{% /colour %}}

Then a linear SVM is trained in the transformed feature space.

The separating hyperplane becomes:

{{% colour "green" %}}
{{< katex display=true >}}
w^T\phi(x)+b=0
{{< /katex >}}
{{% /colour %}}

Prediction becomes:

{{% colour "green" %}}
{{< katex display=true >}}
\hat{y}=
\operatorname{sign}(w^T\phi(x)+b)
{{< /katex >}}
{{% /colour %}}

---

## Problem with Explicit Feature Maps

Explicitly computing {{< katex >}}\phi(x){{< /katex >}} can be expensive.

For example, a polynomial feature map can greatly increase the number of dimensions.

This may lead to:

- high computational cost
- high memory use
- difficult feature construction
- risk of overfitting

The kernel trick avoids explicit computation of {{< katex >}}\phi(x){{< /katex >}}.

---

## Kernel Trick

The linear SVM dual depends on inner products:

{{% colour "green" %}}
{{< katex display=true >}}
x_i^Tx_j
{{< /katex >}}
{{% /colour %}}

In feature space, these become:

{{% colour "green" %}}
{{< katex display=true >}}
\phi(x_i)^T\phi(x_j)
{{< /katex >}}
{{% /colour %}}

A kernel function computes this directly:

{{% colour "green" %}}
{{< katex display=true >}}
K(x_i,x_j)=\phi(x_i)^T\phi(x_j)
{{< /katex >}}
{{% /colour %}}

So we replace:

{{% colour "green" %}}
{{< katex display=true >}}
x_i^Tx_j
{{< /katex >}}
{{% /colour %}}

with:

{{% colour "green" %}}
{{< katex display=true >}}
K(x_i,x_j)
{{< /katex >}}
{{% /colour %}}

This is called the **kernel trick**.

---

## Kernelised SVM Dual

The hard-margin dual for linear SVM is:

{{% colour "green" %}}
{{< katex display=true >}}
\max_{\alpha}
\sum_i\alpha_i
-
\frac{1}{2}
\sum_i\sum_j
\alpha_i\alpha_jy_iy_jx_i^Tx_j
{{< /katex >}}
{{% /colour %}}

For nonlinear SVM, replace {{< katex >}}x_i^Tx_j{{< /katex >}} with {{< katex >}}K(x_i,x_j){{< /katex >}}:

{{% colour "green" %}}
{{< katex display=true >}}
\max_{\alpha}
\sum_i\alpha_i
-
\frac{1}{2}
\sum_i\sum_j
\alpha_i\alpha_jy_iy_jK(x_i,x_j)
{{< /katex >}}
{{% /colour %}}

subject to:

{{% colour "green" %}}
{{< katex display=true >}}
\alpha_i\ge0
{{< /katex >}}
{{% /colour %}}

and:

{{% colour "green" %}}
{{< katex display=true >}}
\sum_i\alpha_i y_i=0
{{< /katex >}}
{{% /colour %}}

---

## Kernelised Classifier

The classifier becomes:

{{% colour "green" %}}
{{< katex display=true >}}
\hat{y}
=
\operatorname{sign}
\left(
\sum_i\alpha_i y_iK(x_i,x)+b
\right)
{{< /katex >}}
{{% /colour %}}

Only support vectors have non-zero {{< katex >}}\alpha_i{{< /katex >}}, so practically:

{{% colour "green" %}}
{{< katex display=true >}}
\hat{y}
=
\operatorname{sign}
\left(
\sum_{i\in SV}\alpha_i y_iK(x_i,x)+b
\right)
{{< /katex >}}
{{% /colour %}}

where {{< katex >}}SV{{< /katex >}} is the set of support vectors.

---

## Kernel Matrix

For training points {{< katex >}}x_1,\ldots,x_n{{< /katex >}}, the kernel matrix is:

{{% colour "green" %}}
{{< katex display=true >}}
K_{ij}=K(x_i,x_j)
{{< /katex >}}
{{% /colour %}}

This matrix stores all pairwise kernel values.

The lecture steps for nonlinear SVM are:

1. Select a kernel function
2. Compute pairwise kernel values between labelled examples
3. Use the kernel matrix to solve for support vectors and {{< katex >}}\alpha{{< /katex >}} weights
4. Classify a new point using kernel values with the support vectors

---

## Properties of Kernel Functions

A kernel function:

{{% colour "green" %}}
{{< katex display=true >}}
K(x,y)
{{< /katex >}}
{{% /colour %}}

takes two inputs and returns a real value.

It is symmetric:

{{% colour "green" %}}
{{< katex display=true >}}
K(x,y)=K(y,x)
{{< /katex >}}
{{% /colour %}}

It corresponds to an inner product in some feature space:

{{% colour "green" %}}
{{< katex display=true >}}
K(x_i,x_j)=\phi(x_i)^T\phi(x_j)
{{< /katex >}}
{{% /colour %}}

According to Mercer's theorem, every positive-semidefinite symmetric function is a valid kernel.

---

## Common Kernels

Kernels allow inner products in high-dimensional feature spaces without explicit mapping.

### Linear Kernel

{{% colour "green" %}}
{{< katex display=true >}}
K(x_i,x_j)=x_i^Tx_j
{{< /katex >}}
{{% /colour %}}

This gives ordinary linear SVM.

---

### Polynomial Kernel

{{% colour "green" %}}
{{< katex display=true >}}
K(x_i,x_j)=(1+x_i^Tx_j)^p
{{< /katex >}}
{{% /colour %}}

where {{< katex >}}p{{< /katex >}} is the polynomial degree.

This kernel can model polynomial decision boundaries.

---

### Sigmoid Kernel

{{% colour "green" %}}
{{< katex display=true >}}
K(x_i,x_j)=\tanh(\beta_0x_i^Tx_j+\beta_1)
{{< /katex >}}
{{% /colour %}}

This resembles the activation function used in neural networks.

---

### Exponential Distance Kernel

The slides mention kernels of the form:

{{% colour "green" %}}
{{< katex display=true >}}
k(x,x')=\exp(-d(x,x'))
{{< /katex >}}
{{% /colour %}}

where {{< katex >}}d(x,x'){{< /katex >}} is a distance function.

A common related form is the Gaussian or RBF kernel:

{{% colour "green" %}}
{{< katex display=true >}}
K(x_i,x_j)=
\exp
\left(
-\frac{\|x_i-x_j\|^2}{2\sigma^2}
\right)
{{< /katex >}}
{{% /colour %}}

---

## Nonlinear SVM Workflow

```text
Input data x
      ↓
Choose feature mapping or kernel
      ↓
Compute kernel matrix K(x_i, x_j)
      ↓
Solve SVM dual for α
      ↓
Identify support vectors
      ↓
Classify new point using kernel values
```

---

## XOR Example

The XOR pattern is a classic nonlinear classification problem.

A typical XOR dataset is:

| Point | {{< katex >}}x_1{{< /katex >}} | {{< katex >}}x_2{{< /katex >}} | Label |
|---|---:|---:|---:|
| {{< katex >}}x_1{{< /katex >}} | -1 | -1 | -1 |
| {{< katex >}}x_2{{< /katex >}} | -1 | +1 | +1 |
| {{< katex >}}x_3{{< /katex >}} | +1 | -1 | +1 |
| {{< katex >}}x_4{{< /katex >}} | +1 | +1 | -1 |

This cannot be separated by a single straight line in the original 2D space.

However, using a nonlinear transformation or kernel, it can become separable.

---

## Feature Map for XOR Intuition

A useful feature for XOR is the product:

{{% colour "green" %}}
{{< katex display=true >}}
z=x_1x_2
{{< /katex >}}
{{% /colour %}}

For the XOR-style labels above:

| {{< katex >}}x_1{{< /katex >}} | {{< katex >}}x_2{{< /katex >}} | {{< katex >}}x_1x_2{{< /katex >}} | Label |
|---:|---:|---:|---:|
| -1 | -1 | +1 | -1 |
| -1 | +1 | -1 | +1 |
| +1 | -1 | -1 | +1 |
| +1 | +1 | +1 | -1 |

Now the data can be separated based on the transformed feature {{< katex >}}x_1x_2{{< /katex >}}.

This shows why nonlinear feature spaces help.

---

## Linear vs Nonlinear SVM

| Aspect | Linear SVM | Nonlinear SVM |
|---|---|---|
| Decision boundary | Straight hyperplane | Curved in original space |
| Uses kernel? | Usually no, or linear kernel | Yes |
| Good for | Linearly separable data | Nonlinear patterns |
| Main formula | {{< katex >}}x_i^Tx_j{{< /katex >}} | {{< katex >}}K(x_i,x_j){{< /katex >}} |
| Example | simple two-class separation | XOR, circular patterns |

---

## Soft Margin with Nonlinear SVM

Nonlinear SVM can also use soft margins.

The soft-margin kernelised dual is:

{{% colour "green" %}}
{{< katex display=true >}}
\max_{\alpha}
\sum_i\alpha_i
-
\frac{1}{2}
\sum_i\sum_j
\alpha_i\alpha_jy_iy_jK(x_i,x_j)
{{< /katex >}}
{{% /colour %}}

subject to:

{{% colour "green" %}}
{{< katex display=true >}}
0\le\alpha_i\le C
{{< /katex >}}
{{% /colour %}}

and:

{{% colour "green" %}}
{{< katex display=true >}}
\sum_i\alpha_i y_i=0
{{< /katex >}}
{{% /colour %}}

The upper bound {{< katex >}}C{{< /katex >}} comes from soft-margin slack variables.

---

## How to Classify a New Example

Suppose {{< katex >}}x_*{{< /katex >}} is a new point.

### Step 1: Compute Kernel Values

For each support vector {{< katex >}}x_i{{< /katex >}}:

{{% colour "green" %}}
{{< katex display=true >}}
K(x_i,x_*)
{{< /katex >}}
{{% /colour %}}

### Step 2: Compute Decision Score

{{% colour "green" %}}
{{< katex display=true >}}
s=
\sum_{i\in SV}\alpha_i y_iK(x_i,x_*)+b
{{< /katex >}}
{{% /colour %}}

### Step 3: Predict Class

{{% colour "green" %}}
{{< katex display=true >}}
\hat{y}=\operatorname{sign}(s)
{{< /katex >}}
{{% /colour %}}

If {{< katex >}}s>0{{< /katex >}}, predict {{< katex >}}+1{{< /katex >}}.
If {{< katex >}}s<0{{< /katex >}}, predict {{< katex >}}-1{{< /katex >}}.

---

## Exam Template: Kernel SVM Classifier

If the question gives {{< katex >}}\alpha_i{{< /katex >}}, {{< katex >}}y_i{{< /katex >}}, support vectors, kernel values and {{< katex >}}b{{< /katex >}}, use:

{{% colour "green" %}}
{{< katex display=true >}}
f(x)=
\sum_{i\in SV}\alpha_i y_iK(x_i,x)+b
{{< /katex >}}
{{% /colour %}}

Then:

{{% colour "green" %}}
{{< katex display=true >}}
\hat{y}=\operatorname{sign}(f(x))
{{< /katex >}}
{{% /colour %}}

Do not calculate {{< katex >}}w{{< /katex >}} explicitly unless the question asks for it.

---

## Exam Template: Kernel Matrix

If asked to compute a kernel matrix:

### Step 1: List Points

{{% colour "green" %}}
{{< katex display=true >}}
x_1,x_2,\ldots,x_n
{{< /katex >}}
{{% /colour %}}

### Step 2: Choose Kernel

For example, polynomial:

{{% colour "green" %}}
{{< katex display=true >}}
K(x_i,x_j)=(1+x_i^Tx_j)^p
{{< /katex >}}
{{% /colour %}}

### Step 3: Fill Matrix

{{% colour "green" %}}
{{< katex display=true >}}
K=
\begin{bmatrix}
K(x_1,x_1) & K(x_1,x_2) & \cdots \\
K(x_2,x_1) & K(x_2,x_2) & \cdots \\
\vdots & \vdots & \ddots
\end{bmatrix}
{{< /katex >}}
{{% /colour %}}

The matrix is symmetric.

---

## Common Exam Mistakes

| Mistake | Correction |
|---|---|
| Explicitly computing {{< katex >}}\phi(x){{< /katex >}} when not needed | Use the kernel directly |
| Forgetting support vectors only | Sum over support vectors with {{< katex >}}\alpha_i>0{{< /katex >}} |
| Replacing {{< katex >}}x_i^Tx_j{{< /katex >}} incorrectly | Replace every dot product with {{< katex >}}K(x_i,x_j){{< /katex >}} |
| Confusing linear and polynomial kernels | Linear: {{< katex >}}x_i^Tx_j{{< /katex >}}, polynomial: {{< katex >}}(1+x_i^Tx_j)^p{{< /katex >}} |
| Forgetting {{< katex >}}b{{< /katex >}} in prediction | Always add {{< katex >}}b{{< /katex >}} before taking sign |

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

## Related Pages

- [My Notes: Mathematical Preliminaries for SVM]({{% relref "14-mathematical-preliminaries-for-svm.md" %}})
- [My Notes: Primal/Dual Perspective for Linear SVM]({{% relref "15-primal-dual-perspective-for-linear-svm.md" %}})
- [My Notes: Dimensionality Reduction and PCA]({{% relref "12-dimensionality-reduction-and-pca.md" %}})

---

{{< home-link "Home" >}} | {{< section-index >}}

---
