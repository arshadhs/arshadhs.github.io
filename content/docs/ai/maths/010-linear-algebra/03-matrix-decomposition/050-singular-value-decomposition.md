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

For any matrix in real space:
{{< colour "green" >}}
{{< katex display=true >}}
A \in \mathbb{R}^{m \times n}
{{< /katex >}}
{{< /colour >}}

Its Singular Value Decomposition is:

{{< colour "green" >}}
{{< katex display=true >}}
A = U \Sigma V^T
{{< /katex >}}
{{< /colour >}}

Where:

- U is an orthogonal matrix of size m × m called the matrix of left singular vectors
- V is an orthogonal matrix of size n × n called the matrix of right singular vectors
- Σ is an m × n diagonal matrix containing singular values

---

# Intuition

From lecture understanding:

- Any matrix transformation can be viewed as rotation, scaling, and rotation again
- SVD captures this as:
  1. Rotate the input space (V^T)
  2. Stretch or shrink along orthogonal directions (Σ)
  3. Rotate again into the output space (U)

---

# Singular Values

The singular values are non-negative numbers.

They are obtained from eigenvalues of:

{{< katex display=true >}}
A^T A
{{< /katex >}}

More precisely:

{{< katex display=true >}}
\sigma_i = \sqrt{\lambda_i(A^T A)}
{{< /katex >}}

Where:

- λ_i are eigenvalues of A^T A
- σ_i are singular values of A

This is one of the most important formulas in the topic.

---

# Key Connection to Eigenvalues

SVD connects directly with the previous lecture on eigenvalues and eigenvectors.

- Eigenvalues → square matrices only  
- SVD → works for ANY matrix  

Important relation:

- Eigenvalue decomposition applies mainly to square matrices
- SVD works for any matrix, including rectangular matrices
- Right singular vectors are eigenvectors of A^T A
- Left singular vectors are eigenvectors of A A^T

So SVD generalises the idea of matrix decomposition beyond square matrices.

---

# Geometric Interpretation

SVD gives a geometric interpretation of a matrix transformation.

Suppose a vector x is given.

Then the matrix A acts on x as follows:

1. V^T changes the coordinate system
2. Σ scales the coordinates along orthogonal axes
3. U maps the result into the final output orientation

This means SVD tells us:

- which directions matter most
- how much stretching happens in each direction
- which directions may be compressed nearly to zero

That is why SVD is so useful in machine learning and data compression.

---

# Structure of the Sigma Matrix

The matrix Σ contains singular values on the diagonal and zeros elsewhere.

For example, if A is 2 × 2, then Σ may look like:

{{< katex display=true >}}
\Sigma =
\begin{bmatrix}
\sigma_1 & 0 \\
0 & \sigma_2
\end{bmatrix}
{{< /katex >}}

Usually the singular values are arranged in descending order:

{{< katex display=true >}}
\sigma_1 \geq \sigma_2 \geq \cdots \geq 0
{{< /katex >}}

This ordering is important in approximation and compression.

---

# Computing SVD Conceptually

A standard conceptual method is:

1. Compute A^T A
2. Find eigenvalues and eigenvectors of A^T A
3. Take square roots of eigenvalues to get singular values
4. Use eigenvectors of A^T A as right singular vectors
5. Compute left singular vectors using:

{{< katex display=true >}}
u_i = \frac{1}{\sigma_i} A v_i
{{< /katex >}}

Here:

- v_i is a right singular vector
- u_i is a left singular vector
- σ_i is the corresponding singular value

This formula is very important for exam questions.

---

# Rank and Singular Values

The rank of a matrix is directly connected to singular values.

Important fact:

- The number of non-zero singular values is equal to the rank of the matrix

So if a matrix has only two non-zero singular values, then its rank is two.

This gives a very clean way to understand matrix rank using SVD.

---

# Low-Rank Approximation

This is one of the most important applications of SVD.

If only the largest singular values are kept, we get an approximation of the original matrix:

{{< katex display=true >}}
A \approx U_k \Sigma_k V_k^T
{{< /katex >}}

This is called the rank-k approximation of A.

It keeps only the most important structure of the matrix.

This is useful because:

- small singular values often represent noise
- large singular values capture dominant patterns
- a lower-rank matrix is cheaper to store and compute with

