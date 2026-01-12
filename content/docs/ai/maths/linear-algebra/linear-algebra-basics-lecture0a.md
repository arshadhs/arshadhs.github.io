---
title: "Linear Algebra Basics (Lecture 0a)"
draft: false
tags: ["Maths", "Linear Algebra", "Octave", "Machine Learning"]
categories: ["AI", "ML", "Maths"]
weight: 1125
---

# Linear Algebra Basics

## Matrices

### Definition
A **matrix** is a rectangular array of numbers arranged in rows and columns.

{{< katex display=true >}}
A \in \mathbb{R}^{m \times n}
{{< /katex >}}

An m × n matrix has:
- m rows
- n columns

```matlab
A = [1 2 3; 4 5 6];
```

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

- Possible only if matrices have the same size
- Addition is performed element-wise

{{< katex display=true >}}
A + B = B + A
{{< /katex >}}

{{< katex display=true >}}
(A + B) + C = A + (B + C)
{{< /katex >}}

---

### Scalar Multiplication

Multiplying a matrix by a scalar multiplies **every entry**.

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

## Transpose of a Matrix

The transpose of a matrix swaps rows and columns.

{{< katex display=true >}}
A^T \in \mathbb{R}^{n \times m}
{{< /katex >}}

### Key rules

{{< katex display=true >}}
(A^T)^T = A
{{< /katex >}}

{{< katex display=true >}}
(A + B)^T = A^T + B^T
{{< /katex >}}

{{< katex display=true >}}
(cA)^T = cA^T
{{< /katex >}}

{{< katex display=true >}}
(AB)^T = B^T A^T
{{< /katex >}}

---

## Special Matrices

### Symmetric Matrix
{{< katex display=true >}}
A^T = A
{{< /katex >}}

### Skew-Symmetric Matrix
{{< katex display=true >}}
A^T = -A
{{< /katex >}}

Diagonal elements are zero.

---

### Diagonal Matrix
Only diagonal entries are non-zero.

---

### Upper Triangular Matrix
All entries below the diagonal are zero.

---

### Lower Triangular Matrix
All entries above the diagonal are zero.

---

### Sparse Matrix
Most entries are zero.

{{% hint info %}}
Sparse matrices are common in NLP, recommender systems, and graph-based ML.
{{% /hint %}}

---

## Positive Definite Matrices

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
- all zero rows are at the bottom
- pivots move to the right as rows go down
- entries below pivots are zero

---

## Reduced Row Echelon Form (RREF)

RREF additionally requires:
- pivots are equal to 1
- each pivot is the only non-zero entry in its column

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

Free variables imply infinitely many solutions.

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

## Key Takeaways

- Linear algebra underpins all ML models
- Matrix multiplication is order-sensitive
- Rank determines solvability of systems
- Special matrices simplify computation
- Octave helps build strong intuition

---

{{< home-link "Home" >}} | {{< section-index >}}
