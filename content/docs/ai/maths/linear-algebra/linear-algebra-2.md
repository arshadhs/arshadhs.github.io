---
title: "Linear Algebra -II"
draft: false
tags: ["Maths", "Linear Algebra", "Octave", "Machine Learning"]
categories: ["AI", "ML", "Maths"]
weight: 1140
---

## Positive Definite Matrices

A square matrix is positive definite if pre-multiplying and post-multiplying it by the same vector always gives a positive number as a result, independently of how we choose the vector.

Positive definite symmetric matrices have the property that all their eigenvalues are positive.

For a real symmetric matrix A:

{{< katex display=true >}}
x^T A x > 0 \quad \forall x \neq 0
{{< /katex >}}

Positive semi-definite:

{{< katex display=true >}}
x^T A x \ge 0
{{< /katex >}}

Used heavily in:
- covariance matrices
- optimisation
- stability analysis

---

## Elementary Row Operations

Allowed operations:
1. Swap two rows
2. Multiply a row by a non-zero scalar
3. Add a multiple of one row to another

---

## Row Echelon Form (REF)

A matrix is in REF if:
- All zero rows are at the bottom
- Pivots (Leading entry) move to the right as rows go down
- All entries below a pivot are zero

---

## Reduced Row Echelon Form (RREF)

RREF additionally requires:
- Pivots are equal to 1
- Each pivot (leading 1) is the only non-zero entry in its column

{{% hint success %}}
The RREF of a matrix is **unique**.
{{% /hint %}}

---

## Rank of a Matrix

Rank is:
- the number of pivots
- the number of non-zero rows in REF or RREF

Rank is invariant under row operations.

---

## Solving Linear Systems

A system can have:
- **no solution**
- **a unique solution**
- **infinitely many solutions**

### Rank test

{{< katex display=true >}}
\text{rank}(A) < \text{rank}([A|b]) \Rightarrow \text{no solution}
{{< /katex >}}

{{< katex display=true >}}
\text{rank}(A) = \text{rank}([A|b]) \Rightarrow \text{solution exists}
{{< /katex >}}

Free variables → infinitely many solutions

---

## Useful Octave Commands

```matlab
zeros(m,n)     % zero matrix
eye(n)         % identity matrix
size(A)        % matrix size
rank(A)        % rank
rref(A)        % reduced row echelon form
det(A)         % determinant
inv(A)         % inverse
issymmetric(A) % symmetry check
```

---

## Summary

- Linear algebra is the backbone of machine learning
- Matrix multiplication is order-sensitive
- Rank determines solvability of systems
- Special matrices simplify computation, and appear everywhere in ML
- Octave helps build strong intuition

---

{{< home-link "Home" >}} | {{< section-index >}}
