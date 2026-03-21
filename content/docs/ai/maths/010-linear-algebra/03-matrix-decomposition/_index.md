---
title: "Matrix Decompositions"
draft: false
tags: ["Machine Learning", "Mathematics", "Linear Algebra"]
categories: ["AI", "ML"]
weight: 1300
bookCollapseSection: true
---

# Matrix Decompositions

Decompositions reveal structure in matrices and power algorithms like PCA.

Matrix decompositions break complex matrices into simpler parts.

From the lecture introduction, matrices are used to describe mappings and transformations of vectors.

That is why decomposition is important:
it lets us understand a complicated transformation by rewriting it using simpler building blocks.

In the slides, the topic is introduced as part of three closely connected goals:
how to summarise matrices,
how matrices can be decomposed,
and how the decompositions can be used for matrix approximations.

In the webinar discussion, decomposition was explained using a practical intuition:
if a matrix represents something large or complicated, such as data or an image,
breaking it into simpler pieces helps us understand the structure more clearly.

{{% hint info %}}
Key Idea:
A good decomposition rewrites a difficult matrix problem into easier subproblems.
In this course, the main focus is on Eigen-decomposition, Diagonalization, Cholesky decomposition, and Singular Value Decomposition.
{{% /hint %}}

---

## Why Matrix Decompositions Matter

In linear algebra, a matrix may represent:
- a system of equations
- a linear transformation
- a covariance structure
- a data table
- a mapping from one space to another

A decomposition helps by separating these effects.

Instead of working with one complicated matrix directly,
we rewrite it into parts that are easier to interpret.

This is useful because:
- diagonal matrices are easier to analyse
- triangular matrices are easier to solve with
- orthogonal matrices preserve geometry
- decomposition often reveals rank, stability, and dominant structure

---

## Common Decompositions in This Course

The course handout and lecture material cover the following decomposition-related topics:

- Eigen-decomposition
- Diagonalization
- Cholesky decomposition
- Singular Value Decomposition (SVD)
- Matrix approximation using SVD

These are the decomposition ideas that are explicitly developed in the slides and transcript discussions.

---

{{< mermaid >}}
flowchart TD
    A["Matrix Decompositions"]

    A --> B["Covered in slides / transcript"]

    B --> B1["Eigen-decomposition"]
    B --> B2["Diagonalization"]
    B --> B3["Cholesky decomposition"]
    B --> B4["Singular Value Decomposition (SVD)"]
    B --> B5["Matrix approximation<br/>related topic"]

    classDef covered fill:#d9f2d9,stroke:#2e7d32,color:#000,stroke-width:1px;
    classDef root fill:#e8f5e9,stroke:#1b5e20,color:#000,stroke-width:1.5px;

    class A root;
    class B,B1,B2,B3,B4,B5 covered;
{{< /mermaid >}}

---

{{< mermaid >}}
flowchart TD
    A["Matrix Decompositions"]

    A --> C["Not covered in this module"]

    C --> C1["LU decomposition"]
    C --> C2["QR decomposition"]
    C --> C3["Schur decomposition"]
    C --> C4["Jordan form"]
    C --> C5["LDLᵀ decomposition"]

    classDef notcovered fill:#f3e5f5,stroke:#7b1fa2,color:#000,stroke-width:1px;
    classDef root fill:#e8f5e9,stroke:#1b5e20,color:#000,stroke-width:1.5px;

    class A root;
    class C,C1,C2,C3,C4,C5 notcovered;
{{< /mermaid >}}

---

## Big Picture Flow in the Course

The material builds decomposition ideas in a sequence.

First,
the course develops determinants, trace, eigenvalues, and eigenvectors.

Then,
it moves to diagonalization and eigendecomposition.

After that,
it introduces Singular Value Decomposition and shows how SVD extends decomposition ideas to rectangular matrices.

Finally,
it connects SVD to matrix approximation,
where a matrix is rewritten as a sum of simpler rank-1 pieces and then truncated to obtain a lower-rank approximation.

This progression is important for exams because the later topics depend on the earlier ones.

---

## Eigen-decomposition

Eigen-decomposition is based on eigenvalues and eigenvectors.

For a square matrix with enough eigenvectors,
we can write the matrix in the form:

{{< colour "green" >}}
{{< katex display=true >}}
A = P D P^{-1}
{{< /katex >}}
{{< /colour >}}

Here:
- P contains eigenvectors
- D is a diagonal matrix of eigenvalues

This is important because diagonal matrices are much easier to work with than general matrices.

From the lecture slides,
this factorization is presented as a way of obtaining a canonical form using eigenvalues and eigenvectors.

---

## Diagonalization

Diagonalization is a special and very useful matrix decomposition idea.

A matrix is diagonalizable if it can be transformed into a diagonal matrix using an invertible change of basis.

{{< colour "green" >}}
{{< katex display=true >}}
D = P^{-1} A P
{{< /katex >}}
{{< /colour >}}

Equivalently:

{{< colour "green" >}}
{{< katex display=true >}}
A = P D P^{-1}
{{< /katex >}}
{{< /colour >}}

