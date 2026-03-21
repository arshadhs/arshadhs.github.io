---
title: "Angles and Orthogonality"
draft: false
tags: ["Linear Algebra", "Vector Spaces"]
categories: ["AI", "ML"]
weight: 60
---

# Angles and Orthogonality

Once we define an inner product, we can define the **angle between two vectors**.

Angles allow us to measure how aligned or different two vectors are in space.

{{% hint info %}}
Key Idea:
Angle measures similarity between vectors.
Orthogonality means complete independence (no similarity).
{{% /hint %}}

## Why It Matters in Machine Learning

- PCA produces orthogonal components  
- Orthogonal features reduce redundancy  
- Gradient directions depend on angle  

---

# Angle Formula

For vectors in n-dimensional space:

{{< katex display=true >}}
\mathbf{a}, \mathbf{b} \in \mathbb{R}^n
{{< /katex >}}

The cosine of the angle is:

{{< colour "green" >}}
{{< katex display=true >}}
\cos \alpha
=
\frac{\langle \mathbf{a}, \mathbf{b} \rangle}
{\|\mathbf{a}\|\,\|\mathbf{b}\|}
{{< /katex >}}
{{< /colour >}}

The angle is:

{{< colour "green" >}}
{{< katex display=true >}}
\alpha
=
\cos^{-1}
\left(
\frac{\langle \mathbf{a}, \mathbf{b} \rangle}
{\|\mathbf{a}\|\,\|\mathbf{b}\|}
\right)
{{< /katex >}}
{{< /colour >}}

---

# Why This Works (Lecture Insight)

From analytic geometry lectures:

- Inner product captures alignment  
- Norm captures magnitude  
- Ratio gives directional similarity  

This is guaranteed to lie in [-1, 1] due to:

{{< katex display=true >}}
| \langle a, b \rangle | \leq \|a\| \|b\|
{{< /katex >}}

This is the **Cauchy–Schwarz inequality**.

---

# Interpretation

- Cosine ≈ 1 → vectors point in same direction  
- Cosine ≈ 0 → vectors are perpendicular  
- Cosine < 0 → vectors point in opposite directions  

---

# Orthogonality

Vectors are orthogonal if:

{{< colour "green" >}}
{{< katex display=true >}}
\langle \mathbf{a}, \mathbf{b} \rangle = 0
{{< /katex >}}
{{< /colour >}}

This implies:

{{< colour "green" >}}
{{< katex display=true >}}
\alpha = \frac{\pi}{2}
{{< /katex >}}
{{< /colour >}}

---

# Geometric Meaning

- Orthogonal vectors form right angles  
- They share no directional component  
- They represent independent directions  

---

# Connection to Norm and Inner Product

From lecture:

{{< katex display=true >}}
\|a\| = \sqrt{a^T a}
{{< /katex >}}

{{< katex display=true >}}
\langle a, b \rangle = a^T b
{{< /katex >}}

So angle depends on both:

- magnitude (norm)  
- alignment (inner product)  

---

# Example

{{< colour "green" >}}
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
{{< /colour >}}

Dot product:

{{< colour "green" >}}
{{< katex display=true >}}
(2)(2) + (2)(-2) = 0
{{< /katex >}}
{{< /colour >}}

Therefore:

- vectors are orthogonal  
- angle = 90 degrees  

---

# Orthonormal Vectors

A set of vectors is orthonormal if:

- each vector has unit norm  
- vectors are mutually orthogonal  

{{< katex display=true >}}
\langle v_i, v_j \rangle = 0 \quad (i \neq j)
{{< /katex >}}

{{< katex display=true >}}
\|v_i\| = 1
{{< /katex >}}

---

# Why Orthogonality Matters

From lectures and ML applications:

- Orthogonal vectors reduce redundancy  
- Used in basis construction  
- Used in Gram-Schmidt process  
- Important in matrix decompositions  

---

# Applications in Machine Learning

- PCA produces orthogonal components  
- Orthogonal features improve learning  
- Gradient directions depend on angle  
- Cosine similarity used in NLP  

---

# Hidden Exam Pattern

From lecture structure:

- Angle questions are combined with:
  - norm  
  - inner product  
  - orthogonality  

👉 rarely standalone  

---

# Common Mistakes

- Forgetting denominator in cosine formula  
- Not normalising vectors  
- Arithmetic errors in dot product  
- Mixing angle and cosine  

---

# Strategy to Prepare

1. Memorise angle formula  
2. Practice dot product calculations  
3. Understand geometric meaning  
4. Link with norm and inner product  

---

# Quick Summary Table

| Concept | Formula | Meaning |
|--------|--------|--------|
| Cosine | <a,b> / (||a|| ||b||) | Alignment |
| Angle | cos^{-1}(...) | Direction difference |
| Orthogonal | <a,b> = 0 | Independent directions |

---

# References

- Lecture slides (Analytic Geometry)  
- Course handout  
- Webinar discussions on inner product and orthogonality  

---

{{< home-link "Home" >}} | {{< section-index >}}

---
