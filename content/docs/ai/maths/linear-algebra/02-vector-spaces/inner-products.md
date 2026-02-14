---
title: "Inner Products and Dot Product"
draft: false
tags: ["Machine Learning", "Mathematics", "Linear Algebra"]
categories: ["AI", "ML"]
weight: 1225
---

# Inner Products and Dot Product

An **inner product** maps two vectors to a **single scalar**.

It allows us to measure:

- similarity  
- vector length  
- projections  
- orthogonality  

---

## Definition

For vectors  
{{< katex >}}\mathbf{a}, \mathbf{b} \in \mathbb{R}^n{{< /katex >}}

The inner product is written as:

{{< katex >}}\langle \mathbf{a}, \mathbf{b} \rangle{{< /katex >}}

In {{< katex >}}\mathbb{R}^n{{< /katex >}}, this is the **dot product**.

---

## Dot Product Formula

Let  

{{< katex >}}\mathbf{a} = (a_1, \dots, a_n){{< /katex >}}  
{{< katex >}}\mathbf{b} = (b_1, \dots, b_n){{< /katex >}}

{{% hint danger %}}
{{< katex display=true >}}
\mathbf{a}\cdot\mathbf{b}
=
\sum_{i=1}^{n} a_i b_i
{{< /katex >}}
{{% /hint %}}

---

## Key Properties

Let  
{{< katex >}}\mathbf{a},\mathbf{b},\mathbf{c}\in\mathbb{R}^n{{< /katex >}}  
and {{< katex >}}\lambda\in\mathbb{R}{{< /katex >}}.

### 1) Symmetry

{{% hint danger %}}
{{< katex display=true >}}
\mathbf{a}\cdot\mathbf{b}
=
\mathbf{b}\cdot\mathbf{a}
{{< /katex >}}
{{% /hint %}}

### 2) Linearity

{{% hint danger %}}
{{< katex display=true >}}
(\mathbf{a}+\mathbf{b})\cdot\mathbf{c}
=
\mathbf{a}\cdot\mathbf{c}
+
\mathbf{b}\cdot\mathbf{c}
{{< /katex >}}
{{% /hint %}}

{{% hint danger %}}
{{< katex display=true >}}
(\lambda\mathbf{a})\cdot\mathbf{b}
=
\lambda(\mathbf{a}\cdot\mathbf{b})
{{< /katex >}}
{{% /hint %}}

### 3) Positivity

{{% hint danger %}}
{{< katex display=true >}}
\mathbf{a}\cdot\mathbf{a} \ge 0
{{< /katex >}}
{{% /hint %}}

Equality holds only if  
{{< katex >}}\mathbf{a}=\mathbf{0}{{< /katex >}}.

---

## Norm (Length of a Vector)

{{% hint danger %}}
{{< katex display=true >}}
\|\mathbf{a}\|
=
\sqrt{\mathbf{a}\cdot\mathbf{a}}
{{< /katex >}}
{{% /hint %}}

---

## Important Theoretical Result

{{% hint info %}}
The **Cauchy–Schwarz Inequality** places a bound on the dot product and guarantees that angle formulas are valid.

See: [Cauchy–Schwarz Inequality](/maths/linear-algebra/02-vector-spaces/analytic-geometry/cauchy-schwarz/)
{{% /hint %}}

---

## Machine Learning Connection

The dot product appears in:

- Linear regression  
  {{% hint danger %}}
  {{< katex display=true >}}
  \hat{y} = \mathbf{w}\cdot\mathbf{x} + b
  {{< /katex >}}
  {{% /hint %}}

- Neural networks  
- SVM linear kernel  
- Cosine similarity  
- Gradient-based optimisation  

---

## Summary

- Inner product maps two vectors to a scalar  
- In {{< katex >}}\mathbb{R}^n{{< /katex >}}, it is the dot product  
- Defines vector length  
- Foundation for geometry and similarity  

---

{{< home-link "Home" >}} | {{< section-index >}}