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

## Conditions for a Vector Space (or Subspace)

For a set of vectors to qualify as a vector space (or subspace), it must satisfy the following conditions.

### 1. Contain the Zero Vector

{{< katex display=true >}}
\mathbf{0} =
\begin{bmatrix}
0 \\
0 \\
\vdots \\
0
\end{bmatrix}
{{< /katex >}}

The zero vector represents the **origin** and acts as the additive identity.

{{< katex display=true >}}
\mathbf{v} + \mathbf{0} = \mathbf{v}
{{< /katex >}}

---

### 2. Closed Under Addition

{{< katex display=true >}}
\mathbf{u}, \mathbf{v} \in V \Rightarrow \mathbf{u} + \mathbf{v} \in V
{{< /katex >}}

Adding two vectors must not take you outside the space.

---

### 3. Closed Under Scalar Multiplication

{{< katex display=true >}}
\alpha \in \mathbb{R},\ \mathbf{v} \in V \Rightarrow \alpha \mathbf{v} \in V
{{< /katex >}}

Scaling a vector must keep it within the same space.

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

## Summary

- **Scalar** → a number  
- **Vector** → a directed point  
- **Matrix** → a space transformer  
- **Feature** → one axis  
- **Feature space** → where data lives  
- **Vector space** → where vectors live  
- **Subspace** → meaningful structure inside that space  

---

You should be able to look at a model and say:

> *“This is transforming vectors using matrices.”*

---

{{< home-link "Home" >}} | {{< section-index >}}
