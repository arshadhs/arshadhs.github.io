---
title: "Eigenvalues and Eigenvectors"
draft: false
tags: ["Machine Learning", "Mathematics", "Linear Algebra"]
categories: ["AI", "ML"]
weight: 1320
---
# Eigenvalues and Eigenvectors

- Eigenvalues give scaling.
- Eigenvectors define invariant directions of transformation.

Eigenvalues and eigenvectors describe **directions that remain unchanged under a linear transformation**, except for scaling.

Let \( A \in \mathbb{R}^{n \times n} \).

A scalar \( \lambda \in \mathbb{R} \) is an **eigenvalue** of \( A \), and 
a non-zero vector \( \mathbf{x} \in \mathbb{R}^n \setminus \{0\} \) is an **eigenvector** corresponding to \( \lambda \), if:

{{% hint danger %}}
{{< katex display=true >}}
A\mathbf{x} = \lambda \mathbf{x}
{{< /katex >}}
{{% /hint %}}

This is called the **eigenvalue equation**.

They are fundamental in:
- PCA
- Optimisation
- Spectral methods
- Stability analysis
- Least squares
- Neural networks

---

# Equivalent Characterisations ⭐

The following statements are equivalent:

1. \( \lambda \) is an eigenvalue of \( A \)

2. There exists \( \mathbf{x} \neq 0 \) such that:

{{% hint danger %}}
{{< katex display=true >}}
(A - \lambda I)\mathbf{x} = 0
{{< /katex >}}
{{% /hint %}}

3. The system has a **non-trivial solution**

4. Rank condition:

{{% hint danger %}}
{{< katex display=true >}}
\operatorname{rank}(A - \lambda I) < n
{{< /katex >}}
{{% /hint %}}

5. Determinant condition:

{{% hint danger %}}
{{< katex display=true >}}
\det(A - \lambda I) = 0
{{< /katex >}}
{{% /hint %}}

---

# Characteristic Polynomial

The polynomial:

{{% hint danger %}}
{{< katex display=true >}}
p_A(\lambda) = \det(A - \lambda I)
{{< /katex >}}
{{% /hint %}}

is called the **characteristic polynomial**.

Eigenvalues are the **roots** of this polynomial.

---

# Important Property

If \( \mathbf{x} \) is an eigenvector corresponding to \( \lambda \), then:

{{% hint danger %}}
{{< katex display=true >}}
c\mathbf{x}, \quad c \in \mathbb{R} \setminus \{0\}
{{< /katex >}}
{{% /hint %}}

is also an eigenvector.

> Eigenvectors are defined **up to scaling**.

---

# Worked Example

Consider:

{{% hint danger %}}
{{< katex display=true >}}
A =
\begin{bmatrix}
1 & 1 \\
1 & 1
\end{bmatrix}
{{< /katex >}}
{{% /hint %}}

## Step 1: Characteristic Polynomial

{{% hint danger %}}
{{< katex display=true >}}
\det(A - \lambda I)
=
\begin{vmatrix}
1-\lambda & 1 \\
1 & 1-\lambda
\end{vmatrix}
=
(1-\lambda)^2 - 1
{{< /katex >}}
{{% /hint %}}

Solve:

{{% hint danger %}}
{{< katex display=true >}}
(1-\lambda)^2 - 1 = 0
{{< /katex >}}
{{% /hint %}}

Eigenvalues:

{{% hint danger %}}
{{< katex display=true >}}
\lambda = 2, \quad 0
{{< /katex >}}
{{% /hint %}}

---

## Step 2: Eigenvectors

For \( \lambda = 0 \):

Solve \( A\mathbf{x} = 0 \).

Nullspace gives:

{{% hint danger %}}
{{< katex display=true >}}
\mathbf{x} =
\begin{bmatrix}
1 \\
-1
\end{bmatrix}
{{< /katex >}}
{{% /hint %}}

For \( \lambda = 2 \):

{{% hint danger %}}
{{< katex display=true >}}
\mathbf{x} =
\begin{bmatrix}
1 \\
1
\end{bmatrix}
{{< /katex >}}
{{% /hint %}}

