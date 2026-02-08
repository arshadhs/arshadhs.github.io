---
title: "Matrices"
draft: false
tags: ["Machine Learning", "Mathematics", "Linear Algebra"]
categories: ["AI", "ML"]
weight: 1140
---

# Matrices

Matrices are the **core data structure of linear algebra** and the **workhorse of machine learning**.  
Almost every ML model can be described as a sequence of matrix operations.

- [Special Matrices](/docs/ai/maths/linear-algebra/03-matrix-decomposition/special-matrices/)

---

## Matrix

A **matrix** is a rectangular array of numbers arranged in **rows and columns**.

{{% hint danger %}}
{{< katex display=true >}}
A \in \mathbb{R}^{m \times n}
{{< /katex >}}
{{% /hint %}}

An \( m \times n \) matrix has:
- \( m \) rows
- \( n \) columns

Example of a matrix:

{{% hint danger %}}
{{< katex display=true >}}
A =
\begin{bmatrix}
1 & 2 & 3 \\
4 & 5 & 6
\end{bmatrix}
{{< /katex >}}
{{% /hint %}}

{{% hint info %}}
Vectors are just special cases of matrices (with one row or one column).
{{% /hint %}}

---

## Equality of Matrices

Two matrices are **equal** if:
1. They have the **same dimensions**
2. Every **corresponding entry** is equal

If either condition fails, the matrices are **not equal**.

---

## Matrix Operations

### Matrix addition

Matrices can be added **only if they have the same dimensions**.

{{% hint danger %}}
{{< katex display=true >}}
A \in \mathbb{R}^{m \times n}, \quad B \in \mathbb{R}^{m \times n}
{{< /katex >}}
{{% /hint %}}

Addition is performed **element-wise**.

#### Properties

**Commutative**
{{% hint danger %}}
{{< katex display=true >}}
A + B = B + A
{{< /katex >}}
{{% /hint %}}

**Associative**
{{% hint danger %}}
{{< katex display=true >}}
(A + B) + C = A + (B + C)
{{< /katex >}}
{{% /hint %}}

---

### Scalar multiplication

Multiplying a matrix by a scalar multiplies **every entry** of the matrix.

#### Properties

{{% hint danger %}}
{{< katex display=true >}}
c(A + B) = cA + cB
{{< /katex >}}
{{% /hint %}}

{{% hint danger %}}
{{< katex display=true >}}
(c + k)A = cA + kA
{{< /katex >}}
{{% /hint %}}

{{% hint danger %}}
{{< katex display=true >}}
(ck)A = c(kA)
{{< /katex >}}
{{% /hint %}}

{{% hint danger %}}
{{< katex display=true >}}
1A = A
{{< /katex >}}
{{% /hint %}}

---

### Matrix multiplication (very important)

Matrix multiplication combines **rows of the first matrix** with **columns of the second**.

If:
- \( A \in \mathbb{R}^{m \times n} \)
- \( B \in \mathbb{R}^{n \times p} \)

then:

{{% hint danger %}}
{{< katex display=true >}}
AB \in \mathbb{R}^{m \times p}
{{< /katex >}}
{{% /hint %}}

{{% hint info %}}
Matrix multiplication represents **composition of linear transformations**.
{{% /hint %}}

{{% hint warning %}}
Matrix multiplication is **not commutative** in general.
{{% /hint %}}

{{% hint danger %}}
{{< katex display=true >}}
AB \neq BA
{{< /katex >}}
{{% /hint %}}

{{% hint info %}}
Even if \( AB = 0 \), it does **not** imply  
\( A = 0 \) or \( B = 0 \).
{{% /hint %}}

---

## Vector

A **vector** is a matrix with **exactly one row or one column**.

{{% hint danger %}}
{{< katex display=true >}}
\text{Row vector: } 1 \times n \qquad
\text{Column vector: } m \times 1
{{< /katex >}}
{{% /hint %}}

### In machine learning

Vectors are used to represent:
- Data points  
- Feature sets  
- Model parameters  

{{% hint info %}}
Machine learning models transform **input vectors** into **output vectors** using matrices.
{{% /hint %}}


---

## Scalar

A **scalar** is a single numerical value representing **magnitude only**.

{{% hint danger %}}
{{< katex display=true >}}
\alpha \in \mathbb{R}
{{< /katex >}}
{{% /hint %}}

Scalars do **not** have direction — only size.

### Geometric intuition
A scalar is like a number on a number line.  
It tells you **how much**, not **which way**.

### In machine learning
Scalars commonly represent:
- Learning rates  
- Bias terms  
- Loss values  

---

{{< home-link "Home" >}} | {{< section-index >}}