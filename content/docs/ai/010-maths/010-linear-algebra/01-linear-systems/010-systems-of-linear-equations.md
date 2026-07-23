---
title: "Systems of Linear Equations"
draft: false
tags: ["Machine Learning", "Mathematics", "Linear Algebra"]
categories: ["AI", "ML"]
weight: 1130
---
# Systems of Linear Equations

A system of linear equations can be written compactly as:

{{% hint danger %}}
{{< katex display=true >}}
A\mathbf{x}=\mathbf{b}
{{< /katex >}}
{{% /hint %}}

This represents:
- a **linear transformation** applied to an unknown vector \(\mathbf{x}\)
- producing an output vector \(\mathbf{b}\)

---

## Key components

### Coefficient matrix \(A\)

\(A\) contains the coefficients of the variables.

{{% hint danger %}}
{{< katex display=true >}}
A \in \mathbb{R}^{m\times n}
{{< /katex >}}
{{% /hint %}}

### Variable vector \(\mathbf{x}\)

\(\mathbf{x}\) contains the unknowns.

{{% hint danger %}}
{{< katex display=true >}}
\mathbf{x}=
\begin{bmatrix}
x_1\\
x_2\\
\vdots\\
x_n
\end{bmatrix}
\in \mathbb{R}^{n}
{{< /katex >}}
{{% /hint %}}

### Constant vector \(\mathbf{b}\)

\(\mathbf{b}\) contains constants on the right-hand side.

{{% hint danger %}}
{{< katex display=true >}}
\mathbf{b}=
\begin{bmatrix}
b_1\\
b_2\\
\vdots\\
b_m
\end{bmatrix}
\in \mathbb{R}^{m}
{{< /katex >}}
{{% /hint %}}

---

## When does a solution exist?

The type of solution depends on \(A\).

### Unique solution

If \(A\) is **square** and **invertible**, then the solution is unique.

{{% hint danger %}}
{{< katex display=true >}}
\det(A)\neq 0
{{< /katex >}}
{{% /hint %}}

In that case:

{{% hint danger %}}
{{< katex display=true >}}
\mathbf{x}=A^{-1}\mathbf{b}
{{< /katex >}}
{{% /hint %}}

{{% hint info %}}
A unique solution requires \(A\) to be invertible (non-singular).
{{% /hint %}}

### No solution or infinitely many solutions

If \(A\) is **not invertible** (singular), then the system can have:
- **no solution** (inconsistent), or
- **infinitely many solutions** (multiple degrees of freedom)

{{% hint danger %}}
{{< katex display=true >}}
\det(A)=0
{{< /katex >}}
{{% /hint %}}

---

## Homogeneous systems

A **homogeneous** system is:

{{% hint danger %}}
{{< katex display=true >}}
A\mathbf{x}=\mathbf{0}
{{< /katex >}}
{{% /hint %}}

- It always has the **trivial solution** \(\mathbf{x}=\mathbf{0}\).
- It has a **non-trivial solution** (\(\mathbf{x}\neq \mathbf{0}\)) if and only if \(A\) is singular.

{{% hint danger %}}
{{< katex display=true >}}
A\mathbf{x}=\mathbf{0}\text{ has a non-trivial solution } \iff \det(A)=0
{{< /katex >}}
{{% /hint %}}

---

## Interpretation

### Linear combination view

The equation \(A\mathbf{x}=\mathbf{b}\) means \(\mathbf{b}\) is a linear combination of the **columns of \(A\)**.

If \(A=[\mathbf{a}_1\ \mathbf{a}_2\ \dots\ \mathbf{a}_n]\), then:

{{% hint danger %}}
{{< katex display=true >}}
A\mathbf{x}=\mathbf{b}
\iff
x_1\mathbf{a}_1+x_2\mathbf{a}_2+\cdots+x_n\mathbf{a}_n=\mathbf{b}
{{< /katex >}}
{{% /hint %}}

### Geometric view

Each equation corresponds to a **hyperplane** in \(n\)-dimensional space.

The solution set is where all hyperplanes intersect:
- one point (unique solution)
- no intersection (no solution)
- an intersection line/plane/subspace (infinitely many solutions)

---

## Methods of solving \(A\mathbf{x}=\mathbf{b}\)

### Inverse method

{{% hint danger %}}
{{< katex display=true >}}
\mathbf{x}=A^{-1}\mathbf{b}
{{< /katex >}}
{{% /hint %}}

{{% hint info %}}
Mainly for theory; numerically expensive and unstable for large systems.
{{% /hint %}}

### Gaussian elimination (recommended)

Solve by row-reducing the augmented matrix:

{{% hint danger %}}
{{< katex display=true >}}
[A\mid \mathbf{b}]
{{< /katex >}}
{{% /hint %}}

This leads to REF/RREF and reveals whether the system is:
- inconsistent
- uniquely solvable
- underdetermined (free variables)

### Cramer's rule

Uses determinants to solve each variable (only for square systems with \(\det(A)\neq 0\)).

{{% hint info %}}
Useful for small \(n\), but not used for large systems.
{{% /hint %}}

### LU decomposition

Factor:

{{% hint danger %}}
{{< katex display=true >}}
A=LU
{{< /katex >}}
{{% /hint %}}

where:
- \(L\) is lower triangular
- \(U\) is upper triangular

Solve using forward + backward substitution.

---

## Consistency of Linear Systems

Consider a system of linear equations written in matrix form:

{{< katex display=true >}}
A\mathbf{x} = \mathbf{b}
{{< /katex >}}

- (A) → **Coefficient matrix**
- ([A | b]) → **Augmented matrix**

---

## Notes
- Consistent vs inconsistent systems
- Unique vs infinitely many solutions
- Geometric interpretation (lines/planes)

---

{{< home-link "Home" >}} | {{< section-index >}}
