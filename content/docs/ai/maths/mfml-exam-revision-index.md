---
title: 'MFML Exam Revision Index'
draft: false
tags: ["MFML", "Maths", "Exam Revision"]
categories: ["AI", "Maths"]
weight: 2
---

# MFML Exam Revision Index

This is a practical revision index for the uploaded Mathematical Foundations for Machine Learning material.

## Exam split

| Exam | Coverage | Main files |
|---|---|---|
| Mid-Semester | Weeks/Sessions 1-8 | Lecture 1 to Lecture 8, Webinar 1, Webinar 2 |
| Comprehensive | Sessions 1-16 | Lecture 1 to Lecture 15, webinars, and any missing Lecture 16/kernel material |

## High-priority concept checklist

### Linear systems and matrices

- Convert equations into matrix form `Ax = b`
- Understand solution types: no solution, unique solution, infinite solutions
- Identify pivot and free variables
- Understand row operations, REF/RREF, rank, nullity
- Know matrix inverse and transpose properties

### Vector spaces

- Definition of vector space and subspace
- Closure under addition and scalar multiplication
- Span, linear combination, linear independence
- Basis, dimension, rank
- Column space, row space, nullspace

### Analytic geometry

- Norm properties
- Manhattan norm and Euclidean norm
- Inner product definition
- Symmetric positive-definite matrices
- Distance, angle, orthogonality
- Orthonormal basis and Gram-Schmidt

### Matrix decompositions

- Determinant and trace
- Cofactor expansion
- Row operation effect on determinant
- Eigenvalue equation `Av = λv`
- Characteristic equation `det(A - λI) = 0`
- Diagonalisation `A = PDP^{-1}`
- Spectral theorem for symmetric matrices
- Cholesky decomposition
- SVD `A = UΣV^T`
- Low-rank approximation

### Vector calculus

- Derivative from first principles
- Partial derivatives
- Gradient as direction of steepest ascent
- Gradient of vector-valued functions
- Matrix-gradient identities
- Chain rule
- Backpropagation and automatic differentiation

### Taylor series and Hessian

- Taylor polynomial
- Taylor series and Maclaurin series
- Remainder term
- Taylor series in two variables
- Hessian matrix
- First derivative and second derivative tests
- Maxima, minima and saddle points

### Gradient descent and optimisation

- Negative gradient direction
- Learning rate/step size
- Line search
- Convergence and local minima
- Constrained vs unconstrained optimisation
- Lagrange multipliers
- Convex optimisation
- SGD and optimisation in ML
- Feature preprocessing and scaling
- Overfitting in optimisation examples

### Nonlinear optimisation algorithms

- Difficult surfaces: cliffs, valleys, flat regions
- Curvature and why first-order methods can struggle
- Momentum update and intuition
- AdaGrad
- RMSProp
- Adam
- Learning rate decay

### PCA

- Dimensionality reduction problem
- Centred data and covariance matrix
- Maximum variance view
- Projection/reconstruction view
- Principal components as eigenvectors of covariance matrix
- SVD relation to PCA
- Low-rank approximation and Eckart-Young theorem
- PCA in high dimensions
- Practical PCA steps

### SVM

- Linear classifiers
- Margin and support vectors
- Hard-margin SVM primal formulation
- Lagrangian for SVM
- KKT conditions
- Primal vs dual perspective
- Role of inner products
- Kernel trick
- Hinge loss
- Soft-margin SVM

## Suggested revision order

### Phase 1: Foundations

1. Lecture 1
2. Lecture 2
3. Lecture 3
4. Webinar 1 problems related to REF, nullspace, column space and subspaces

### Phase 2: Matrix decompositions

1. Lecture 4
2. Lecture 5
3. Webinar 1 and Webinar 2 eigenvalue/eigendecomposition problems

### Phase 3: Calculus and optimisation foundations

1. Lecture 6
2. Lecture 7
3. Lecture 8
4. Webinar 2 maxima/minima and Hessian problems

### Phase 4: Optimisation for ML

1. Lecture 9
2. Lecture 10
3. Lecture 11
4. Webinar 3 gradient-descent step-size problems

### Phase 5: PCA and SVM

1. Lecture 12
2. Lecture 13
3. Lecture 14
4. Lecture 15
5. Webinar 4 / SVM problems

## What to ask me next

Use these prompts when generating Hugo pages:

```text
Generate a detailed Hugo Markdown page for Lecture X using the uploaded lecture PDF, course handout, and matching webinar problems. Use my preferred Hugo style, green colour shortcode for formulas, pastel Mermaid diagrams, and an exam-focus section.
```

```text
Create a formula sheet for Lecture X with definitions, formulas, common exam traps, and worked mini examples.
```

```text
Create an exam problem bank for Topic X using the webinar PDFs and lecture examples.
```

```text
Compare Lecture X with the course handout and tell me what is missing or needs extra study.
```

