---
title: "Diagonalization"
draft: false
tags: ["Machine Learning", "Mathematics", "Linear Algebra"]
categories: ["AI", "ML"]
weight: 1347
---
# Diagonalization

Diagonalisation expresses a matrix using its eigenvectors and eigenvalues when possible.

From lecture explanation, diagonalisation is one of the most powerful tools because it converts a complicated matrix into a much simpler form.

Instead of working with a full matrix, we work with a diagonal matrix, which is much easier to analyse and compute.

{{% hint info %}}
Key Idea:
If a matrix has enough independent eigenvectors, it can be rewritten as a diagonal matrix using a change of basis.
This simplifies matrix operations significantly.
{{% /hint %}}

---

# Core Idea

A matrix is diagonalizable if we can write it as:

{{< colour "green" >}}
{{< katex display=true >}}
A = P D P^{-1}
{{< /katex >}}
{{< /colour >}}

Where:
- P is a matrix whose columns are eigenvectors  
- D is a diagonal matrix containing eigenvalues  

---

# Why Diagonalization Works (Lecture Insight)

From lectures:

- Matrix multiplication represents transformation  
- Eigenvectors are directions that do not change direction  
- Eigenvalues tell how much scaling happens  

So if we express everything in terms of eigenvectors:

- transformation becomes simple scaling  
- no mixing of components  

---

# Change of Basis Interpretation

Diagonalization is essentially a change of coordinate system.

{{< colour "green" >}}
{{< katex display=true >}}
D = P^{-1} A P
{{< /katex >}}
{{< /colour >}}

Interpretation:

- P converts to eigenvector basis  
- A acts as scaling  
- P^{-1} converts back  

From lecture:
this is why diagonal matrices are easy — they scale each coordinate independently.

---

# When is a Matrix Diagonalizable?

A matrix is diagonalizable if it has enough independent eigenvectors.

{{< colour "green" >}}
{{< katex display=true >}}
\text{number of independent eigenvectors} = n
{{< /katex >}}
{{< /colour >}}

Lecture rule:

- Distinct eigenvalues ⇒ automatically diagonalizable  
- Repeated eigenvalues ⇒ must check eigenvectors  

---

# Algebraic vs Geometric Multiplicity

From slides:

- Algebraic multiplicity = number of times eigenvalue appears  
- Geometric multiplicity = number of independent eigenvectors  

Condition:

{{< colour "green" >}}
{{< katex display=true >}}
\text{geometric multiplicity} = \text{algebraic multiplicity}
{{< /katex >}}
{{< /colour >}}

for diagonalization.

---

# Special Case: Symmetric Matrices

From lecture:

Symmetric matrices are always diagonalizable.

Even stronger:

{{< colour "green" >}}
{{< katex display=true >}}
A = Q \Lambda Q^T
{{< /katex >}}
{{< /colour >}}

Where:
- Q is orthogonal  
- eigenvectors are orthonormal  

This is called spectral decomposition.

---

# Why Diagonalization is Useful

From lecture and webinar:

### 1. Matrix Powers

{{< colour "green" >}}
{{< katex display=true >}}
A^k = P D^k P^{-1}
{{< /katex >}}
{{< /colour >}}

Easy because D is diagonal.

---

### 2. Matrix Inverse

{{< colour "green" >}}
{{< katex display=true >}}
A^{-1} = P D^{-1} P^{-1}
{{< /katex >}}
{{< /colour >}}

---

### 3. Understanding Structure

- Eigenvalues show scaling behaviour  
- Eigenvectors show directions  

---

# Step-by-Step Method (Exam)

1. Find eigenvalues using:

{{< colour "green" >}}
{{< katex display=true >}}
\det(A - \lambda I) = 0
{{< /katex >}}
{{< /colour >}}

2. Find eigenvectors:

{{< colour "green" >}}
{{< katex display=true >}}
(A - \lambda I)v = 0
{{< /katex >}}
{{< /colour >}}

3. Form matrix P using eigenvectors  

4. Form diagonal matrix D using eigenvalues  

5. Verify:

{{< colour "green" >}}
{{< katex display=true >}}
A = P D P^{-1}
{{< /katex >}}
{{< /colour >}}

---

# Example

Given:

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
- Find eigenvalues  
- Find eigenvectors  
- Construct P and D  

---

# Geometric Interpretation

- Original space → transformed space  
- Diagonalization aligns axes with eigenvectors  

Result:
- transformation becomes pure scaling  

---

# Connection to SVD

From lecture:

- Diagonalization works for square matrices  
- SVD generalises this idea  

Both aim to simplify matrix structure.

---

# Hidden Exam Pattern

From lectures:

- Often combined with:
  - eigenvalues  
  - matrix powers  
  - rank  

---

# Common Mistakes

- Not checking number of eigenvectors  
- Assuming repeated eigenvalues ⇒ diagonalizable  
- Incorrect inverse of P  
- Mixing order of multiplication  

---

# Strategy to Prepare

1. Practice eigenvalue problems  
2. Check independence of eigenvectors  
3. Practice forming P and D  
4. Solve matrix power problems  

---

# Quick Summary Table

| Concept | Meaning |
|--------|--------|
| A = PDP⁻¹ | Diagonalization |
| D | Eigenvalues |
| P | Eigenvectors |
| Condition | n independent eigenvectors |

---

# References

- Lecture slides (Eigenvalues, Decomposition)  
- Course handout  
- Webinar discussions  

---

{{< home-link "Home" >}} | {{< section-index >}}
