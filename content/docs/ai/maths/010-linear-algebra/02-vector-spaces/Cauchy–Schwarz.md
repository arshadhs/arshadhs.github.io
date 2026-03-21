---
title: "Cauchy–Schwarz"
draft: false
tags: ["Machine Learning", "Mathematics", "Linear Algebra"]
categories: ["AI", "ML"]
weight: 1228
---

# Cauchy–Schwarz Inequality

The **Cauchy–Schwarz Inequality** is one of the most important results in linear algebra.

It places a fundamental bound on the inner product of two vectors.

{{% hint info %}}
If you see **angle**, **cosine**, **similarity**, or **inner product bounds**  
→ think **Cauchy–Schwarz Inequality**

Key Idea:
The inner product (dot product) can never exceed the product of magnitudes.
This ensures all geometric interpretations (angles, cosine) are valid.
{{% /hint %}}

---

## Statement of the Inequality

For any vectors:

{{< katex display=true >}}
\mathbf{a}, \mathbf{b} \in \mathbb{R}^n
{{< /katex >}}

{{< colour "yellow" >}}
{{< katex display=true >}}
|\mathbf{a}\cdot\mathbf{b}|
\le
\|\mathbf{a}\|\,\|\mathbf{b}\|
{{< /katex >}}
{{< /colour >}}

---

## Why This Inequality Matters

From lectures on inner product and angles:

Cauchy–Schwarz guarantees that:

{{< colour "yellow" >}}
{{< katex display=true >}}
-1 \le
\frac{\mathbf{a}\cdot\mathbf{b}}
{\|\mathbf{a}\|\,\|\mathbf{b}\|}
\le 1
{{< /katex >}}
{{< /colour >}}

This ensures:

- cosine values are always valid  
- angle between vectors is well-defined  
- geometric interpretation is consistent  

---

## Equality Condition

Equality holds **if and only if**:

{{< katex display=true >}}
\mathbf{a} = \lambda \mathbf{b}
{{< /katex >}}

This means:

- vectors are linearly dependent  
- vectors lie on the same line  
- direction is same or opposite  

---

## Connection to Angles

From analytic geometry:

{{< katex display=true >}}
\cos \alpha =
\frac{\langle a, b \rangle}
{\|a\| \|b\|}
{{< /katex >}}

Cauchy–Schwarz ensures this expression always lies in valid cosine range.

---

## Geometric Interpretation

If the dot product is:
- Large dot product → vectors align  
- Zero dot product → orthogonal vectors  
- Maximum value → vectors collinear  

{{% hint info %}}
Interpretation:
“The projection of one vector onto another cannot exceed its length.”
{{% /hint %}}

---

## Connection to Norm

From lecture:

{{< katex display=true >}}
\|x\| = \sqrt{x^T x}
{{< /katex >}}

Cauchy–Schwarz ensures consistency between:

- norm  
- inner product  
- distance  

---

## Important Consequence

Triangle inequality is derived using Cauchy–Schwarz:

{{< katex display=true >}}
\|x + y\| \le \|x\| + \|y\|
{{< /katex >}}

---

## Example

{{< katex display=true >}}
a = (1,2), \quad b = (3,4)
{{< /katex >}}

{{< katex display=true >}}
a \cdot b = 11
{{< /katex >}}

{{< katex display=true >}}
\|a\| = \sqrt{5}, \quad \|b\| = 5
{{< /katex >}}

{{< katex display=true >}}
|11| \le \sqrt{5} \cdot 5
{{< /katex >}}

Inequality holds.

---

## Machine Learning Connection

Cauchy–Schwarz appears in:

- cosine similarity  
- projection formulas  
- optimisation bounds  
- gradient analysis  
- kernel methods  

{{% hint info %}}
Without this inequality:
- cosine similarity breaks  
- angle-based ML models fail  
{{% /hint %}}

---

## Hidden Exam Pattern

From lectures:

- Used in:
  - angle proofs  
  - norm inequalities  
  - optimisation derivations  

👉 often appears indirectly  

---

## Common Mistakes

- Forgetting absolute value  
- Mixing dot product and norm  
- Ignoring equality condition  
- Not recognising hidden usage  

---

## Strategy to Prepare

1. Memorise inequality  
2. Understand geometric meaning  
3. Practice applying in proofs  
4. Link with norm and angle  

---

## Quick Summary

| Concept | Meaning |
|--------|--------|
| Bound | dot product ≤ product of norms |
| Equality | vectors are dependent |
| Use | angles, projections, ML |

---

## Reference

- [math.berkeley](https://math.berkeley.edu/~arash/54/notes/6_7.pdf)

- Lecture slides (Inner Product, Geometry)  
- Course handout  

---

{{< home-link "Home" >}} | {{< section-index >}}

---
