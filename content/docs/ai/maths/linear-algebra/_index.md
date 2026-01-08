---
title: "Linear Algebra"
draft: false
tags: ["Machine Learning", "Mathematics", "Linear Algebra"]
categories: ["AI", "ML"]
weight: 1110
bookCollapseSection: true
---

# Linear Algebra

Linear Algebra forms the foundation for representing data and transformations in machine learning.

Key observations:
- Every machine learning model uses matrices
- Neural networks are pipelines of matrix operations

Linear Algebra provides the mathematical language used to represent data, transformations, and structure in machine learning.

---

## What to Learn

- Scalars, vectors, and matrices  
- Vector operations (addition, dot product)  
- Matrix multiplication *(critical)*  
- Identity matrices and transpose  
- Eigenvalues and eigenvectors *(conceptual understanding)*  
---
- **Scalar** → a number
- **Vector** → a directed point  
- **Matrix** → a space transformer  
- **Linear transformation** → structured mapping  
- **Feature** → one axis  
- **Feature space** → where data lives  
- **Vector space** → where vectors live  
---
![Matrix](/images/ai/matrix_vector_operations.png)
---
## Scalar

### Definition
A **scalar** is a single numerical value representing **magnitude only**.

{{< katex display=true >}}
\alpha \in \mathbb{R}
{{< /katex >}}

Scalars do not have direction — only size.

### Geometric Intuition
A scalar is like a number on a number line.  
It tells you **how much**, not **which way**.

### In Machine Learning
- Learning rates  
- Bias terms  
- Loss values  

---

## Vector

### Definition
A **vector** is an ordered collection of numbers representing **magnitude and direction**.

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
A **matrix** is a rectangular array of numbers arranged in rows and columns.

{{< katex display=true >}}
A =
\begin{bmatrix}
a_{11} & a_{12} \\
a_{21} & a_{22}
\end{bmatrix}
{{< /katex >}}

A vector is a special case of a matrix with one row or one column.

### Geometric Intuition
Matrices represent **linear transformations** of space.

When a matrix multiplies a vector, it can:
- Rotate it  
- Scale it  
- Stretch it  
- Project it into another space

---

## Linear Transformations

A function \( T : \mathbb{R}^n \rightarrow \mathbb{R}^m \) is a **linear transformation** if:

{{< katex display=true >}}
T(\mathbf{u} + \mathbf{v}) = T(\mathbf{u}) + T(\mathbf{v})
{{< /katex >}}

{{< katex display=true >}}
T(c\mathbf{u}) = cT(\mathbf{u})
{{< /katex >}}

Every matrix defines a linear transformation:

{{< katex display=true >}}
T(\mathbf{x}) = A\mathbf{x}
{{< /katex >}}

---

## Machine Learning View

Many machine learning models can be abstracted as:

{{< katex display=true >}}
y = Wx + b
{{< /katex >}}

> **Models transform input vectors using matrices.**

---

{{< home-link "Home" >}} | {{< section-index >}}
