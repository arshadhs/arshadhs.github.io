---
title: "Cholesky Decomposition"
draft: false
tags: ["Machine Learning", "Mathematics", "Linear Algebra"]
categories: ["AI", "ML"]
weight: 1330
---

# Cholesky Decomposition

Cholesky decomposition is a special matrix factorisation used for symmetric positive definite matrices.

From lecture discussions, this decomposition is powerful because it reduces a matrix into a triangular form, making computations easier and more stable.

{{% hint info %}}
Key Idea:
Cholesky decomposition expresses a matrix as a product of a lower triangular matrix and its transpose.
It is efficient and numerically stable.
{{% /hint %}}

---

## Definition

For a symmetric positive definite matrix:

{{< colour "green" >}}
{{< katex display=true >}}
A = L L^T
{{< /katex >}}
{{< /colour >}}

Where:
- L is a lower triangular matrix  
- diagonal entries of L are positive  

---

## Conditions for Cholesky

From lectures:

Matrix must be:

1. Symmetric  

{{< colour "green" >}}
{{< katex display=true >}}
A = A^T
{{< /katex >}}
{{< /colour >}}

2. Positive definite  

{{< colour "green" >}}
{{< katex display=true >}}
x^T A x > 0 \quad \forall x \neq 0
{{< /katex >}}
{{< /colour >}}

If these conditions are not satisfied → Cholesky cannot be applied.

---

## Why It Works (Lecture Insight)

From lecture explanation:

- Symmetry ensures consistent structure  
- Positive definiteness ensures square roots are valid  
- Triangular form simplifies solving systems  

So instead of solving Ax = b directly, we solve:

{{< colour "green" >}}
{{< katex display=true >}}
L y = b
{{< /katex >}}
{{< /colour >}}

{{< colour "green" >}}
{{< katex display=true >}}
L^T x = y
{{< /katex >}}
{{< /colour >}}

---

## Step-by-Step Computation

From slides:

To compute L:

1. Fill entries row by row  
2. Use previously computed values  
3. Diagonal entries involve square roots  

Example formula:

{{< colour "green" >}}
{{< katex display=true >}}
l_{ii} = \sqrt{a_{ii} - \sum_{k=1}^{i-1} l_{ik}^2}
{{< /katex >}}
{{< /colour >}}

---

## Example

Given:

{{< colour "green" >}}
{{< katex display=true >}}
A =
\begin{bmatrix}
4 & 2 \\
2 & 3
\end{bmatrix}
{{< /katex >}}
{{< /colour >}}

We compute L such that:

{{< colour "green" >}}
{{< katex display=true >}}
A = L L^T
{{< /katex >}}
{{< /colour >}}

---

## Geometric Interpretation

From lecture:

- Matrix defines quadratic form  
- Cholesky expresses it as squared transformation  

This simplifies analysis of shapes like ellipses.

---

## Why Cholesky is Useful

From lecture and webinar:

### 1. Solving Linear Systems

Efficient alternative to Gaussian elimination.

---

### 2. Numerical Stability

- Fewer operations  
- Reduced rounding errors  

---

### 3. Machine Learning Applications

- Multivariate Gaussian sampling  
- Covariance matrices  
- Optimisation algorithms  

---

## Connection to Other Decompositions

- Similar to LU decomposition but specialised  
- More efficient when conditions are met  
- Used before SVD in some pipelines  

---

## Hidden Exam Pattern

From lectures:

- Questions focus on:
  - checking conditions  
  - constructing L  
  - solving systems  

---

## Common Mistakes

- Applying to non-symmetric matrix  
- Ignoring positive definiteness  
- Incorrect square root calculation  
- Mixing L and U forms  

---

## Strategy to Prepare

1. Check symmetry and positivity first  
2. Practice small matrix examples  
3. Memorise computation steps  
4. Understand application in solving systems  

---

## Quick Summary Table

| Concept | Meaning |
|--------|--------|
| A = LL^T | Cholesky decomposition |
| Condition | symmetric + positive definite |
| L | lower triangular |
| Use | efficient solving |

---

## References

- Lecture slides (Matrix Decomposition)  
- Course handout  
- Webinar discussions  

- [My Notes: Diagonalization]({{% relref "diagonalization.md" %}})
- [My Notes: SVD]({{% relref "svd.md" %}})

---

{{< home-link "Home" >}} | {{< section-index >}}