---

# General Procedure

1. Compute \( \det(A - \lambda I) \)
2. Solve for roots \( \lambda \)
3. For each \( \lambda \), compute nullspace of \( A - \lambda I \)

---

# Eigenspace

For eigenvalue \( \lambda \), the **eigenspace** is:

{{% hint danger %}}
{{< katex display=true >}}
E_\lambda = \operatorname{Null}(A - \lambda I)
{{< /katex >}}
{{% /hint %}}

It is a **subspace of \( \mathbb{R}^n \)**.

---

# Spectrum

The set of all eigenvalues of \( A \) is called the **spectrum** of \( A \).

---

# Additional Properties

### 1. Transpose Property

A matrix and its transpose have the same eigenvalues.

Reason:

{{% hint danger %}}
{{< katex display=true >}}
\det(A - \lambda I)
=
\det((A - \lambda I)^T)
=
\det(A^T - \lambda I)
{{< /katex >}}
{{% /hint %}}

---

### 2. Distinct Eigenvalues ⇒ Linear Independence ⭐

If an \( n \times n \) matrix has \( n \) **distinct eigenvalues**, then its eigenvectors are **linearly independent**.

---

### 3. Identity Matrix Example

For \( I_n \):

{{% hint danger %}}
{{< katex display=true >}}
I_n \mathbf{x} = 1 \cdot \mathbf{x}
{{< /katex >}}
{{% /hint %}}

- Only eigenvalue: \( \lambda = 1 \)
- Eigenspace: \( \mathbb{R}^n \)

---

# Symmetric Matrices ⭐

If \( A \) is symmetric:

- All eigenvalues are **real**
- Eigenvectors corresponding to different eigenvalues are **orthogonal**

---

# Spectral Theorem ⭐

If \( A \in \mathbb{R}^{n \times n} \) is symmetric:

- There exists an **orthonormal basis** of eigenvectors
- All eigenvalues are real

This allows diagonalisation:

{{% hint danger %}}
{{< katex display=true >}}
A = Q \Lambda Q^T
{{< /katex >}}
{{% /hint %}}

Where:
- \( Q \) = orthogonal matrix of eigenvectors
- \( \Lambda \) = diagonal matrix of eigenvalues

---

# Connection to Machine Learning

## 1. Least Squares

If \( A \in \mathbb{R}^{m \times n} \), then:

{{% hint danger %}}
{{< katex display=true >}}
A^T A
{{< /katex >}}
{{% /hint %}}

is:
- Symmetric
- Positive definite (if rank \( A = n \))

Because:

{{% hint danger %}}
{{< katex display=true >}}
\mathbf{x}^T A^T A \mathbf{x}
=
\|A\mathbf{x}\|^2 > 0
{{< /katex >}}
{{% /hint %}}

This matrix appears in:
- Linear regression
- Normal equations
- PCA

---

# Important Conceptual Question

Does every matrix have \( n \) eigenvectors?

- **No**

Example:

{{% hint danger %}}
{{< katex display=true >}}
\begin{bmatrix}
0 & 1 \\
0 & 0
\end{bmatrix}
{{< /katex >}}
{{% /hint %}}

This matrix does not have a full set of linearly independent eigenvectors.

---

# Geometric Interpretation

The equation:

{{% hint danger %}}
{{< katex display=true >}}
A\mathbf{x} = \lambda \mathbf{x}
{{< /katex >}}
{{% /hint %}}

means:

> Matrix \( A \) preserves the direction of \( \mathbf{x} \)  
> and only scales it by \( \lambda \).

---

# Summary

- Eigenvalues are roots of the characteristic polynomial
- Eigenvectors form eigenspaces
- Symmetric matrices have real eigenvalues
- Distinct eigenvalues ⇒ independent eigenvectors
- \( A^T A \) is symmetric positive definite (full rank case)
- Spectral theorem gives orthonormal eigenbasis

---

{{< home-link "Home" >}} | {{< section-index >}}
