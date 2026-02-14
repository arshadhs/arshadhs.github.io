---
title: "Angles and Orthogonality"
draft: false
tags: ["Linear Algebra", "Vector Spaces"]
categories: ["AI", "ML"]
weight: 1226
---

# Angles and Orthogonality

Once we define an inner product, we can define the **angle between two vectors**.

---

## Angle Formula

For  
{{< katex >}}\mathbf{a}, \mathbf{b} \in \mathbb{R}^n{{< /katex >}}

{{% hint danger %}}
{{< katex display=true >}}
\cos \alpha
=
\frac{\langle \mathbf{a}, \mathbf{b} \rangle}
{\|\mathbf{a}\|\,\|\mathbf{b}\|}
{{< /katex >}}
{{% /hint %}}

The angle is:

{{% hint danger %}}
{{< katex display=true >}}
\alpha
=
\cos^{-1}
\left(
\frac{\langle \mathbf{a}, \mathbf{b} \rangle}
{\|\mathbf{a}\|\,\|\mathbf{b}\|}
\right)
{{< /katex >}}
{{% /hint %}}

{{% hint info %}}
The reason this fraction always lies in [-1,1] is guaranteed by the  
**Cauchy–Schwarz Inequality**.
{{% /hint %}}

---

## Interpretation

- Cosine ≈ 1 → vectors align  
- Cosine ≈ 0 → vectors are perpendicular  
- Cosine < 0 → vectors oppose  

---

## Orthogonality

Vectors are orthogonal if:

{{% hint danger %}}
{{< katex display=true >}}
\langle \mathbf{a}, \mathbf{b} \rangle = 0
{{< /katex >}}
{{% /hint %}}

Then:

{{% hint danger %}}
{{< katex display=true >}}
\alpha = \frac{\pi}{2}
{{< /katex >}}
{{% /hint %}}

---

## Example

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

Dot product:

{{% hint danger %}}
{{< katex display=true >}}
(2)(2) + (2)(-2) = 0
{{< /katex >}}
{{% /hint %}}

So vectors are orthogonal.

---

## Why It Matters in Machine Learning

- PCA produces orthogonal components  
- Orthogonal features reduce redundancy  
- Gradient directions depend on angle  

---

{{< home-link "Home" >}} | {{< section-index >}}