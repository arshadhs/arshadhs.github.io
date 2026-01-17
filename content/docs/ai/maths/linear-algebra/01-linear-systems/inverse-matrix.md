---
title: "Inverse Matrix"
draft: false
tags: ["Machine Learning", "Mathematics", "Linear Algebra"]
categories: ["AI", "ML"]
weight: 1160
---

# Inverse Matrix

The inverse of a matrix is a matrix that, when multiplied by itself, results in the identity matrix 𝜤.

A square matrix \(A\) is **invertible** if there exists a matrix \(A^{-1}\) such that:

{{< katex display=true >}}
AA^{-1} = A^{-1}A = I
{{< /katex >}}

## When does an inverse exist?

An inverse exists **only if**:

{{< katex display=true >}}
\det(A) \neq 0
{{< /katex >}}

---

## Inverse using adjoint (classical formula)

If \( \det(A)\neq 0 \):

{{< katex display=true >}}
A^{-1} = \frac{1}{\det(A)}\operatorname{adj}(A)
{{< /katex >}}

Where:

{{< katex display=true >}}
\operatorname{adj}(A) = C^T
{{< /katex >}}

and \(C\) is the cofactor matrix.

{{% hint warning %}}
This formula is important for exams and theory, but for large matrices it is not used numerically (row reduction and decompositions are preferred).
{{% /hint %}}

---

## Determinant of the inverse

A very important identity:

{{< katex display=true >}}
\det(A^{-1}) = \frac{1}{\det(A)}
{{< /katex >}}

This holds only when \( \det(A)\neq 0 \).

---

---
{{< home-link "Home" >}} | {{< section-index >}}
