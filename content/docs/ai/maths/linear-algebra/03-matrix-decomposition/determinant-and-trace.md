---
title: "Determinant and Trace"
draft: false
tags: ["Machine Learning", "Mathematics", "Linear Algebra"]
categories: ["AI", "ML"]
weight: 1310
---
# Determinant and Trace

---

# Minor

The **minor** of an element \(a_{ij}\) is the determinant of the smaller square matrix formed by:
- removing **row \(i\)**
- removing **column \(j\)**

The minor is denoted \(M_{ij}\).

{{% hint info %}}
Minors are used to compute **cofactors**, which are used for determinants and inverses (via adjoint/adjugate).
{{% /hint %}}

---

# Cofactor

The **cofactor** of \(a_{ij}\), denoted \(C_{ij}\), is:

{{< katex display=true >}}
C_{ij} = (-1)^{i+j} M_{ij}
{{< /katex >}}

Where:
- \(i\) is the row index
- \(j\) is the column index
- \(M_{ij}\) is the minor

## Why the sign term exists

The factor \( (-1)^{i+j} \) accounts for alternating signs depending on position in the matrix.

---

# Cofactor Matrix and Adjoint (Adjugate)

## Cofactor matrix

The **cofactor matrix** is the matrix formed by taking the cofactor of every entry.

---

## Determinant

The **determinant** is a **scalar value** that can be calculated for a **square matrix** (m x m).

The determinant of a square matrix,  \( det(A) \), is a function that maps matrices to real scalars. The determinant is equal to the product of all the eigenvalues of the matrix.

It is written as:
- \( det(A) \) or \( |A| \)


- It serves as a scaling factor that is used for the transformation of a matrix.
- Acts as a **scaling factor** for linear transformations
  
- Indicates whether a matrix is **invertible**

- It is a single numerical value that plays a key role in various matrix operations, such as calculating the inverse of a matrix or solving **systems of linear equations**.
- enable the **computation of eigenvalues**, which are fundamental to PCA and dimensionality reduction in machine learning.
- Appears in **calculus**, **optimisation**, and **probability** (e.g., Jacobians, covariance matrices)

---

## Determinants of different sizes

### 1×1 matrix

If \(X = [a]\), then:

{{< katex display=true >}}
\det(X) = a
{{< /katex >}}

---

### 2×2 matrix

For:

{{< katex display=true >}}
A =
\begin{bmatrix}
a & b \\
c & d
\end{bmatrix}
{{< /katex >}}

{{< katex display=true >}}
\det(A) = ad - bc
{{< /katex >}}

---

### 3×3 matrix (concept)

A 3×3 determinant is computed by expanding into **2×2 determinants**.

This can be done by expanding along:
- any row (\(R_1, R_2, R_3\))
- or any column (\(C_1, C_2, C_3\))

---

## Adjoint / Adjugate

The **adjoint** (more precisely, **adjugate**) of \(A\) is:

{{< katex display=true >}}
\operatorname{adj}(A) = C^T
{{< /katex >}}

Where:
- \(C\) is the cofactor matrix
- \(C^T\) is its transpose

- Used in the classical formula for the inverse:
  - \(A^{-1} = \frac{1}{\det(A)}\operatorname{adj}(A)\) (when \(\det(A)\neq 0\))

---

# Properties of the determinant

- **Transpose property**
  {{< katex display=true >}}
  \det(A)=\det(A^T)
  {{< /katex >}}

- **Zero property**
  If a matrix has:
  - a zero row/column, or
  - two identical rows/columns, or
  - two proportional rows/columns  
  then \( \det(A)=0 \)

- **Row/column swap**
  Swapping two rows/columns changes the sign:
  {{< katex display=true >}}
  \det(A) \rightarrow -\det(A)
  {{< /katex >}}

- **Scalar multiple**
  Multiplying one row/column by \(k\) multiplies the determinant by \(k\)

