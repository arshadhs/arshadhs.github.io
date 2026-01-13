---
title: "Matrices"
draft: false
tags: ["Machine Learning", "Mathematics", "Linear Algebra"]
categories: ["AI", "ML"]
weight: 1140
---

## Scalar

### Definition
A **scalar** is a single numerical value representing **magnitude only**.

{{< katex display=true >}}
\alpha \in \mathbb{R}
{{< /katex >}}

Scalars do not have direction — only size.

### Geometric Intuition
A scalar is like a number on a number line.  
It tells you **how much**, not **which way**.

### In Machine Learning
- Learning rates  
- Bias terms  
- Loss values  

---

## Matrices

### Definition
A **matrix** is a rectangular array of numbers arranged in rows and columns.

{{< katex display=true >}}
A \in \mathbb{R}^{m \times n}
{{< /katex >}}

An m × n matrix has:
- m rows
- n columns

{{< katex display=true >}}
\mathbf{x} =
\begin{bmatrix}
1 2 3 \\
4 5 6 \\
\end{bmatrix}
{{< /katex >}}

---

## Vectors

A **vector** is a matrix with only one row or one column.

{{< katex display=true >}}
\text{Row vector: } 1 \times n \qquad
\text{Column vector: } m \times 1
{{< /katex >}}

Vectors represent:
- data points
- feature sets
- model parameters

---

## Equality of Matrices

Two matrices are equal if:
1. They have the same dimensions
2. All corresponding entries are equal

---

## Matrix Operations

### Matrix Addition

- Matrices should have the same size (e.g. A mxn and B mxn)
{{< katex display=true >}}
{A}^{m \times n} and {B}^{m \times n} 
{{< /katex >}}
- Addition is performed element-wise

Properties:
- Commutative:
{{< katex display=true >}}
A + B = B + A
{{< /katex >}}

- Associative:
{{< katex display=true >}}
(A + B) + C = A + (B + C)
{{< /katex >}}

---

### Scalar Multiplication

Multiplying a matrix by a scalar multiplies **every entry**.

Properties:
{{< katex display=true >}}
c(A + B) = cA + cB
{{< /katex >}}

{{< katex display=true >}}
(c + k)A = cA + kA
{{< /katex >}}

{{< katex display=true >}}
(ck)A = c(kA)
{{< /katex >}}

{{< katex display=true >}}
1A = A
{{< /katex >}}

---

## Matrix Multiplication (Very Important)

If AB = 0 does not necessarily imply BA = 0 or A = 0 or B = 0.

If:
- A ∈ ℝ^{m × n}
- B ∈ ℝ^{n × p}

then:

{{< katex display=true >}}
AB \in \mathbb{R}^{m \times p}
{{< /katex >}}

{{% hint warning %}}
Matrix multiplication is **not commutative** in general.
{{% /hint %}}

{{< katex display=true >}}
AB \neq BA
{{< /katex >}}

---

{{< home-link "Home" >}} | {{< section-index >}}
