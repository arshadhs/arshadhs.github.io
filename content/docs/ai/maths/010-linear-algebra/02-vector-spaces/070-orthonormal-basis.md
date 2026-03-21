---
title: "Orthonormal Basis"
draft: false
weight: 70
tags: ["Linear Algebra", "Vector Spaces"]
---

# Orthonormal Basis

A basis is **orthonormal** if its vectors are:
- orthogonal to each other
- each has unit length

{{< katex display=true >}}
\langle \mathbf{e}_i, \mathbf{e}_j \rangle =
\begin{cases}
1 & i=j \\
0 & i\ne j
\end{cases}
{{< /katex >}}

{{% hint info %}}
Key Idea:
Orthonormal basis = perfectly independent + perfectly scaled.
This makes computations extremely simple and stable.
{{% /hint %}}

Why it matters: orthonormal bases make projections and computations simple.

---

# Intuition (From Lectures)

From analytic geometry and vector space lectures:

- Basis gives coordinate system  
- Orthonormal basis gives **perfect coordinate system**  

Why?

- No overlap between directions (orthogonal)  
- No scaling distortion (unit length)  

---

# Properties of Orthonormal Basis

For vectors:

{{< katex display=true >}}
\{e_1, e_2, \dots, e_n\}
{{< /katex >}}

They satisfy:

### Orthogonality

{{< katex display=true >}}
\langle e_i, e_j \rangle = 0 \quad (i \ne j)
{{< /katex >}}

### Unit Norm

{{< katex display=true >}}
\|e_i\| = 1
{{< /katex >}}

---

# Matrix Form (Important)

If we form a matrix:

{{< katex display=true >}}
Q = [e_1 \; e_2 \; \cdots \; e_n]
{{< /katex >}}

Then:

{{< colour "yellow" >}}
{{< katex display=true >}}
Q^T Q = I
{{< /katex >}}
{{< /colour >}}

This means:

- columns are orthonormal  
- Q is an orthogonal matrix  

---

# Why Orthonormal Basis Matters

From lectures:

### 1. Easy Coordinates

Any vector can be written as:

{{< katex display=true >}}
x = \sum \langle x, e_i \rangle e_i
{{< /katex >}}

No solving system required.

---

### 2. Simple Projection

Projection onto basis vector:

{{< katex display=true >}}
\text{proj}_{e_i}(x) = \langle x, e_i \rangle e_i
{{< /katex >}}

---

### 3. Numerical Stability

- No redundancy  
- No scaling distortion  
- Stable computations  

---

# Connection to Gram-Schmidt (From Lecture)

From lecture discussions:

- We can convert any independent set into orthonormal basis  
- Using **Gram-Schmidt process**

Steps:

1. Start with independent vectors  
2. Make them orthogonal  
3. Normalize them  

---

# Geometric Interpretation

- Orthonormal basis = perpendicular axes  
- Like standard coordinate axes  

Example:

{{< katex display=true >}}
(1,0), (0,1)
{{< /katex >}}

These are orthonormal in 2D.

---

# Connection to Matrix Decomposition

From later lectures:

- Eigen decomposition uses orthogonal eigenvectors  
- SVD produces orthonormal matrices  

{{< katex display=true >}}
A = U \Sigma V^T
{{< /katex >}}

Where:

- U and V are orthonormal matrices  

---

# Orthonormal vs Normal Basis

| Type | Property |
|------|--------|
| Basis | Independent + spanning |
| Orthonormal basis | Independent + spanning + orthogonal + unit norm |

---

# Example

Given vectors:

{{< katex display=true >}}
v_1 = (1,0), \quad v_2 = (1,1)
{{< /katex >}}

These are not orthogonal.

After Gram-Schmidt:

{{< katex display=true >}}
e_1 = (1,0), \quad e_2 = (0,1)
{{< /katex >}}

Now orthonormal.

---

# Applications in Machine Learning

- PCA gives orthonormal directions  
- SVD produces orthonormal matrices  
- Feature decorrelation  
- Dimensionality reduction  

---

# Hidden Exam Pattern

From lecture flow:

- Orthonormal basis appears with:
  - orthogonality  
  - inner product  
  - projections  
  - Gram-Schmidt  

👉 rarely asked alone  

---

# Common Mistakes

- Forgetting normalization  
- Assuming orthogonal = orthonormal  
- Not checking unit length  
- Mixing up row and column orthogonality  

---

# Strategy to Prepare

1. Practice Gram-Schmidt  
2. Verify orthogonality using dot product  
3. Normalize vectors correctly  
4. Understand projection formula  

---

# Quick Summary Table

| Concept | Formula | Meaning |
|--------|--------|--------|
| Orthogonal | <e_i, e_j> = 0 | Independent directions |
| Unit norm | ||e_i|| = 1 | Standard scale |
| Orthonormal | both above | Perfect basis |
| Matrix form | Q^T Q = I | Orthogonal matrix |

---

# References

- Lecture slides (Analytic Geometry, Orthogonality)  
- Course handout  
- Webinar discussions  

- [My Notes: Angles and Orthogonality]({{% relref "angles-orthogonality.md" %}})
- [My Notes: Norm]({{% relref "norm.md" %}})

---

{{< home-link "Home" >}} | {{< section-index >}}

---
