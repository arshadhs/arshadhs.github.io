---
title: "Eigenvalues and Eigenvectors"
draft: false
tags: ["Machine Learning", "Mathematics", "Linear Algebra"]
categories: ["AI", "ML"]
weight: 1320
---

# Eigenvalues and Eigenvectors

- Eigenvalues give **scaling**.
- Eigenvectors define **invariant directions** of transformation.

Eigenvalues and eigenvectors describe directions that remain unchanged under a linear transformation, except for scaling.

Let {{< katex >}}A \in \mathbb{R}^{n \times n}{{< /katex >}}.

A scalar {{< katex >}}\lambda \in \mathbb{R}{{< /katex >}} is an **eigenvalue** of {{< katex >}}A{{< /katex >}}, and a non-zero vector  
{{< katex >}}\mathbf{x} \in \mathbb{R}^n \setminus \{0\}{{< /katex >}}  
is an **eigenvector** corresponding to {{< katex >}}\lambda{{< /katex >}} if:

{{% hint danger %}}
{{< katex display=true >}}
A\mathbf{x} = \lambda \mathbf{x}
{{< /katex >}}
{{% /hint %}}

This is called the **eigenvalue equation**.

They are fundamental in:
- PCA  
- Optimisation  
- Spectral methods  
- Stability analysis  
- Least squares  
- Neural networks  

---

# Equivalent Characterisations

The following statements are equivalent:

1. {{< katex >}}\lambda{{< /katex >}} is an eigenvalue of {{< katex >}}A{{< /katex >}}.

2. There exists {{< katex >}}\mathbf{x} \neq 0{{< /katex >}} such that:

{{% hint danger %}}
{{< katex display=true >}}
(A - \lambda I)\mathbf{x} = 0
{{< /katex >}}
{{% /hint %}}

3. The system has a **non-trivial solution**.

4. Rank condition:

{{% hint danger %}}
{{< katex display=true >}}
\operatorname{rank}(A - \lambda I) < n
{{< /katex >}}
{{% /hint %}}

5. Determinant condition:

{{% hint danger %}}
{{< katex display=true >}}
\det(A - \lambda I) = 0
{{< /katex >}}
{{% /hint %}}

---

# Characteristic Polynomial

{{% hint danger %}}
{{< katex display=true >}}
p_A(\lambda) = \det(A - \lambda I)
{{< /katex >}}
{{% /hint %}}

Eigenvalues are the **roots** of the characteristic polynomial.

---

# Scaling Property

If {{< katex >}}\mathbf{x}{{< /katex >}} is an eigenvector corresponding to {{< katex >}}\lambda{{< /katex >}}, then:

{{% hint danger %}}
{{< katex display=true >}}
c\mathbf{x}, \quad c \in \mathbb{R} \setminus \{0\}
{{< /katex >}}
{{% /hint %}}

is also an eigenvector.

Eigenvectors are defined **up to scaling**.

---

# Worked Example

Consider:

{{% hint danger %}}
{{< katex display=true >}}
A =
\begin{bmatrix}
1 & 1 \\
1 & 1
\end{bmatrix}
{{< /katex >}}
{{% /hint %}}

## Step 1: Characteristic Polynomial

{{% hint danger %}}
{{< katex display=true >}}
\det(A - \lambda I)
=
\begin{vmatrix}
1-\lambda & 1 \\
1 & 1-\lambda
\end{vmatrix}
=
(1-\lambda)^2 - 1
{{< /katex >}}
{{% /hint %}}

Solve:

{{% hint danger %}}
{{< katex display=true >}}
(1-\lambda)^2 - 1 = 0
{{< /katex >}}
{{% /hint %}}

Eigenvalues:

{{% hint danger %}}
{{< katex display=true >}}
\lambda = 2, \quad 0
{{< /katex >}}
{{% /hint %}}

---

## Step 2: Eigenvectors

For {{< katex >}}\lambda = 0{{< /katex >}}:

{{% hint danger %}}
{{< katex display=true >}}
\mathbf{x} =
\begin{bmatrix}
1 \\
-1
\end{bmatrix}
{{< /katex >}}
{{% /hint %}}

For {{< katex >}}\lambda = 2{{< /katex >}}:

{{% hint danger %}}
{{< katex display=true >}}
\mathbf{x} =
\begin{bmatrix}
1 \\
1
\end{bmatrix}
{{< /katex >}}
{{% /hint %}}

---

# Eigenspace

For eigenvalue {{< katex >}}\lambda{{< /katex >}}, the eigenspace is:

{{% hint danger %}}
{{< katex display=true >}}
E_\lambda = \operatorname{Null}(A - \lambda I)
{{< /katex >}}
{{% /hint %}}

It is a subspace of {{< katex >}}\mathbb{R}^n{{< /katex >}}.

---

# Spectrum

The set of all eigenvalues of {{< katex >}}A{{< /katex >}} is called the **spectrum** of {{< katex >}}A{{< /katex >}}.

---

# Important Properties

### Transpose Property

{{% hint danger %}}
{{< katex display=true >}}
\det(A - \lambda I)
=
\det(A^T - \lambda I)
{{< /katex >}}
{{% /hint %}}

Therefore, {{< katex >}}A{{< /katex >}} and {{< katex >}}A^T{{< /katex >}} have the same eigenvalues.

---

### Distinct Eigenvalues

If an {{< katex >}}n \times n{{< /katex >}} matrix has {{< katex >}}n{{< /katex >}} distinct eigenvalues, its eigenvectors are linearly independent.

---

### Identity Matrix Example

For {{< katex >}}I_n{{< /katex >}}:

{{% hint danger %}}
{{< katex display=true >}}
I_n \mathbf{x} = 1 \cdot \mathbf{x}
{{< /katex >}}
{{% /hint %}}

- Only eigenvalue: {{< katex >}}\lambda = 1{{< /katex >}}  
- Eigenspace: {{< katex >}}\mathbb{R}^n{{< /katex >}}

---

# Symmetric Matrices

If {{< katex >}}A{{< /katex >}} is symmetric:

- All eigenvalues are **real**
- Eigenvectors for distinct eigenvalues are **orthogonal**

---

# Spectral Theorem

If {{< katex >}}A \in \mathbb{R}^{n \times n}{{< /katex >}} is symmetric:

- There exists an orthonormal basis of eigenvectors
- All eigenvalues are real

Diagonalisation:

{{% hint danger %}}
{{< katex display=true >}}
A = Q \Lambda Q^T
{{< /katex >}}
{{% /hint %}}

Where:
- {{< katex >}}Q{{< /katex >}} is orthogonal  
- {{< katex >}}\Lambda{{< /katex >}} is diagonal  

---

# Machine Learning Connection

If {{< katex >}}A \in \mathbb{R}^{m \times n}{{< /katex >}}, then:

{{% hint danger %}}
{{< katex display=true >}}
A^T A
{{< /katex >}}
{{% /hint %}}

is symmetric and positive definite (if {{< katex >}}\operatorname{rank}(A)=n{{< /katex >}}), because:

{{% hint danger %}}
{{< katex display=true >}}
\mathbf{x}^T A^T A \mathbf{x}
=
\|A\mathbf{x}\|^2 > 0
{{< /katex >}}
{{% /hint %}}

Appears in:
- Linear regression  
- Normal equations  
- PCA  

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