---
title: "Matrix Properties and Decompositions"
draft: false
tags: ["Machine Learning", "Mathematics", "Linear Algebra"]
categories: ["AI", "ML"]
weight: 1130
---

# Matrix Properties and Decompositions

Advanced linear algebra concepts are used to analyse, simplify, and optimise machine learning models.

---

## Linear Systems

Linear systems describe multiple linear equations solved simultaneously.

They connect algebraic equations with matrix representations.

---

## Matrix Properties

### Addition
Commutative and associative for same-sized matrices.

### Multiplication
{{< katex display=true >}}
AB \ne BA
{{< /katex >}}

Order matters.

---

### Transpose
{{< katex display=true >}}
(AB)^T = B^T A^T
{{< /katex >}}

---

### Inverse
For invertible matrices:

{{< katex display=true >}}
(AB)^{-1} = B^{-1} A^{-1}
{{< /katex >}}

---

### Determinant
Indicates invertibility and volume scaling.

---

## Rank–Nullity Theorem

{{< katex display=true >}}
\text{rank}(A) + \text{nullity}(A) = n
{{< /katex >}}

This explains redundancy and dimensionality.

---

## Eigenvalues and Eigenvectors

{{< katex display=true >}}
A\mathbf{x} = \lambda \mathbf{x}
{{< /katex >}}

They represent invariant directions of transformation.

---

## Analytic Geometry

- Norms
- Inner products
- Angles
- Orthogonality
- Orthonormal bases

These define similarity and distance.

---

## Matrix Decompositions

Matrix decompositions break complex matrices into simpler parts.

### Common Decompositions
- Eigen-decomposition  
- Singular Value Decomposition (SVD)  
- Cholesky decomposition  

---

## Low-Rank Approximation

Used for:
- Dimensionality reduction
- Noise removal
- Efficient computation

Forms the basis of **PCA**.

---

## Machine Learning Perspective

Matrix decompositions help:
- Reduce dimensionality
- Improve numerical stability
- Extract structure from data

---

{{< home-link "Home" >}} | {{< section-index >}}
