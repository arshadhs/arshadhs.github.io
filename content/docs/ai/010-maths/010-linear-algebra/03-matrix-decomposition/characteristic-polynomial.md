---
title: "Characteristic Polynomial"
draft: false
tags: ["Machine Learning", "Mathematics", "Linear Algebra"]
categories: ["AI", "ML"]
weight: 1305
---

# Characteristic Polynomial

The **characteristic polynomial** of a square matrix is the key tool used to compute **eigenvalues**.

It connects:

- Determinants  
- Trace  
- Eigenvalues  
- Matrix structure  

---

## Definition

Let  
{{< katex >}}A \in \mathbb{R}^{n \times n}{{< /katex >}}  
and {{< katex >}}\lambda \in \mathbb{R}{{< /katex >}}.

The **characteristic polynomial** of \(A\) is defined as:

{{% hint danger %}}
{{< katex display=true >}}
p_A(\lambda) = \det(A - \lambda I)
{{< /katex >}}
{{% /hint %}}

It is a polynomial in {{< katex >}}\lambda{{< /katex >}} of degree \(n\).

---

## General Form

We can show that:

{{% hint danger %}}
{{< katex display=true >}}
p_A(\lambda)
=
c_0 + c_1 \lambda + \dots + c_{n-1} \lambda^{n-1}
+ (-1)^n \lambda^n
{{< /katex >}}
{{% /hint %}}

where  
{{< katex >}}c_0, c_1, \dots, c_{n-1} \in \mathbb{R}{{< /katex >}}.

---

## Important Coefficients

Two coefficients are especially important.
### 1. Constant Term

Set {{< katex >}}\lambda = 0{{< /katex >}}:

{{% hint danger %}}
{{< katex display=true >}}
p_A(0) = \det(A)
{{< /katex >}}
{{% /hint %}}

So:

{{% hint info %}}
{{< katex >}}c_0 = \det(A){{< /katex >}}
{{% /hint %}}

---

### 2. Coefficient of \( \lambda^{n-1} \)

One can show (via determinant expansion) that:

{{% hint danger %}}
{{< katex display=true >}}
c_{n-1} = (-1)^{n-1} \operatorname{tr}(A)
{{< /katex >}}
{{% /hint %}}

{{% hint info %}}
So the **trace of a matrix appears directly inside the characteristic polynomial**.
{{% /hint %}}

---

### Leading Term

The highest-degree term is always:

{{% hint danger %}}
{{< katex display=true >}}
(-1)^n \lambda^n
{{< /katex >}}
{{% /hint %}}

So:

{{% hint info %}}
The characteristic polynomial is always degree \(n\).
{{% /hint %}}

---

# Why These Coefficients Appear (Intuition from Expansion)

Consider a \(3 \times 3\) matrix:

{{% hint danger %}}
{{< katex display=true >}}
A - \lambda I =
\begin{bmatrix}
a_{11} - \lambda & a_{12} & a_{13} \\
a_{21} & a_{22} - \lambda & a_{23} \\
a_{31} & a_{32} & a_{33} - \lambda
\end{bmatrix}
{{< /katex >}}
{{% /hint %}}

When expanding the determinant:

- The product  
  {{< katex >}}\prod_{i=1}^{3} (a_{ii} - \lambda){{< /katex >}}  
  generates the highest powers of {{< katex >}}\lambda{{< /katex >}}.

- Other determinant terms contain fewer factors of {{< katex >}}\lambda{{< /katex >}}  
  and therefore produce lower powers.

---

### General Case (n × n)

When expanding along the first row:

- The term  
  {{< katex >}}\prod_{i=1}^{n}(a_{ii} - \lambda){{< /katex >}}  
  produces powers up to {{< katex >}}\lambda^n{{< /katex >}}.

- All other expansion terms contain minors where at least one  
  {{< katex >}}(a_{ii} - \lambda){{< /katex >}} factor is removed.

So:

- Only one contributor produces the \( \lambda^n \) term  
- Only that same contributor produces the \( \lambda^{n-1} \) term  

Which leads to:

{{% hint danger %}}
{{< katex display=true >}}
\text{Coefficient of } \lambda^n = (-1)^n
{{< /katex >}}
{{% /hint %}}

and

{{% hint danger %}}
{{< katex display=true >}}
\text{Coefficient of } \lambda^{n-1}
=
(-1)^{n-1} \sum_{i=1}^{n} a_{ii}
=
(-1)^{n-1} \operatorname{tr}(A)
{{< /katex >}}
{{% /hint %}}

---

# Connection to Eigenvalues

Eigenvalues are defined as the roots of the characteristic polynomial.

{{% hint danger %}}
{{< katex display=true >}}
\det(A - \lambda I) = 0
{{< /katex >}}
{{% /hint %}}

{{% hint info %}}
Solving this equation gives all eigenvalues of \(A\).
{{% /hint %}}

---

# Important Consequences

From the polynomial structure we obtain:

### 1️⃣ Product of Eigenvalues

{{% hint danger %}}
{{< katex display=true >}}
\prod_{i=1}^{n} \lambda_i = \det(A)
{{< /katex >}}
{{% /hint %}}

---

### 2️⃣ Sum of Eigenvalues

{{% hint danger %}}
{{< katex display=true >}}
\sum_{i=1}^{n} \lambda_i = \operatorname{tr}(A)
{{< /katex >}}
{{% /hint %}}

These follow from the relationship between polynomial coefficients and roots.

---

# Why This Matters in Machine Learning

Characteristic polynomials are used to:

- Compute eigenvalues (PCA, SVD foundations)
- Analyse stability of systems
- Understand diagonalisation
- Study covariance matrices
- Analyse Hessians in optimisation

{{% hint warning %}}
Every time you compute eigenvalues, you are solving  
{{< katex >}}\det(A - \lambda I) = 0{{< /katex >}}.
{{% /hint %}}

---

# Summary

- The characteristic polynomial is  
  {{< katex >}}p_A(\lambda) = \det(A - \lambda I){{< /katex >}}
- It is a degree \(n\) polynomial
- Constant term = determinant
- \( \lambda^{n-1} \) coefficient involves the trace
- Roots of the polynomial are the eigenvalues

---

{{< home-link "Home" >}} | {{< section-index >}}