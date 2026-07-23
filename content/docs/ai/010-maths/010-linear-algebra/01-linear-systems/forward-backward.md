---
title: "Forward and Backward Substitution"
draft: false
tags: ["Machine Learning", "Mathematics", "Linear Algebra"]
categories: ["AI", "ML"]
weight: 1155
---

# Forward and Backward Substitution

Forward and backward substitution are efficient algorithms used to solve linear systems when the coefficient matrix is **triangular**.

They are typically used after:
- Gaussian elimination
- LU decomposition

---

# 1. Forward Substitution (Lower Triangular Systems)

Used to solve:

{{% hint danger %}}
{{< katex display=true >}}
L\mathbf{x} = \mathbf{b}
{{< /katex >}}
{{% /hint %}}

where \(L\) is a **lower triangular matrix**:

{{% hint danger %}}
{{< katex display=true >}}
\ell_{ij} = 0 \quad \text{for } i < j
{{< /katex >}}
{{% /hint %}}

---

## Structure of the system

{{% hint danger %}}
{{< katex display=true >}}
\begin{aligned}
\ell_{11}x_1 &= b_1 \\
\ell_{21}x_1 + \ell_{22}x_2 &= b_2 \\
\ell_{31}x_1 + \ell_{32}x_2 + \ell_{33}x_3 &= b_3 \\
\vdots \\
\ell_{n1}x_1 + \cdots + \ell_{nn}x_n &= b_n
\end{aligned}
{{< /katex >}}
{{% /hint %}}

---

## Algorithm

Start from the first equation and move downward.

{{% hint danger %}}
{{< katex display=true >}}
x_i
=
\frac{1}{\ell_{ii}}
\left(
b_i
-
\sum_{j=1}^{i-1} \ell_{ij} x_j
\right),
\quad i=1,\dots,n
{{< /katex >}}
{{% /hint %}}

---

## Important condition

{{% hint warning %}}
Diagonal entries must satisfy:
{{< katex >}}\ell_{ii} \neq 0{{< /katex >}}
{{% /hint %}}

Otherwise, division is not possible and the system does not have a unique solution.

---

# 2. Backward Substitution (Upper Triangular Systems)

Used to solve:

{{% hint danger %}}
{{< katex display=true >}}
U\mathbf{x} = \mathbf{b}
{{< /katex >}}
{{% /hint %}}

where \(U\) is an **upper triangular matrix**:

{{% hint danger %}}
{{< katex display=true >}}
u_{ij} = 0 \quad \text{for } i > j
{{< /katex >}}
{{% /hint %}}

---

## Structure of the system

{{% hint danger %}}
{{< katex display=true >}}
\begin{aligned}
u_{11}x_1 + \cdots + u_{1n}x_n &= b_1 \\
\vdots \\
u_{n-1,n-1}x_{n-1} + u_{n-1,n}x_n &= b_{n-1} \\
u_{nn}x_n &= b_n
\end{aligned}
{{< /katex >}}
{{% /hint %}}

---

## Algorithm

Start from the last equation and move upward.

{{% hint danger %}}
{{< katex display=true >}}
x_i
=
\frac{1}{u_{ii}}
\left(
b_i
-
\sum_{j=i+1}^{n} u_{ij} x_j
\right),
\quad i=n,\dots,1
{{< /katex >}}
{{% /hint %}}

---

## Important condition

{{% hint warning %}}
Diagonal entries must satisfy:
{{< katex >}}u_{ii} \neq 0{{< /katex >}}
{{% /hint %}}

---

# Application in LU Decomposition

To solve a general system:

{{% hint danger %}}
{{< katex display=true >}}
A\mathbf{x} = \mathbf{b}
{{< /katex >}}
{{% /hint %}}

### Step 1: Factorise

Decompose:

{{% hint danger %}}
{{< katex display=true >}}
A = LU
{{< /katex >}}
{{% /hint %}}

where:
- \(L\) is lower triangular
- \(U\) is upper triangular

---

### Step 2: Forward substitution

Solve:

{{% hint danger %}}
{{< katex display=true >}}
L\mathbf{y} = \mathbf{b}
{{< /katex >}}
{{% /hint %}}

---

### Step 3: Backward substitution

Solve:

{{% hint danger %}}
{{< katex display=true >}}
U\mathbf{x} = \mathbf{y}
{{< /katex >}}
{{% /hint %}}

This yields the final solution \(\mathbf{x}\).

---

# Key Characteristics

### Efficiency

Both algorithms have computational complexity:

{{% hint danger %}}
{{< katex display=true >}}
\mathcal{O}(n^2)
{{< /katex >}}
{{% /hint %}}

This is much cheaper than computing an inverse.

---

### Requirements

- Triangular matrix
- Non-zero diagonal entries
- System must be consistent

---

### Why it matters in Machine Learning

Forward and backward substitution are used in:

- Solving least-squares problems
- Linear regression
- Cholesky decomposition
- LU factorisation
- Large sparse systems

They are numerically stable and computationally efficient compared to computing matrix inverses.

---

{{< home-link "Home" >}} | {{< section-index >}}