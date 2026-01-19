---
title: "Angles and Orthogonality"
draft: false
tags: ["Machine Learning", "Mathematics", "Linear Algebra"]
categories: ["AI", "ML"]
weight: 26
tags: ["Linear Algebra", "Vector Spaces"]
---
# Angles and Orthogonality

The angle between vectors is defined using the inner product:

{{< katex display=true >}}
\cos\alpha =
\frac{\langle \mathbf{a}, \mathbf{b} \rangle}{\lVert \mathbf{a} \rVert \,\lVert \mathbf{b} \rVert}
{{< /katex >}}

Vectors are **orthogonal** if their inner product is zero:

{{< katex display=true >}}
\langle \mathbf{a}, \mathbf{b} \rangle = 0
{{< /katex >}}

To understand the **angle between two vectors**, we use the **inner product (dot product)**.

For any two vectors \( \mathbf{a}, \mathbf{b} \in \mathbb{R}^n \), the following always holds:

{{< katex display=true >}}
-1 \le \frac{\langle \mathbf{a}, \mathbf{b} \rangle}{\|\mathbf{a}\| \, \|\mathbf{b}\|} \le 1
{{< /katex >}}

This allows us to define the angle between the vectors.

---

## Angle Between Two Vectors

Let \( \alpha \) be the angle between vectors \( \mathbf{a} \) and \( \mathbf{b} \).

{{< katex display=true >}}
\alpha = \cos^{-1}\left(\frac{\langle \mathbf{a}, \mathbf{b} \rangle}{\|\mathbf{a}\| \, \|\mathbf{b}\|}\right)
{{< /katex >}}

### Intuition

- If the dot product is **large and positive**, the vectors point in **similar directions**
- If the dot product is **small (near 0)**, the vectors point in **very different directions**
- If the dot product is **negative**, the vectors point in **opposite directions**

---

## Orthogonality

Two vectors are **orthogonal** if their dot product is zero:

{{< katex display=true >}}
\langle \mathbf{a}, \mathbf{b} \rangle = 0
{{< /katex >}}

In this case, the angle between them is:

{{< katex display=true >}}
\alpha = \frac{\pi}{2}
{{< /katex >}}

This means the vectors are **perpendicular**.

---

## Example

Consider the vectors:

{{< katex display=true >}}
\mathbf{a} =
\begin{bmatrix}
2 \\
2
\end{bmatrix},
\quad
\mathbf{b} =
\begin{bmatrix}
2 \\
-2
\end{bmatrix}
{{< /katex >}}

Their dot product is:

{{< katex display=true >}}
\langle \mathbf{a}, \mathbf{b} \rangle
= (2)(2) + (2)(-2)
= 4 - 4
= 0
{{< /katex >}}

Since the dot product is zero, the vectors are **orthogonal**.

---

## Key Takeaways

- The angle between vectors is defined using the **dot product**
- Orthogonal vectors have **zero dot product**
- Orthogonality means vectors share **no directional overlap**

## Why it matters
- In machine learning, orthogonal features often represent **independent information**, which can make models easier to train and interpret

---

{{< home-link "Home" >}} | {{< section-index >}}
