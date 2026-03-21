---
title: "Eigenvalues and Eigenvectors"
draft: false
tags: ["Machine Learning", "Mathematics", "Linear Algebra"]
categories: ["AI", "ML"]
weight: 1320
---

# Eigenvalues and Eigenvectors

- Eigenvalues give scaling.  
- Eigenvectors define invariant directions of transformation.  

Eigenvalues and eigenvectors describe directions that remain unchanged under a linear transformation, except for scaling.  

From lectures:
matrix multiplication represents a transformation of space.  
Most vectors change direction and magnitude.  
Some special vectors only scale.  
These are eigenvectors.  

{{% hint info %}}
Key Idea:
A matrix transformation stretches or compresses vectors.
Eigenvectors are directions that remain unchanged.
Eigenvalues tell how much scaling happens.
{{% /hint %}}

---

# Definition

Let:

{{< colour "green" >}}
{{< katex display=true >}}
A \in \mathbb{R}^{n \times n}
{{< /katex >}}
{{< /colour >}}

A scalar:

{{< colour "green" >}}
{{< katex display=true >}}
\lambda \in \mathbb{R}
{{< /katex >}}
{{< /colour >}}

and a non-zero vector:

{{< colour "green" >}}
{{< katex display=true >}}
\mathbf{x} \in \mathbb{R}^n \setminus \{0\}
{{< /katex >}}
{{< /colour >}}

is an eigenvector if:

{{< colour "green" >}}
{{< katex display=true >}}
A\mathbf{x} = \lambda \mathbf{x}
{{< /katex >}}
{{< /colour >}}

---

# Intuition (Lecture + Webinar)

- Matrix = transformation  
- Most vectors → rotate + scale  
- Eigenvectors → only scale  

Eigenvectors define the “natural directions” of a matrix.

---

# Geometric Interpretation

| Eigenvalue | Meaning |
|----------|--------|
| > 1 | Stretch |
| between 0 and 1 | Shrink |
| = 1 | No change |
| = 0 | Collapse |
| < 0 | Flip |

---

# Equivalent Characterisations

The following are equivalent:

{{< colour "green" >}}
{{< katex display=true >}}
(A - \lambda I)\mathbf{x} = 0
{{< /katex >}}
{{< /colour >}}

{{< colour "green" >}}
{{< katex display=true >}}
\operatorname{rank}(A - \lambda I) < n
{{< /katex >}}
{{< /colour >}}

{{< colour "green" >}}
{{< katex display=true >}}
\det(A - \lambda I) = 0
{{< /katex >}}
{{< /colour >}}

---

# Characteristic Polynomial

{{< colour "green" >}}
{{< katex display=true >}}
p_A(\lambda) = \det(A - \lambda I)
{{< /katex >}}
{{< /colour >}}

Eigenvalues are roots.

---

# How to Find Eigenvalues

{{< colour "green" >}}
{{< katex display=true >}}
\det(A - \lambda I) = 0
{{< /katex >}}
{{< /colour >}}

---

# Finding Eigenvectors

{{< colour "green" >}}
{{< katex display=true >}}
(A - \lambda I)\mathbf{x} = 0
{{< /katex >}}
{{< /colour >}}

Eigenvectors lie in:

{{< colour "green" >}}
{{< katex display=true >}}
\operatorname{Null}(A - \lambda I)
{{< /katex >}}
{{< /colour >}}

---

# Scaling Property

{{< colour "green" >}}
{{< katex display=true >}}
c\mathbf{x}, \quad c \neq 0
{{< /katex >}}
{{< /colour >}}

---

# Eigenspace

{{< colour "green" >}}
{{< katex display=true >}}
E_\lambda = \operatorname{Null}(A - \lambda I)
{{< /katex >}}
{{< /colour >}}

---

# Spectrum

Set of all eigenvalues = spectrum.

---

# Worked Example

{{< colour "green" >}}
{{< katex display=true >}}
A =
\begin{bmatrix}
1 & 1 \\
1 & 1
\end{bmatrix}
{{< /katex >}}
{{< /colour >}}

Eigenvalues:

{{< colour "green" >}}
{{< katex display=true >}}
\lambda = 2, 0
{{< /katex >}}
{{< /colour >}}

Eigenvectors:

{{< colour "green" >}}
{{< katex display=true >}}
\begin{bmatrix}1\\1\end{bmatrix}, \quad
\begin{bmatrix}1\\-1\end{bmatrix}
{{< /katex >}}
{{< /colour >}}

---

# Important Properties

### Distinct Eigenvalues

Eigenvectors are linearly independent.

---

### Transpose Property

{{< colour "green" >}}
{{< katex display=true >}}
\det(A - \lambda I) = \det(A^T - \lambda I)
{{< /katex >}}
{{< /colour >}}

---

# Symmetric Matrices

- Eigenvalues are real  
- Eigenvectors are orthogonal  

---

# Spectral Theorem

{{< colour "green" >}}
{{< katex display=true >}}
A = Q \Lambda Q^T
{{< /katex >}}
{{< /colour >}}

---

# Diagonalisation Link

{{< colour "green" >}}
{{< katex display=true >}}
A = P D P^{-1}
{{< /katex >}}
{{< /colour >}}

---

# Machine Learning Connection

{{< colour "green" >}}
{{< katex display=true >}}
A^T A
{{< /katex >}}
{{< /colour >}}

is symmetric positive definite:

{{< colour "green" >}}
{{< katex display=true >}}
x^T A^T A x = \|Ax\|^2 > 0
{{< /katex >}}
{{< /colour >}}

Used in:
- PCA  
- Regression  
- SVD  

---

# Common Exam Questions

- Find eigenvalues  
- Find eigenvectors  
- Check diagonalisation  
- Link with null space  
- Interpret geometrically  

---

# Hidden Exam Pattern

- Concept + computation  
- Links with rank, null space, SVD  

---

# Mistakes to Avoid

- Zero vector ❌  
- Missing determinant condition  
- Not solving null space fully  
- Ignoring multiplicity  

---

# Strategy to Prepare

1. Practice determinant  
2. Solve systems  
3. Connect to rank  
4. Link with diagonalisation  

---

# Summary

- Eigenvalues are roots of the characteristic polynomial
- Eigenspace is the nullspace of {{< katex >}}A - \lambda I{{< /katex >}}
- Symmetric matrices have real eigenvalues
- Distinct eigenvalues imply independence
- {{< katex >}}A^T A{{< /katex >}} is symmetric positive definite (full rank case)
- Spectral theorem provides orthonormal eigenbasis

---

{{< home-link "Home" >}} | {{< section-index >}}
