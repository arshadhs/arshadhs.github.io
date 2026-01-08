---
title: "Linear Algebra"
draft: false
tags: ["Machine Learning", "Mathematics", "Linear Algebra", "Probability", "Statistics", "Calculus"]
categories: ["AI", "ML"]
weight: 1110
menu: main
---
# Linear Algebra

- Every ML model uses matrices
- Neural networks are just matrix pipelines

## What to learn
- Scalars, vectors, matrices
- Vector operations (add, dot product)
- Matrix multiplication (very important)
- Identity & transpose
- Eigenvalues & eigenvectors (conceptual)

---
## Scalar

### Definition
A **scalar** is a single numerical value that represents **magnitude only**.

{{< katex display=true >}}
\alpha \in \mathbb{R}
{{< /katex >}}

Scalars do not have direction — only size.

### Geometric Intuition
A scalar is like a number on a number line.  
It tells you **how much**, not **which way**.

### In Machine Learning
Scalars commonly represent:
- Learning rates
- Bias terms
- Loss values

---
## Vector

### Definition
A **vector** is an ordered collection of numbers that represents **magnitude and direction**.

{{< katex display=true >}}
\mathbf{x} =
\begin{bmatrix}
x_1 \\
x_2 \\
\vdots \\
x_n
\end{bmatrix}
{{< /katex >}}

### Geometric Intuition
A vector can be visualised as an **arrow**:
- The tail starts at the origin
- The head points to a location in space

In two dimensions, a vector points to a position on a plane.

### In Machine Learning
Vectors represent:
- Data points
- Feature sets
- Model parameters

---

## Matrix

### Definition
A **matrix** is a rectangular array of numbers arranged in **rows and columns**.

{{< katex display=true >}}
A =
\begin{bmatrix}
a_{11} & a_{12} \\
a_{21} & a_{22}
\end{bmatrix}
{{< /katex >}}

### Geometric Intuition
A matrix represents a **transformation of space**.

When a matrix multiplies a vector, it can:
- Rotate it
- Scale it
- Stretch it
- Project it into another space

### In Machine Learning
Matrices act as **transformation operators** applied to data and parameters.

---

## Vector Space

### Definition
A **vector space** is a set of vectors that is closed under:
- Vector addition
- Scalar multiplication

It also:
- Contains a zero vector
- Contains additive inverses

### Geometric Intuition
A vector space is the **entire space where vectors live**.

Examples:
- A line through the origin
- A plane through the origin
- Higher-dimensional spaces

### In Machine Learning
Feature spaces and embedding spaces are vector spaces.

---

## Vector Subspace

### Definition
A **vector subspace** is a subset of a vector space that is itself a vector space under the same operations.

A subspace must:
- Contain the zero vector
- Be closed under addition
- Be closed under scalar multiplication

### Geometric Intuition
A subspace is a **smaller space inside a larger space** that still behaves like a full space.

Examples:
- A line inside a plane
- A plane inside three-dimensional space

### In Machine Learning
Subspaces capture **lower-dimensional structure** in data, such as the space spanned by principal components.

---

## Summary
- **Scalar** → a number  
- **Vector** → a directed point  
- **Matrix** → a space transformer  
- **Vector space** → where vectors live  
- **Subspace** → meaningful structure inside that space

---

You can look at a model and say:
“This is transforming vectors using matrices.”

---

{{< home-link "Home" >}} | {{< section-index >}}
