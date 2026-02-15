---
title: "Special Matrices"
draft: false
weight: 1126
---

# Special Matrices

Certain types of matrices have special structural properties that are widely used in linear algebra and machine learning.

---

## Identity Matrix

An **identity matrix** is a square matrix where:
- all diagonal entries are 1
- all other entries are 0

Example (3×3):

{{< katex display=true >}}
I =
\begin{bmatrix}
1 & 0 & 0 \\
0 & 1 & 0 \\
0 & 0 & 1
\end{bmatrix}
{{< /katex >}}

### Key Property

{{< katex display=true >}}
AI = IA = A
{{< /katex >}}

### Determinant

{{< katex display=true >}}
\det(I) = 1
{{< /katex >}}

---

## Symmetric Matrix

A matrix is **symmetric** if:

{{< katex display=true >}}
A^T = A
{{< /katex >}}

**Used in**
- Covariance matrices  
- Quadratic forms  
- Optimisation  


The matrix is symmetric about its main diagonal.

---

## Skew-Symmetric Matrix

A matrix is **skew-symmetric** if:

{{< katex display=true >}}
A^T = -A
{{< /katex >}}

This implies:

{{< katex display=true >}}
a_{ii} = 0
{{< /katex >}}

This implies all diagonal elements are zero.

---

## Diagonal Matrix

A matrix with non-zero elements only on its main diagonal.

{{< katex display=true >}}
A =
\begin{bmatrix}
a_1 & 0 & 0 \\
0 & a_2 & 0 \\
0 & 0 & a_3
\end{bmatrix}
{{< /katex >}}

---

# Triangular Matrices

---

## Upper Triangular Matrix

All elements below the main diagonal are zero:

{{< katex display=true >}}
a_{ij} = 0 \quad \text{for } i > j
{{< /katex >}}

---

## Lower Triangular Matrix

All elements above the main diagonal are zero:

{{< katex display=true >}}
a_{ij} = 0 \quad \text{for } i < j
{{< /katex >}}

---

## Determinant of Triangular Matrices ⭐

For **both upper and lower triangular matrices**:

{{< katex display=true >}}
\det(A) = \prod_{i=1}^{n} a_{ii}
{{< /katex >}}

- The determinant equals the **product of diagonal entries**.

{{% hint info %}}
This makes computing determinants extremely fast for triangular matrices.
{{% /hint %}}

---

## Inverse of Triangular Matrices ⭐

A triangular matrix is invertible **if and only if**:

{{< katex display=true >}}
a_{ii} \neq 0 \quad \forall i
{{< /katex >}}

If invertible:

{{< katex display=true >}}
A^{-1} \text{ is also triangular of the same type}
{{< /katex >}}

- Upper triangular → inverse is upper triangular  
- Lower triangular → inverse is lower triangular  

---

## Strictly Lower Triangular Matrix

All elements on and above the diagonal are zero:

{{< katex display=true >}}
a_{ij} = 0 \quad \text{for } i \le j
{{< /katex >}}

---

## Unit Lower Triangular Matrix

Diagonal entries are 1:

{{< katex display=true >}}
a_{ii} = 1
{{< /katex >}}

Appears in **LU decomposition**.

---

## Solving Systems with Triangular Matrices

For lower triangular:

{{< katex display=true >}}
A\mathbf{x} = \mathbf{b}
{{< /katex >}}

Solve using **forward substitution**.

For upper triangular:

Use **back substitution**.

---

## Positive Definite Matrix ⭐

A symmetric matrix \(A\) is **positive definite** if:

{{< katex display=true >}}
\mathbf{x}^T A \mathbf{x} > 0 \quad \forall \mathbf{x} \neq 0
{{< /katex >}}

---

## Positive Semi-Definite

{{< katex display=true >}}
\mathbf{x}^T A \mathbf{x} \ge 0
{{< /katex >}}

---

## Key Properties

If \(A\) is positive definite:

- All eigenvalues are positive  
- Determinant is positive  
- Matrix is invertible  
- Cholesky decomposition exists  

---

## Why Positive Definite Matters in ML

- Covariance matrices are positive semi-definite  
- Hessian matrix in optimisation  
- Guarantees convexity of quadratic functions  
- Appears in Gaussian distributions  

Example quadratic form:

{{< katex display=true >}}
f(\mathbf{x}) = \mathbf{x}^T A \mathbf{x}
{{< /katex >}}

If \(A\) is positive definite → function has a **unique global minimum**.

---

## Sparse Matrix

A matrix with mostly zero entries.

**Used in**
- Recommender systems  
- Graph representations  
- NLP  
- Large-scale ML  

Reduces:
- Memory  
- Computation time  

---

{{< home-link "Home" >}} | {{< section-index >}}