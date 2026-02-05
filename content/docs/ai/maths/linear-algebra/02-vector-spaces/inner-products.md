---
title: "Inner Products and Dot Product"
draft: false
tags: ["Machine Learning", "Mathematics", "Linear Algebra"]
categories: ["AI", "ML"]
weight: 1225
---

# Inner Products and Dot Product

In Machine Learning, we constantly measure:
- **similarity** between data points
- **angles** between vectors
- **lengths** (magnitudes)
- **orthogonality** (independence / no overlap)

All of these start with one key tool:

**the inner product**.

---

## What is an Inner Product?

An **inner product** is an operation that takes two vectors and returns a **single number**.

{{% hint info %}}
In {{< katex >}}\mathbb{R}^n{{< /katex >}}, the most common inner product is the **dot product**.
{{% /hint %}}

For vectors  
{{< katex >}}\mathbf{a}, \mathbf{b} \in \mathbb{R}^n{{< /katex >}},  
we write the inner product as:

- {{< katex >}}\langle \mathbf{a}, \mathbf{b} \rangle{{< /katex >}}

---

## Dot Product (Inner Product in {{< katex >}}\mathbb{R}^n{{< /katex >}})

Let:

- {{< katex >}}\mathbf{a} = (a_1, a_2, \dots, a_n){{< /katex >}}
- {{< katex >}}\mathbf{b} = (b_1, b_2, \dots, b_n){{< /katex >}}

The **dot product** is:

{{% hint danger %}}
{{< katex display=true >}}
\mathbf{a}\cdot\mathbf{b}
= \langle \mathbf{a}, \mathbf{b} \rangle
= \sum_{i=1}^{n} a_i b_i
{{< /katex >}}
{{% /hint %}}

---

## Geometric Meaning of the Dot Product

The dot product connects algebra to geometry:

{{% hint danger %}}
{{< katex display=true >}}
\mathbf{a}\cdot\mathbf{b}
= \|\mathbf{a}\|\,\|\mathbf{b}\|\cos(\theta)
{{< /katex >}}
{{% /hint %}}

Where:
- {{< katex >}}\|\mathbf{a}\|{{< /katex >}} and {{< katex >}}\|\mathbf{b}\|{{< /katex >}} are vector lengths (norms)
- {{< katex >}}\theta{{< /katex >}} is the angle between them

{{% hint info %}}
Interpretation (Important):
- If {{< katex >}}\cos(\theta){{< /katex >}} is near **1** → vectors align
- If {{< katex >}}\cos(\theta)=0{{< /katex >}} → vectors are **orthogonal**
- If {{< katex >}}\cos(\theta)<0{{< /katex >}} → vectors oppose each other
{{% /hint %}}

---

## Angle Between Two Vectors

Rearranging gives the angle:

{{% hint danger %}}
{{< katex display=true >}}
\theta
= \cos^{-1}
\left(
\frac{\mathbf{a}\cdot\mathbf{b}}
{\|\mathbf{a}\|\,\|\mathbf{b}\|}
\right)
{{< /katex >}}
{{% /hint %}}

{{% hint info %}}
The fraction always lies in {{< katex >}}[-1,1]{{< /katex >}}, so the angle is well-defined.
{{% /hint %}}

---

## Orthogonality

Two vectors are **orthogonal** if their dot product is zero:

{{% hint danger %}}
{{< katex display=true >}}
\mathbf{a}\cdot\mathbf{b} = 0
{{< /katex >}}
{{% /hint %}}

This implies:

{{% hint danger %}}
{{< katex display=true >}}
\theta = \frac{\pi}{2}
{{< /katex >}}
{{% /hint %}}

So the vectors are **perpendicular**.

---

## Worked Example

Let:

{{% hint danger %}}
{{< katex display=true >}}
\mathbf{a} =
\begin{bmatrix}
2\\
2
\end{bmatrix},
\quad
\mathbf{b} =
\begin{bmatrix}
2\\
-2
\end{bmatrix}
{{< /katex >}}
{{% /hint %}}

Compute dot product:

{{% hint danger %}}
{{< katex display=true >}}
\mathbf{a}\cdot\mathbf{b}
= (2)(2) + (2)(-2)
= 4 - 4
= 0
{{< /katex >}}
{{% /hint %}}

So the vectors are **orthogonal**.

---

## Key Properties of the Dot Product

Let  
{{< katex >}}\mathbf{a},\mathbf{b},\mathbf{c}\in\mathbb{R}^n{{< /katex >}}  
and {{< katex >}}\lambda\in\mathbb{R}{{< /katex >}}.

### 1. Symmetry

{{% hint danger %}}
{{< katex display=true >}}
\mathbf{a}\cdot\mathbf{b}
= \mathbf{b}\cdot\mathbf{a}
{{< /katex >}}
{{% /hint %}}

### 2. Linearity

{{% hint danger %}}
{{< katex display=true >}}
(\mathbf{a}+\mathbf{b})\cdot\mathbf{c}
= \mathbf{a}\cdot\mathbf{c}
+ \mathbf{b}\cdot\mathbf{c}
{{< /katex >}}
{{% /hint %}}

{{% hint danger %}}
{{< katex display=true >}}
(\lambda\mathbf{a})\cdot\mathbf{b}
= \lambda(\mathbf{a}\cdot\mathbf{b})
{{< /katex >}}
{{% /hint %}}

### 3. Positivity

{{% hint danger %}}
{{< katex display=true >}}
\mathbf{a}\cdot\mathbf{a} \ge 0
{{< /katex >}}
{{% /hint %}}

Equality holds **only** when  
{{< katex >}}\mathbf{a}=\mathbf{0}{{< /katex >}}.

### 4. Relation to Norm

{{% hint danger %}}
{{< katex display=true >}}
\|\mathbf{a}\|
= \sqrt{\mathbf{a}\cdot\mathbf{a}}
{{< /katex >}}
{{% /hint %}}

---

## Why This Matters in Machine Learning

The dot product appears everywhere:

- **Linear models**: predictions use dot products
  {{% hint danger %}}
  {{< katex display=true >}}
  \hat{y} = \mathbf{w}\cdot\mathbf{x} + b
  {{< /katex >}}
  {{% /hint %}}

- **Similarity**: cosine similarity is derived from the dot product
- **SVMs**: kernels (especially linear kernel) use dot products
- **Neural networks**: each neuron computes a weighted sum (dot product)
- **Optimisation**: gradients and updates rely on inner products

{{% hint info %}}
Practical intuition:
The dot product measures how much two vectors “point in the same direction”.
In ML, that often means “how similar” two feature vectors are.
{{% /hint %}}

---

## Summary

- An **inner product** maps two vectors to a scalar
- Inner product is usually the **dot product** {{< katex >}}\mathbb{R}^n{{< /katex >}}
- Dot product connects to:
  - angle between vectors
  - orthogonality
  - lengths (norms)
- Fundamental to ML models

---

## Reference

- [Dot Product](https://www.geeksforgeeks.org/maths/dot-product/)

---

{{< home-link "Home" >}} | {{< section-index >}}