The lecture explains that this is powerful because:
- matrix powers become easy
- inverse becomes easy when diagonal entries are non-zero
- structure becomes visible through eigenvalues

For symmetric matrices,
the spectral theorem gives an even stronger result:
the eigenvectors can be chosen orthonormally.

{{< colour "green" >}}
{{< katex display=true >}}
A = Q \Lambda Q^T
{{< /katex >}}
{{< /colour >}}

This is especially important in machine learning because symmetric matrices such as:

{{< colour "green" >}}
{{< katex display=true >}}
A^T A
{{< /katex >}}
{{< /colour >}}

and

{{< colour "green" >}}
{{< katex display=true >}}
A A^T
{{< /katex >}}
{{< /colour >}}

appear naturally in SVD and PCA.

---

## Cholesky Decomposition

Cholesky decomposition is a more specialised decomposition.

It applies to symmetric positive-definite matrices.

The lecture states the theorem in the form:

{{< colour "green" >}}
{{< katex display=true >}}
A = L L^T
{{< /katex >}}
{{< /colour >}}

where:
- A is symmetric positive-definite
- L is lower triangular
- the diagonal entries of L are positive

This decomposition is important because triangular matrices are easier to solve with.

In the webinar explanation,
extra emphasis was placed on the fact that Cholesky is not for all matrices.

It is a specific decomposition for a specific class of matrices.

That makes it both restrictive and powerful.

The webinar also discussed uniqueness:
if the diagonal entries of L are taken to be positive,
the Cholesky factor is unique.

A machine learning application mentioned in the slides is sampling from multivariate Gaussian distributions,
because covariance matrices are symmetric positive-definite and can be factored using Cholesky.

---

## Singular Value Decomposition (SVD)

SVD is described in the lecture as a central matrix decomposition method in linear algebra.

Unlike eigendecomposition,
SVD exists for all rectangular matrices.

This is one of the most important conceptual differences in the course.

The decomposition is written as:

{{< colour "green" >}}
{{< katex display=true >}}
A = U \Sigma V^T
{{< /katex >}}
{{< /colour >}}

where:
- U is orthogonal
- V is orthogonal
- \Sigma contains singular values

The slides explain that:
- left singular vectors are eigenvectors of {{< katex >}}A A^T{{< /katex >}}
- right singular vectors are eigenvectors of {{< katex >}}A^T A{{< /katex >}}
- non-zero singular values are square roots of the non-zero eigenvalues of {{< katex >}}A^T A{{< /katex >}}

{{< colour "green" >}}
{{< katex display=true >}}
\sigma_i = \sqrt{\lambda_i(A^T A)}
{{< /katex >}}
{{< /colour >}}

This makes SVD the natural extension of eigen-based ideas to general matrices.

---

## Matrix Approximation

After introducing SVD,
the course uses it for matrix approximation.

This is one of the machine learning motivations for studying decomposition.

The lecture explains that a rank-r matrix can be written as a sum of rank-1 matrices:

{{< colour "green" >}}
{{< katex display=true >}}
A = \sum_{i=1}^{r} \sigma_i u_i v_i^T
{{< /katex >}}
{{< /colour >}}

If we keep only the first k terms,
we get a rank-k approximation:

{{< colour "green" >}}
{{< katex display=true >}}
\hat{A}^{(k)} = \sum_{i=1}^{k} \sigma_i u_i v_i^T
{{< /katex >}}
{{< /colour >}}

This is important because:
- it reduces storage
- it reduces computation
- it captures dominant structure
- it connects directly to dimensionality reduction and PCA

This is one of the clearest ways decomposition becomes useful in machine learning rather than remaining only a pure linear algebra topic.

---

## Machine Learning Perspective

Matrix decompositions help:
- Reduce dimensionality
- Improve numerical stability
- Extract structure from data

They also help us:
- identify dominant directions in data
- work with covariance matrices
- compress matrices using lower-rank approximations
- connect algebraic structure to geometric interpretation

From the slides and course handout,
this is exactly why decomposition sits at the centre of PCA, SVD-based approximation, and several later ML topics.

---

## Covered vs Not Covered

It is useful to separate:
what this course module explicitly covers,
and what exists more broadly in linear algebra but is not part of the present lecture sequence.

### Covered in the current slides / transcript
- Eigen-decomposition
- Diagonalization
- Cholesky decomposition
- Singular Value Decomposition
- Matrix approximation

### Not developed in the current slides / transcript
- LU decomposition
- QR decomposition
- Schur decomposition
- Jordan form
- LDLᵀ decomposition

These can be useful in advanced numerical linear algebra,
but they are not the focus of the current module.

---

## Exam-Focused Reading of This Page

For exam preparation,
this introduction should help you remember the following hierarchy:

1. Eigenvalues and eigenvectors build the language
2. Diagonalization and eigendecomposition simplify square matrices
3. Cholesky handles symmetric positive-definite matrices
4. SVD handles all matrices
5. Matrix approximation is the applied payoff of SVD

If you keep this progression clear,
the later decomposition topics become much easier to organise in your mind.

---

{{< home-link "Home" >}} | {{< section-index >}}
