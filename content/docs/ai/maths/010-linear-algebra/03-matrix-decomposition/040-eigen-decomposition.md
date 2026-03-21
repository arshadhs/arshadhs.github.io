---
title: "Eigen Decomposition"
draft: false
tags: ["Machine Learning", "Mathematics", "Linear Algebra"]
categories: ["AI", "ML"]
weight: 1350
---

# Eigen Decomposition

Eigen decomposition expresses a matrix using its eigenvectors and eigenvalues.

From lecture discussions, this is one of the most important ways to understand the internal structure of a matrix.

Instead of treating the matrix as a black box, eigen decomposition reveals its fundamental directions and scaling behaviour.

{{% hint info %}}
Key Idea:
Eigen decomposition rewrites a matrix in terms of directions (eigenvectors) and scaling factors (eigenvalues).
This makes complex transformations easier to understand and compute.
{{% /hint %}}

---

## Core Formula

For a square matrix:

{{< colour "green" >}}
{{< katex display=true >}}
A = P D P^{-1}
{{< /katex >}}
{{< /colour >}}

Where:
- P = matrix of eigenvectors  
- D = diagonal matrix of eigenvalues  

---

## Why It Works (Lecture Insight)

From lectures:

- Matrix multiplication represents a transformation  
- Most vectors change direction  
- Eigenvectors do NOT change direction  

They only scale:

{{< colour "green" >}}
{{< katex display=true >}}
A v = \lambda v
{{< /katex >}}
{{< /colour >}}

So if we express everything in eigenvector basis:
- transformation becomes simple scaling  

---

## Connection to Diagonalization

Eigen decomposition is essentially diagonalization.

{{< colour "green" >}}
{{< katex display=true >}}
D = P^{-1} A P
{{< /katex >}}
{{< /colour >}}

This shows:
- A becomes diagonal in eigenvector basis  
- diagonal entries = eigenvalues  

---

## Conditions for Eigen Decomposition

From lecture:

Matrix must have:
- n independent eigenvectors  

{{< colour "green" >}}
{{< katex display=true >}}
\text{rank}(P) = n
{{< /katex >}}
{{< /colour >}}

If not:
- matrix is not diagonalizable  
- decomposition fails  

---

## Special Case: Symmetric Matrices

From slides:

Symmetric matrices have stronger properties:

{{< colour "green" >}}
{{< katex display=true >}}
A = Q \Lambda Q^T
{{< /katex >}}
{{< /colour >}}

Where:
- Q is orthogonal  
- eigenvectors are orthonormal  

Lecture insight:
This makes computations more stable and simpler.

---

## Why Eigen Decomposition is Useful

### 1. Matrix Powers

{{< colour "green" >}}
{{< katex display=true >}}
A^k = P D^k P^{-1}
{{< /katex >}}
{{< /colour >}}

---

### 2. Understanding Structure

- Eigenvalues → scaling  
- Eigenvectors → directions  

---

### 3. Simplifying Computation

- Diagonal matrix is easy to compute  
- No cross interactions  

---

## Connection to SVD

From lectures:

- Eigen decomposition works for square matrices  
- SVD generalises this to all matrices  

Also:

{{< colour "green" >}}
{{< katex display=true >}}
\sigma_i = \sqrt{\lambda_i(A^T A)}
{{< /katex >}}
{{< /colour >}}

---

## Example

Given matrix:

{{< colour "green" >}}
{{< katex display=true >}}
A =
\begin{bmatrix}
4 & 2 \\
1 & 3
\end{bmatrix}
{{< /katex >}}
{{< /colour >}}

Steps:
1. Find eigenvalues  
2. Find eigenvectors  
3. Form P and D  

---

## Geometric Interpretation

From lecture:

- Eigenvectors define axes  
- Eigenvalues define scaling along axes  

So transformation becomes:
- stretch or shrink along fixed directions  

---

## Hidden Exam Pattern

From lectures:

- Eigen decomposition is combined with:
  - diagonalization  
  - matrix powers  
  - SVD  

---

## Common Mistakes

- Not checking independence of eigenvectors  
- Confusing eigenvalues with singular values  
- Incorrect inverse of P  
- Skipping verification step  

---

## Strategy to Prepare

1. Practice eigenvalue problems  
2. Solve eigenvector systems  
3. Form decomposition clearly  
4. Link with diagonalization  

---

## Quick Summary Table

| Concept | Meaning |
|--------|--------|
| A = PDP⁻¹ | Eigen decomposition |
| D | Eigenvalues |
| P | Eigenvectors |
| Condition | independent eigenvectors |

---

## References

- Lecture slides  
- Course handout  
- Webinar discussions  

- [Khan Academy](https://www.khanacademy.org/math/linear-algebra/alternate-bases/eigen-everything/v/linear-algebra-introduction-to-eigenvalues-and-eigenvectors)


---

{{< home-link "Home" >}} | {{< section-index >}}
