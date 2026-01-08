---
title: "Linear Algebra"
draft: false
tags: ["Machine Learning", "Mathematics", "Linear Algebra", "Probability", "Statistics", "Calculus"]
categories: ["AI", "ML"]
weight: 1110
menu: main
---

# Linear Algebra

- Every machine learning model uses matrices
- Neural networks are essentially pipelines of matrix operations

Linear Algebra provides the mathematical language used to represent data, transformations, and structure in machine learning.

---

## What to Learn

- Scalars, vectors, matrices  
- Vector operations (addition, dot product)  
- Matrix multiplication *(very important)*  
- Identity matrices and transpose  
- Eigenvalues and eigenvectors *(conceptual understanding)*  

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

Matrices are rectangular arrays of numbers that generalise vectors (a vector is a "skinny" matrix with one row or column)

### Geometric Intuition
A matrix represents a **linear transformation of space**.

When a matrix multiplies a vector, it can:
- Rotate it  
- Scale it  
- Stretch it  
- Project it into another space

### In Machine Learning
Matrices act as **transformation operators** applied to data and parameters.

---

## Matrix Properties

### Operational Properties

**Addition**  
- Commutative {{< katex display=true >}}(A+B=B+A){{< /katex >}} and associative, but only for matrices of the same dimensions.

**Multiplication**  
{{< katex display=true >}}
AB \ne BA \quad \text{(in general)}
{{< /katex >}}

The order of multiplication matters.

**Transpose**  
{{< katex display=true >}}
(A^T)^T = A, \quad (AB)^T = B^T A^T
{{< /katex >}}

**Inverse**  
For non-singular square matrices:
{{< katex display=true >}}
(AB)^{-1} = B^{-1} A^{-1}
{{< /katex >}}

**Determinant**  
 Identifies if a matrix is invertible; similar matrices share the same determinant.

---

### Fundamental Linear Algebra Results

**Linear Transformations**  
Every \( m \times n \) matrix represents a linear map between vector spaces.

**Rank–Nullity Theorem**  
{{< katex display=true >}}
\text{rank}(A) + \text{nullity}(A) = n
{{< /katex >}}

**Eigenvalues and Eigenvectors**  
{{< katex display=true >}}
A\mathbf{x} = \lambda \mathbf{x}
{{< /katex >}}

They describe the principal directions of a transformation.

---

__The following concepts connect linear algebra directly to how data is represented in machine learning.__

## Feature

### Definition
A **feature** is an individual measurable property or characteristic of a data point used as input to a machine learning model.

Each feature corresponds to **one dimension**.

{{< katex display=true >}}
x_i \in \mathbb{R}
{{< /katex >}}

A data point with \( d \) features is represented as:

{{< katex display=true >}}
\mathbf{x} =
\begin{bmatrix}
x_1 \\
x_2 \\
\vdots \\
x_d
\end{bmatrix}
{{< /katex >}}

### Geometric Intuition
A feature is like **one axis** in a coordinate system.

- One feature → line  
- Two features → plane  
- Three features → 3D space  

Each new feature adds a **new dimension**.

### Why Features Matter
Models learn patterns in features, not raw objects.

Good features:
- Capture relevant information  
- Make patterns easier to detect  

Poor features:
- Add noise  
- Increase dimensionality unnecessarily  

---

## Feature Space

### Definition
A **feature space** is the vector space formed by all features, where each data point is represented as a vector.

{{< katex display=true >}}
\mathbf{x} \in \mathbb{R}^d
{{< /katex >}}

Here, \( d \) is the number of features.

### Geometric Intuition
A feature space is a **coordinate system**:

- 1 feature → line  
- 2 features → plane  
- 3 features → 3D space  
- Many features → high-dimensional space  

Each data point becomes a point (or vector) in this space.

### Relation to Linear Algebra
Feature spaces are:
- Vector spaces  
- Often high-dimensional  
- Manipulated using matrices and vectors  

Operations such as projection, rotation, and dimensionality reduction occur directly in feature space.

---

## Vector Space

### Definition
A **vector space** is a set of vectors that follows **ten axioms**, defined under two operations:
- Vector addition  
- Scalar multiplication  

These axioms ensure consistent linear behaviour.

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
A **vector subspace** is a subset of a vector space that satisfies all vector space conditions.

A subspace must:
- Contain the zero vector
- Be closed under addition
- Be closed under scalar multiplication

### Geometric Intuition
A subspace is a **smaller space inside a larger space** that still behaves like a full space.

Examples:
- A line inside a plane  
- A plane inside 3D space  

### In Machine Learning
Subspaces capture **lower-dimensional structure** in data, such as the space spanned by principal components.

---
## Axioms of a Vector Space

### Properties of Vector Addition

**Closure**  
{{< katex display=true >}}
\mathbf{u}, \mathbf{v} \in V \Rightarrow \mathbf{u} + \mathbf{v} \in V
{{< /katex >}}

**Commutativity**  
{{< katex display=true >}}
\mathbf{u} + \mathbf{v} = \mathbf{v} + \mathbf{u}
{{< /katex >}}

**Associativity**  
{{< katex display=true >}}
(\mathbf{u} + \mathbf{v}) + \mathbf{w} = \mathbf{u} + (\mathbf{v} + \mathbf{w})
{{< /katex >}}

**Additive Identity**  
{{< katex display=true >}}
\mathbf{u} + \mathbf{0} = \mathbf{u}
{{< /katex >}}

**Additive Inverse**  
{{< katex display=true >}}
\mathbf{u} + (-\mathbf{u}) = \mathbf{0}
{{< /katex >}}

---

### Properties of Scalar Multiplication

**Distributivity over Vector Addition**  
{{< katex display=true >}}
c(\mathbf{u} + \mathbf{v}) = c\mathbf{u} + c\mathbf{v}
{{< /katex >}}

**Distributivity over Scalar Addition**  
{{< katex display=true >}}
(c + d)\mathbf{u} = c\mathbf{u} + d\mathbf{u}
{{< /katex >}}

**Associativity of Scalars**  
{{< katex display=true >}}
c(d\mathbf{u}) = (cd)\mathbf{u}
{{< /katex >}}

**Multiplicative Identity**  
{{< katex display=true >}}
1\mathbf{u} = \mathbf{u}
{{< /katex >}}

**Zero Property**  
{{< katex display=true >}}
0\mathbf{u} = \mathbf{0}, \quad c\mathbf{0} = \mathbf{0}
{{< /katex >}}

---

## Summary

- **Scalar** → a number  

- **Vector** → a directed point  
- **Vector space** → where vectors live  

- **Matrix** → a space transformer  

- **Feature** → one axis  
- **Feature space** → where data lives  

---

You should be able to look at a model and say:

> *“This is transforming vectors using matrices.”*

---

{{< home-link "Home" >}} | {{< section-index >}}
