---
title: "Cauchy–Schwarz"
draft: false
tags: ["Machine Learning", "Mathematics", "Linear Algebra"]
categories: ["AI", "ML"]
weight: 1228
---

# Cauchy–Schwarz Inequality

The **Cauchy–Schwarz Inequality** is one of the most important results in linear algebra.

It places a fundamental bound on the dot product of two vectors.

{{% hint warning %}}
If you see **angle**, **cosine**, **similarity**, or **inner product bounds**  
→ think **Cauchy–Schwarz Inequality**
{{% /hint %}}

---

## Statement of the Inequality

For any vectors  
{{< katex >}}\mathbf{a}, \mathbf{b} \in \mathbb{R}^n{{< /katex >}}:

{{% hint danger %}}
{{< katex display=true >}}
|\mathbf{a}\cdot\mathbf{b}|
\;\le\;
\|\mathbf{a}\|\,\|\mathbf{b}\|
{{< /katex >}}
{{% /hint %}}

This is one of the most important inequalities in linear algebra.

It guarantees that:

{{% hint info %}}
The cosine formula for angles is always valid.
{{% /hint %}}

---

## Equality Condition

Equality holds **if and only if** the vectors are **linearly dependent**, i.e.:

{{% hint info %}}
One vector is a scalar multiple of the other:
{{< katex >}}\mathbf{a} = \lambda \mathbf{b}{{< /katex >}}
{{% /hint %}}

This means the vectors point in the **same or opposite direction**.

---

## Why This Inequality Matters

Cauchy–Schwarz guarantees that:

{{% hint info %}}
{{< katex >}}
-1 \le
\frac{\mathbf{a}\cdot\mathbf{b}}
{\|\mathbf{a}\|\,\|\mathbf{b}\|}
\le 1
{{< /katex >}}
{{% /hint %}}

Because of this:
- The **angle between vectors** is always well-defined
- The cosine formula never breaks
- Inner products behave consistently

---

## Geometric Interpretation

- If the dot product is **large**, vectors align
- If it is **zero**, vectors are orthogonal
- If it reaches the bound, vectors are collinear

{{% hint info %}}
Cauchy–Schwarz tells us:
> “The dot product can never exceed the product of lengths.”
{{% /hint %}}

---

## Machine Learning Connection

Cauchy–Schwarz appears implicitly in:

- **Cosine similarity**
- **SVM kernels**
- **Projection formulas**
- **Gradient bounds**
- **Proofs of convergence**

{{% hint info %}}
Without Cauchy–Schwarz:
- cosine similarity would be invalid
- angle-based similarity would not work
{{% /hint %}}

---

## Reference

- [math.berkeley](https://math.berkeley.edu/~arash/54/notes/6_7.pdf)

---

{{< home-link "Home" >}} | {{< section-index >}}
