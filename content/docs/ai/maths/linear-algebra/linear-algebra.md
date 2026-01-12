---
title: "Linear Algebra Basics (Lecture 0a)"
draft: false
tags: ["Maths", "Linear Algebra", "Octave", "Machine Learning"]
categories: ["AI", "ML", "Maths"]
weight: 1125
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

An \( m \times n \) matrix has:
- \( m \) rows
- \( n \) columns

Example:

```matlab
A = ⎡1 2 3⎤
    ⎣4 5 6⎦    

```

---

## Vectors

A **vector** is a matrix with only one row or one column.

- Row vector → \( 1 \times n \)
- Column vector → \( m \times 1 \)
```matlab
a = [1 2 3]
```

---

## Equality of Matrices

Two matrices are equal if:
1. They have the same dimensions
2. All corresponding entries are equal

---

## Matrix Operations

### Matrix Addition

- Only possible if matrices have the same size
- Addition is element-wise

Properties:
- Commutative: \( A + B = B + A \)
- Associative: \( (A + B) + C = A + (B + C) \)

---

### Scalar Multiplication

Multiplying a matrix by a scalar multiplies **every entry**.

Properties:
- \( c(A + B) = cA + cB \)
- \( (c + k)A = cA + kA \)
- \( (ck)A = c(kA) \)
- \( 1A = A \)

---

## Matrix Multiplication (Very Important)

If:
- \( A \) is \( m \times n \)
- \( B \) is \( n \times p \)

Then:
- \( AB \) is defined
- Result is \( m \times p \)

Matrix multiplication is **not commutative**:
- \( AB \neq BA \) (in general)

If AB = 0 does not necessarily imply BA = 0 or A = 0 or B = 0.

---

## Transpose of a Matrix

The transpose of a matrix swaps rows and columns.

If \( A \) is \( m \times n \), then \( A^T \) is \( n \times m \).

### Key rules
- \( (A^T)^T = A \)
- \( (A + B)^T = A^T + B^T \)
- \( (cA)^T = cA^T \)
- \( (AB)^T = B^T A^T \)

---

## Special Matrices

### Symmetric Matrix
\( A^T = A \)

### Skew-Symmetric Matrix
\( A^T = -A \)

### Diagonal Matrix
Non-zero entries only on the main diagonal

### Upper Triangular Matrix
All entries below the diagonal are zero

### Lower Triangular Matrix
All entries above the diagonal are zero

### Sparse Matrix
- Most entries are zero.
- Storage and Processing efficiency for large datasets.
- Usage: Recommendation Systems.

---

## Positive Definite Matrices

For a real symmetric matrix \( A \):

- Positive definite if  
  \( x^T A x > 0 \) for all \( x \neq 0 \)

- Positive semi-definite if  
  \( x^T A x \ge 0 \)

{{% hint info %}}
Real symmetric matrix means all enteries are real numbers and \( A^T = A \)
{{% /hint %}}

Used heavily in:
- covariance matrices
- optimisation
- machine learning stability

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
- Leading entries move to the right as rows go down
- All entries below a pivot are zero

---

## Reduced Row Echelon Form (RREF)

RREF adds:
- Leading entries are 1
- Each leading 1 is the only non-zero entry in its column

RREF of a matrix is **unique**.

---

## Rank of a Matrix

Rank is:
- number of non-zero rows in REF or RREF
- invariant under row operations

---

## Solving Linear Systems

A system can have:
- **No solution** (inconsistent)
- **Unique solution**
- **Infinitely many solutions**

### Rank test
- If  
  \( \text{rank}(A) < \text{rank}([A|b]) \) → no solution
- If  
  \( \text{rank}(A) = \text{rank}([A|b]) \) → solution exists

Free variables → infinitely many solutions

---

## Useful Octave Commands

```matlab
zeros(m,n)     % zero matrix
eye(n)         % identity matrix
size(A)        % size of matrix
rank(A)        % rank
rref(A)        % reduced row echelon form
det(A)         % determinant
inv(A)         % inverse
issymmetric(A) % check symmetry
```

---

## Key Takeaways

- Linear algebra is the backbone of machine learning
- Matrix multiplication is order-sensitive
- REF, RREF, and rank determine system solutions
- Special matrices appear everywhere in ML
- Octave helps build intuition through practice

---

{{< home-link "Home" >}} | {{< section-index >}}
