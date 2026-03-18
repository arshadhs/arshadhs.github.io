---
title: "Eigenvalues and Eigenvectors II"
date: 2026-03-18
draft: false
weight: 1401
tags: ["Machine Learning", "Linear Algebra", "Eigenvalues"]
categories: ["AI", "ML"]
---

# Eigenvalues and Eigenvectors

Eigenvalues and eigenvectors are fundamental concepts in linear algebra that describe how a matrix transforms vectors.

They help us understand:
- Directional scaling
- Matrix behaviour
- Dimensionality reduction (very important for ML)

{{% hint info %}}
Key Idea:
A matrix transformation stretches or compresses vectors. Eigenvectors are the directions that remain unchanged (only scaled), and eigenvalues tell us how much scaling happens.
{{% /hint %}}

---

# Definition

For a square matrix \( A \in \mathbb{R}^{n \times n} \):

{{< colour "green" >}}
\[
A v = \lambda v
\]
{{< /colour >}}

Where:
- \( v \neq 0 \) → eigenvector  
- \( \lambda \) → eigenvalue  

---

# Intuition (from Lecture + Webinar)

From lecture discussions:

- Matrix multiplication = transformation  
- Most vectors change **direction + magnitude**
- But some special vectors:
  - Only **scale**
  - Do NOT change direction

These are eigenvectors.

---

# Geometric Interpretation

- Eigenvector → direction stays same  
- Eigenvalue → scaling factor  

| Eigenvalue | Meaning |
|----------|--------|
| \( \lambda > 1 \) | Stretch |
| \( 0 < \lambda < 1 \) | Shrink |
| \( \lambda = 1 \) | No change |
| \( \lambda = 0 \) | Collapse to zero |
| \( \lambda < 0 \) | Flip direction |

---

# How to Find Eigenvalues

Step 1: Start from definition  
Step 2: Rearrange  

{{< colour "green" >}}
\[
(A - \lambda I)v = 0
\]
{{< /colour >}}

Step 3: Non-trivial solution exists only if:

{{< colour "green" >}}
\[
\det(A - \lambda I) = 0
\]
{{< /colour >}}

This is called the **characteristic equation**.

---

# Example

Let:

{{< colour "green" >}}
\[
A =
\begin{bmatrix}
4 & 2 \\
1 & 3
\end{bmatrix}
\]
{{< /colour >}}

Characteristic equation:

{{< colour "green" >}}
\[
\det(A - \lambda I) =
\begin{vmatrix}
4-\lambda & 2 \\
1 & 3-\lambda
\end{vmatrix}
= 0
\]
{{< /colour >}}

Solve → eigenvalues.

---

# Finding Eigenvectors

For each eigenvalue \( \lambda \):

Solve:

{{< colour "green" >}}
\[
(A - \lambda I)v = 0
\]
{{< /colour >}}

This is a **homogeneous system**.

👉 From lecture:
- This relates to **null space**  
- Eigenvectors lie in **null space of \( A - \lambda I \)**  

---

# Important Connections (Exam Insight)

From lectures:

- Solution of \( Ax = b \) uses pivot columns  
- Solution of \( Ax = 0 \) → null space :contentReference[oaicite:1]{index=1}  

👉 Eigenvectors are exactly:
- **non-zero solutions of a homogeneous system**

---

# Properties

1. A matrix can have multiple eigenvalues  
2. Eigenvectors corresponding to distinct eigenvalues are linearly independent  
3. Number of eigenvectors determines diagonalizability  

---

# Diagonalisation Link

If matrix has enough independent eigenvectors:

{{< colour "green" >}}
\[
A = PDP^{-1}
\]
{{< /colour >}}

Where:
- \( D \) = diagonal matrix of eigenvalues  
- \( P \) = matrix of eigenvectors  

---

# Why Important (ML Perspective)

Eigenvalues & eigenvectors are used in:

- PCA (Principal Component Analysis)
- Dimensionality reduction
- Covariance matrix analysis
- SVD (VERY IMPORTANT for exam)

---

# Common Exam Questions

Based on past papers + webinar patterns:

1. Find eigenvalues of a matrix  
2. Find eigenvectors  
3. Check if matrix is diagonalizable  
4. Relation with null space  
5. Interpret geometrically  

---

# Hidden Exam Pattern (Important)

From course discussion:

- Questions are NOT direct formula-based  
- Expect:
  - Concept + computation combined  
  - Link with rank/null space  
  - Application-based questions  

---

# Mistakes to Avoid

- Forgetting determinant condition  
- Taking zero vector as eigenvector ❌  
- Not solving full null space  
- Missing multiplicity cases  

---

# Strategy to Prepare

1. Practice determinant + solving systems  
2. Understand null space deeply  
3. Solve at least 10–15 matrices  
4. Connect with:
   - Rank
   - Diagonalisation
   - SVD  

---

# References

- T1 Sections 4.1, 4.2  
- Lecture slides  
- Webinar problem discussions  

- [My Notes: Linear Systems]({{% relref "linear-systems.md" %}})
- [My Notes: Vector Spaces]({{% relref "vector-spaces.md" %}})

---

{{< home-link "Home" >}} | {{< section-index >}}

---