- **Row operation invariance**
  Adding a multiple of one row/column to another does not change the determinant:
  {{< katex display=true >}}
  R_i \rightarrow R_i + kR_j
  {{< /katex >}}

- **Product rule**
  {{< katex display=true >}}
  \det(AB)=\det(A)\det(B)
  {{< /katex >}}

- **Inverse property**
  {{< katex display=true >}}
  \det(A^{-1})=\frac{1}{\det(A)} \quad (\det(A)\neq 0)
  {{< /katex >}}

- **Triangular matrices**
  For upper/lower triangular matrices, determinant equals the product of diagonal entries

---

## Trace

The **trace** of an \( n \times n \) square matrix \( A \) is defined as the **sum of its diagonal elements**.

{{% hint danger %}}
{{< katex display=true >}}
\operatorname{tr}(A) = \sum_{i=1}^{n} a_{ii}
{{< /katex >}}
{{% /hint %}}

In simple terms:

> Trace = sum of diagonal entries.

---

## Example

For:

{{% hint danger %}}
{{< katex display=true >}}
A =
\begin{bmatrix}
a_{11} & a_{12} \\
a_{21} & a_{22}
\end{bmatrix}
{{< /katex >}}
{{% /hint %}}

{{% hint danger %}}
{{< katex display=true >}}
\operatorname{tr}(A) = a_{11} + a_{22}
{{< /katex >}}
{{% /hint %}}

---

# Properties of the Trace

Let \( A, B \in \mathbb{R}^{n \times n} \) and \( \alpha \in \mathbb{R} \).

---

### 1. Linearity (Addition)

{{% hint danger %}}
{{< katex display=true >}}
\operatorname{tr}(A + B) = \operatorname{tr}(A) + \operatorname{tr}(B)
{{< /katex >}}
{{% /hint %}}

---

### 2. Scalar Multiplication

{{% hint danger %}}
{{< katex display=true >}}
\operatorname{tr}(\alpha A) = \alpha \operatorname{tr}(A)
{{< /katex >}}
{{% /hint %}}

---

### 3. Trace of Identity

{{% hint danger %}}
{{< katex display=true >}}
\operatorname{tr}(I_n) = n
{{< /katex >}}
{{% /hint %}}

Because the identity matrix has \( n \) ones on the diagonal.

---

### 4. Cyclic Property (Very Important ⭐)

If  
\( A \in \mathbb{R}^{n \times k} \) and  
\( B \in \mathbb{R}^{k \times n} \), then:

{{% hint danger %}}
{{< katex display=true >}}
\operatorname{tr}(AB) = \operatorname{tr}(BA)
{{< /katex >}}
{{% /hint %}}

{{% hint info %}}
This does **not** mean \( AB = BA \).  
It only means their traces are equal.
{{% /hint %}}

This property is extremely important in:
- Optimisation
- Matrix calculus
- Machine learning derivations

---

## Important Identity

If \( A \) has eigenvalues \( \lambda_1, \lambda_2, \dots, \lambda_n \), then:

{{% hint danger %}}
{{< katex display=true >}}
\operatorname{tr}(A) = \sum_{i=1}^{n} \lambda_i
{{< /katex >}}
{{% /hint %}}

---

## Why Trace Matters in Machine Learning

Trace appears in:

- Matrix derivatives
- Quadratic forms
- PCA
- Gaussian likelihoods
- Optimisation proofs

Example quadratic form:

{{% hint danger %}}
{{< katex display=true >}}
\mathbf{x}^T A \mathbf{x} = \operatorname{tr}(\mathbf{x}^T A \mathbf{x})
{{< /katex >}}
{{% /hint %}}

Trace is often used to simplify matrix expressions.

---

{{% hint info %}}
The proofs of these properties are straightforward and follow directly from the definition of the trace and properties of summation.
{{% /hint %}}

---

{{< home-link "Home" >}} | {{< section-index >}}
