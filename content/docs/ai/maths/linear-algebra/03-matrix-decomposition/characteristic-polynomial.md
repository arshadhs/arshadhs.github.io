---
title: "Characteristic Polynomial"
draft: false
tags: ["Machine Learning", "Mathematics", "Linear Algebra"]
categories: ["AI", "ML"]
weight: 1312
---

# Characteristic Polynomial

Characteristic polynomial of a square matrix is a polynomial which is invariant under matrix similarity and has the eigenvalues as roots.

- an invariant is a property of a mathematical object (or a class of mathematical objects) which remains unchanged after operations or transformations of a certain type are applied to the objects.

The **characteristic polynomial** of a square matrix is the key tool used to compute **eigenvalues**.

---

Let  

{{< katex >}}A \in \mathbb{R}^{n \times n}{{< /katex >}}  
and  
{{< katex >}}\lambda \in \mathbb{R}{{< /katex >}}

The **characteristic polynomial** of \( A \) is defined as:

{{% hint danger %}}
{{< katex display=true >}}
p_A(\lambda)
=
\det(A - \lambda I)
{{< /katex >}}
{{% /hint %}}

This is a polynomial in \( \lambda \) of degree \( n \).

---

## General Form

We can show that:

{{% hint danger %}}
{{< katex display=true >}}
p_A(\lambda)
=
c_0
+
c_1 \lambda
+
\cdots
+
c_{n-1}\lambda^{n-1}
+
(-1)^n \lambda^n
{{< /katex >}}
{{% /hint %}}

Where:

{{< katex >}}c_0, c_1, \dots, c_{n-1} \in \mathbb{R}{{< /katex >}}

---

## Important Coefficients

Two coefficients are especially important.

### 1. Constant Term

If we set  

{{< katex >}}\lambda = 0{{< /katex >}}

then:

{{% hint danger %}}
{{< katex display=true >}}
p_A(0)
=
\det(A)
=
c_0
{{< /katex >}}
{{% /hint %}}

So:

{{% hint info %}}
The constant term of the characteristic polynomial is the determinant of the matrix.
{{% /hint %}}

---

### 2. Coefficient of \( \lambda^{n-1} \)

The coefficient of \( \lambda^{n-1} \) turns out to be:

{{% hint danger %}}
{{< katex display=true >}}
c_{n-1}
=
(-1)^{\,n-1}\operatorname{tr}(A)
{{< /katex >}}
{{% /hint %}}

Where:

{{< katex >}}\operatorname{tr}(A){{< /katex >}}  
is the **trace** of \( A \) (sum of diagonal elements).

{{% hint info %}}
So the trace of a matrix appears directly in its characteristic polynomial.
{{% /hint %}}

---

...
...

---

{{< home-link "Home" >}} | {{< section-index >}}
