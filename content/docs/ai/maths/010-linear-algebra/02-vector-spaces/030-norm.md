---
title: "Norm"
draft: false
weight: 30
tags: ["Linear Algebra", "Vector Spaces"]
---

# Norm

A **norm** measures the length (magnitude) of a vector.

- the norm of a vector x measures the distance from the origin to the point x.

Common example: Euclidean norm.

{{< katex display=true >}}
\lVert \mathbf{x} \rVert_2 = \sqrt{x_1^2 + \cdots + x_n^2}
{{< /katex >}}

{{% hint info %}}
Key Idea:
Norm = measure of size or length of a vector.
It generalises the idea of distance in geometry to higher dimensions.
{{% /hint %}}

---
## Common norms
- L1
- L2
- Infinity norm

## Why it matters
- norms quantify size
- are used in distances and regularisation.

---

# Intuition (From Lectures)

From lecture discussions on analytic geometry:

- Vectors represent points or directions in space  
- Norm tells “how far” a vector is from origin  
- It is essentially a distance measure  

So:

- Small norm → close to origin  
- Large norm → far from origin  

---

# Formal Definition

A norm is a function:

{{< katex display=true >}}
\lVert \cdot \rVert : \mathbb{R}^n \rightarrow \mathbb{R}
{{< /katex >}}

which satisfies:

---

## Properties of Norm

### 1. Non-negativity

{{< katex display=true >}}
\lVert x \rVert \geq 0
{{< /katex >}}

and

{{< katex display=true >}}
\lVert x \rVert = 0 \iff x = 0
{{< /katex >}}

---

### 2. Homogeneity

{{< katex display=true >}}
\lVert \alpha x \rVert = |\alpha| \lVert x \rVert
{{< /katex >}}

---

### 3. Triangle Inequality

{{< katex display=true >}}
\lVert x + y \rVert \leq \lVert x \rVert + \lVert y \rVert
{{< /katex >}}

---

These three properties define any valid norm.

---

# Common Norms

## L1 Norm (Manhattan Norm)

{{< katex display=true >}}
\lVert x \rVert_1 = \sum_{i=1}^{n} |x_i|
{{< /katex >}}

- Measures path along axes  
- Used in sparse models  

---

## L2 Norm (Euclidean Norm)

{{< katex display=true >}}
\lVert x \rVert_2 = \sqrt{\sum_{i=1}^{n} x_i^2}
{{< /katex >}}

- Most common norm  
- Corresponds to standard distance  

---

## Infinity Norm

{{< katex display=true >}}
\lVert x \rVert_\infty = \max_i |x_i|
{{< /katex >}}

- Takes maximum component  
- Useful in worst-case analysis  

---

# Distance Using Norm

Distance between two vectors:

{{< katex display=true >}}
d(x, y) = \lVert x - y \rVert
{{< /katex >}}

From lecture insight:

- Norm + subtraction → distance  
- Used in clustering and similarity  

---

# Connection to Inner Product

From analytic geometry lectures:

Norm is induced by inner product:

{{< katex display=true >}}
\lVert x \rVert = \sqrt{x^T x}
{{< /katex >}}

This links:

- Norm  
- Inner product  
- Geometry  

---

# Length and Angle

Using norm and inner product:

{{< katex display=true >}}
\cos \theta = \frac{x^T y}{\lVert x \rVert \lVert y \rVert}
{{< /katex >}}

So norm is essential for:

- measuring angles  
- checking orthogonality  

---

# Geometric Interpretation

- L2 norm → circular contours  
- L1 norm → diamond shape  
- L∞ norm → square shape  

This affects optimisation behaviour.

---

# Norm in Machine Learning

Norms are used extensively in ML:

### Regularisation

- L2 norm → Ridge regression  
- L1 norm → Lasso regression  

### Distance Metrics

- k-NN uses norms  
- clustering uses norms  

### Optimisation

- Gradient descent uses norm of gradients  

---

# Norm and Optimization

From later lectures:

- Norm helps measure error size  
- Used in loss functions  
- Helps determine convergence  

---

# Important Inequalities

### Cauchy-Schwarz Inequality

{{< katex display=true >}}
|x^T y| \leq \lVert x \rVert \lVert y \rVert
{{< /katex >}}

This is fundamental in ML proofs.

---

# Example

Given:

{{< katex display=true >}}
x = (3,4)
{{< /katex >}}

{{< katex display=true >}}
\lVert x \rVert_2 = \sqrt{3^2 + 4^2} = 5
{{< /katex >}}

---

# Common Exam Questions

1. Compute L1, L2, L∞ norms  
2. Prove triangle inequality  
3. Use norm to compute distance  
4. Relate norm with inner product  
5. Interpret geometrically  

---

# Hidden Exam Pattern

From lectures:

- Norm appears with:
  - inner product  
  - orthogonality  
  - distance  

👉 rarely asked alone  

---

# Common Mistakes

- Mixing L1 and L2 formulas  
- Forgetting absolute values in L1  
- Not applying square root in L2  
- Confusing norm with squared norm  

---

# Strategy to Prepare

1. Memorise formulas of all norms  
2. Practice geometric interpretation  
3. Solve distance-based problems  
4. Link with inner product  

---

# Quick Summary Table

| Norm | Formula | Meaning |
|------|--------|--------|
| L1 | sum of absolute values | Manhattan distance |
| L2 | square root of sum of squares | Euclidean distance |
| L∞ | max absolute value | Maximum deviation |

---

## References

[Vector Norms](https://www.geeksforgeeks.org/maths/vector-norms/)

- Lecture slides (Analytic Geometry, Inner Product)  
- Course handout  

---

{{< home-link "Home" >}} | {{< section-index >}}

---
