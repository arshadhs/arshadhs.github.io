---
title: "Systems of Linear Equations"
draft: false
tags: ["Machine Learning", "Mathematics", "Linear Algebra"]
categories: ["AI", "ML"]
weight: 1130
---

# Systems of Linear Equations

A system of linear equations can be written compactly as:

{{< colour "green" >}}
{{< katex display=true >}}
A\mathbf{x}=\mathbf{b}
{{< /katex >}}
{{< /colour >}}

This represents:
- a linear transformation applied to an unknown vector \(\mathbf{x}\)
- producing an output vector \(\mathbf{b}\)

---

## Key components

### Coefficient matrix \(A\)

\(A\) contains the coefficients of the variables.

{{< colour "green" >}}
{{< katex display=true >}}
A \in \mathbb{R}^{m\times n}
{{< /katex >}}
{{< /colour >}}

### Variable vector \(\mathbf{x}\)

\(\mathbf{x}\) contains the unknowns.

{{< colour "green" >}}
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
{{< /colour >}}

### Constant vector \(\mathbf{b}\)

\(\mathbf{b}\) contains constants on the right-hand side.

{{< colour "green" >}}
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
{{< /colour >}}

---

## When does a solution exist?

The type of solution depends on \(A\).

### Unique solution

If \(A\) is square and invertible, then the solution is unique.

{{< colour "green" >}}
{{< katex display=true >}}
\det(A)\neq 0
{{< /katex >}}
{{< /colour >}}

{{< colour "green" >}}
{{< katex display=true >}}
\mathbf{x}=A^{-1}\mathbf{b}
{{< /katex >}}
{{< /colour >}}

### No solution or infinitely many solutions

{{< colour "green" >}}
{{< katex display=true >}}
\det(A)=0
{{< /katex >}}
{{< /colour >}}

---

## Homogeneous systems

{{< colour "green" >}}
{{< katex display=true >}}
A\mathbf{x}=\mathbf{0}
{{< /katex >}}
{{< /colour >}}

{{< colour "green" >}}
{{< katex display=true >}}
A\mathbf{x}=\mathbf{0}\iff \det(A)=0
{{< /katex >}}
{{< /colour >}}

---

## Linear combination view

{{< colour "green" >}}
{{< katex display=true >}}
x_1\mathbf{a}_1+x_2\mathbf{a}_2+\cdots+x_n\mathbf{a}_n=\mathbf{b}
{{< /katex >}}
{{< /colour >}}

---

## Methods

{{< colour "green" >}}
{{< katex display=true >}}
[A|\mathbf{b}]
{{< /katex >}}
{{< /colour >}}

{{< colour "green" >}}
{{< katex display=true >}}
A=LU
{{< /katex >}}
{{< /colour >}}

---

{{< home-link "Home" >}} | {{< section-index >}}
