---
title: "Singular Value Decomposition (SVD)"
date: 2026-03-18
draft: false
weight: 1350
tags: ["Machine Learning", "Linear Algebra", "SVD"]
categories: ["AI", "ML"]
---

# Singular Value Decomposition (SVD)

Singular Value Decomposition (SVD) is one of the most important matrix decomposition techniques in linear algebra and machine learning.

It factorises any matrix into three simpler matrices that reveal its structure.

{{% hint info %}}
Key Idea:
SVD decomposes a matrix into rotations + scaling.
It tells us how data is transformed along orthogonal directions.
{{% /hint %}}

---

# Definition

For any matrix \( A \in \mathbb{R}^{m 	imes n} \):

{{< colour "green" >}}
\[
A = U \Sigma V^T
\]
{{< /colour >}}

Where:

- \( U \in \mathbb{R}^{m 	imes m} \): orthogonal matrix (left singular vectors)
- \( V \in \mathbb{R}^{n 	imes n} \): orthogonal matrix (right singular vectors)
- \( \Sigma \in \mathbb{R}^{m 	imes n} \): diagonal matrix (singular values)

---

# Intuition

From lecture understanding:

- Any matrix transformation = rotation → scaling → rotation
- SVD captures this as:
  1. Rotate (V^T)
  2. Scale (Σ)
  3. Rotate again (U)

---

# Singular Values

Singular values are:

{{< colour "green" >}}
\[
\sigma_i = \sqrt{\lambda_i(A^T A)}
\]
{{< /colour >}}

Where:
- \( \lambda_i \) are eigenvalues of \( A^T A \)

---

# Key Connection to Eigenvalues

- Eigenvalues → square matrices only  
- SVD → works for ANY matrix  

Important relation:

- Right singular vectors = eigenvectors of \( A^T A \)
- Left singular vectors = eigenvectors of \( A A^T \)

---

# Geometric Interpretation

- V: defines input directions  
- Σ: scales along those directions  
- U: rotates to final output  

---

# Example Structure

If:

{{< colour "green" >}}
\[
\Sigma =
\begin{bmatrix}
\sigma_1 & 0 \\
0 & \sigma_2
\end{bmatrix}
\]
{{< /colour >}}

Then:
- Data is stretched by \( \sigma_1 \), \( \sigma_2 \)

---

# Low-Rank Approximation (VERY IMPORTANT)

SVD allows approximation:

{{< colour "green" >}}
\[
A \approx U_k \Sigma_k V_k^T
\]
{{< /colour >}}

Keep only top k singular values.

Used for:
- Compression
- Noise reduction

---

# Connection to Rank

- Number of non-zero singular values = rank of matrix

---

# Steps to Compute SVD (Conceptual)

1. Compute \( A^T A \)
2. Find eigenvalues and eigenvectors
3. Compute singular values
4. Compute U using:

{{< colour "green" >}}
\[
u_i = \frac{1}{\sigma_i} A v_i
\]
{{< /colour >}}

---

# Applications in ML

- PCA (Principal Component Analysis)
- Image compression
- Recommendation systems
- Latent semantic analysis

---

# Common Exam Questions

1. Compute singular values  
2. Relation with eigenvalues  
3. Rank from SVD  
4. Low-rank approximation  
5. Conceptual explanation  

---

# Hidden Exam Pattern

From lectures:

- SVD is often linked with:
  - Eigenvalues
  - Rank
  - Orthogonality

👉 Questions are mixed (not standalone)

---

# Mistakes to Avoid

- Confusing eigenvalues with singular values  
- Forgetting square root relation  
- Not ordering singular values  

---

# Strategy to Prepare

1. Master eigenvalues first  
2. Understand orthogonality  
3. Practice 2–3 full SVD problems  
4. Focus on interpretation (not just steps)

---

# References

- T1 Sections 4.5, 4.6  
- Lecture slides  
- Webinar problems  

- [My Notes: Eigenvalues]({{% relref "020-eigenvalues-and-eigenvectors.md" %}})
- [My Notes: Rank and Null Space]({{% relref "020-basis-and-rank.md" %}})
---

{{< home-link "Home" >}} | {{< section-index >}}

---
