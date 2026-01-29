---
title: "Inverse Matrix"
draft: false
tags: ["Machine Learning", "Mathematics", "Linear Algebra"]
categories: ["AI", "ML"]
weight: 1160
---

# Inverse Matrix

The **inverse of a matrix** is a matrix that, when multiplied with the original matrix, produces the **identity matrix**.

A square matrix \(A\) is **invertible** if there exists a matrix \(A^{-1}\) such that:

{{% hint danger %}}
{{< katex display=true >}}
AA^{-1} = A^{-1}A = I
{{< /katex >}}
{{% /hint %}}

Here:
- \(A\) is the original matrix  
- \(A^{-1}\) is the inverse  
- \(I\) is the identity matrix  

{{% hint info %}}
The inverse “undoes” the effect of the matrix transformation.
{{% /hint %}}

---

## When does an inverse exist?

An inverse exists **only if the determinant is non-zero**.

{{% hint danger %}}
{{< katex display=true >}}
\det(A) \neq 0
{{< /katex >}}
{{% /hint %}}

If \( \det(A) = 0 \), the matrix is **singular** and **not invertible**.

{{% hint info %}}
Geometrically, a non-invertible matrix collapses space into a lower dimension.
{{% /hint %}}

---

## Inverse using adjoint (classical formula)

If \( \det(A) \neq 0 \), the inverse can be computed using the **adjoint (adjugate)**:

{{% hint danger %}}
{{< katex display=true >}}
A^{-1} = \frac{1}{\det(A)}\operatorname{adj}(A)
{{< /katex >}}
{{% /hint %}}

where the adjoint is defined as:

{{% hint danger %}}
{{< katex display=true >}}
\operatorname{adj}(A) = C^T
{{< /katex >}}
{{% /hint %}}

and:
- \(C\) is the **cofactor matrix**
- \(C^T\) is its transpose

{{% hint warning %}}
This formula is **important for exams and theory**, but it is **not used in practice** for large matrices.  
Numerical methods such as **row reduction** or **matrix decompositions** are preferred.
{{% /hint %}}

---

## Determinant of the inverse

A key identity relating determinants and inverses:

{{% hint danger %}}
{{< katex display=true >}}
\det(A^{-1}) = \frac{1}{\det(A)}
{{< /katex >}}
{{% /hint %}}

This identity holds **only if** \( \det(A) \neq 0 \).

{{% hint info %}}
If the determinant is small, the inverse may exist but be numerically unstable.
{{% /hint %}}

---

## Why inverse matrices matter in Machine Learning

Inverse matrices appear in:
- Solving systems of linear equations  
- Least squares solutions  
- Closed-form solutions in linear regression  
- Covariance matrices  
- Gaussian distributions  

{{% hint info %}}
Many ML algorithms avoid explicit matrix inversion due to numerical instability.
{{% /hint %}}

---

> An inverse matrix reverses a linear transformation and exists **only when the determinant is non-zero**.

---

{{< home-link "Home" >}} | {{< section-index >}}