---

# Why Low-Rank Approximation Matters

Low-rank approximation is used in many machine learning and data science tasks.

Examples:

- image compression
- dimensionality reduction
- recommendation systems
- latent semantic analysis
- denoising data

The main idea is:

- keep the important information
- discard the less important information

This is one of the reasons SVD is such a high-value topic.

---

# SVD and Orthogonality

The matrices U and V are orthogonal matrices.

That means:

{{< katex display=true >}}
U^T U = I
{{< /katex >}}

and

{{< katex display=true >}}
V^T V = I
{{< /katex >}}

This orthogonality is important because:

- it preserves lengths and angles during rotation
- it makes the decomposition numerically stable
- it connects SVD to orthonormal bases from earlier lectures

So SVD is not isolated.
It builds on orthogonality, eigenvalues, rank, and matrix transformations.

---

# Relation to Matrix Approximation

Lecture discussions emphasise that matrix decompositions are useful not only for exact factorisation but also for approximation.

SVD is especially important because it gives the best low-rank approximation in a least-squares sense.

That means if you want the “best” rank-k approximation of a matrix, truncating the SVD is the standard method.

This idea appears often in machine learning where data is high-dimensional and noisy.

---

# Typical Exam Interpretation

In exams, SVD questions are usually not just about writing the formula.

You may be asked to:

- compute singular values
- connect singular values to eigenvalues
- determine rank using singular values
- explain why SVD applies to rectangular matrices
- construct the decomposition using A^T A
- interpret the decomposition geometrically
- explain low-rank approximation

So preparation must include both computation and understanding.

---

# Common Exam Questions

Based on lecture flow and standard question patterns, common SVD questions include:

1. Find singular values of a matrix
2. Find right singular vectors using A^T A
3. Find left singular vectors using the formula for u_i
4. Write the full decomposition A = UΣV^T
5. Find the rank using singular values
6. Construct a low-rank approximation
7. Explain difference between eigen decomposition and SVD

---

# Hidden Exam Pattern

From the way the course builds the topic, SVD is rarely completely standalone.

It is usually mixed with earlier concepts such as:

- eigenvalues and eigenvectors
- orthogonality
- rank
- diagonalisation
- matrix approximation

So if SVD feels difficult, the real issue is often weakness in one of those prerequisite topics.

---

# Common Mistakes to Avoid

- Confusing eigenvalues with singular values
- Forgetting that singular values are square roots of eigenvalues of A^T A
- Ignoring the order of singular values
- Forgetting that SVD works for rectangular matrices
- Mixing up U and V
- Using the wrong formula when computing left singular vectors
- Assuming Σ must always be square

---

# Preparation Strategy

To prepare SVD properly:

1. revise eigenvalues and eigenvectors first
2. be comfortable with orthogonal matrices
3. practise computing A^T A
4. solve at least a few full SVD problems by hand
5. understand rank and low-rank approximation conceptually

If you do only the mechanical steps, you may still get stuck in exam questions that ask for explanation.

So combine formulas + concept + interpretation.

---

# Quick Summary Table

| Item | Meaning |
|---|---|
| A = UΣV^T | Singular Value Decomposition |
| U | Left singular vectors |
| V | Right singular vectors |
| Σ | Singular values on diagonal |
| σ_i = √λ_i(A^T A) | Relation between singular values and eigenvalues |
| Number of non-zero singular values | Rank of matrix |
| U_k Σ_k V_k^T | Rank-k approximation |

---

# Why SVD Matters in Machine Learning

SVD is important in machine learning because many datasets contain redundancy.

SVD helps us:

- identify dominant structure
- compress data
- remove noise
- extract latent features
- reduce dimensionality

This makes it a bridge topic between pure linear algebra and practical machine learning.

---

# References

- T1 Sections 4.5 and 4.6
- Lecture slides on Singular Value Decomposition
- Webinar and revision discussions on decomposition and related linear algebra ideas

- [My Notes: Eigenvalues]({{% relref "020-eigenvalues-and-eigenvectors.md" %}})
- [My Notes: Rank and Null Space]({{% relref "020-basis-and-rank.md" %}})

---

{{< home-link "Home" >}} | {{< section-index >}}

---
