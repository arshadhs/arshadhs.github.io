---
title: 'MFML Lecture to Course Content Map'
draft: false
tags: ["MFML", "Maths", "Exam", "Course Map"]
categories: ["AI", "Maths"]
weight: 1
---

# MFML Lecture to Course Content Map

This file maps the uploaded Maths lecture PDFs and webinar PDFs against the official course handout/contact-session plan.
It is intended as an exam preparation index and as a source map for future Hugo Markdown notes.

## Course identity

- Course: **Mathematical Foundations for Machine Learning**
- Course code: **AIML ZC416**
- Main areas: linear algebra, vector spaces, matrix decompositions, vector calculus, optimisation, PCA, and SVM.

## Official module structure

| Module | Course handout area | Main ideas | Uploaded lecture coverage |
|---|---|---|---|
| M1 | Solution of linear systems | Systems of equations, matrices, solving Ax = b | Lecture 1, Webinar 1 |
| M2 | Vector spaces and analytic geometry | Vector spaces, linear independence, basis, rank, norms, inner products, angles, orthogonality, orthonormal basis | Lecture 2, Lecture 3, Webinar 1 |
| M3 | Matrix decomposition methods | Determinant, trace, eigenvalues, eigenvectors, Cholesky, eigendecomposition, diagonalisation, SVD, matrix approximation | Lecture 4, Lecture 5, Webinar 1, Webinar 2 |
| M4 | Vector calculus | Univariate differentiation, partial derivatives, gradients, matrix gradients, Taylor/Maclaurin series, Hessian, backpropagation, automatic differentiation | Lecture 6, Lecture 7, Lecture 8, Webinar 2 |
| M5 | Continuous optimisation | Gradient descent, constrained optimisation, Lagrange multipliers, convex optimisation | Lecture 9, Lecture 14, Webinar 2, Webinar 3, Webinar 4 |
| M6 | Nonlinear optimisation | Learning rate, initialisation, SGD, feature preprocessing, local optima, cliffs/valleys, momentum, AdaGrad, RMSProp, Adam | Lecture 10, Lecture 11, Webinar 3 |
| M7 | Dimensionality reduction, PCA, SVM | PCA perspectives, low-rank approximation, high-dimensional PCA, practical PCA, SVM preliminaries, primal/dual SVM, kernels | Lecture 12, Lecture 13, Lecture 14, Lecture 15, Webinar 4 |

## Contact session by lecture

| Session | Course handout topic | Uploaded file | What the lecture appears to cover | Exam relevance |
|---:|---|---|---|---|
| 1 | Solution of linear systems | `Lecture_1.pdf` | Linear algebra introduction, closure, systems of linear equations, matrix representation, solution types: no solution, unique solution, infinite solutions, pivot/free variables, matrix operations, inverse, transpose, compact Ax=b form | Very high for Mid-Sem and Comprehensive |
| 2 | Vector spaces, linear independence, basis, rank | `Lecture_2.pdf` | Groups, Abelian groups, vector spaces, vector subspaces, closure tests, linear combinations, span, linear independence, basis, rank, nullspace/column space ideas | Very high for Mid-Sem and Comprehensive |
| 3 | Analytic geometry | `Lecture_3.pdf` | Norms, dot product, inner products, bilinear mappings, symmetric positive-definite matrices, lengths, distances, angles, orthogonality, orthonormal basis, Gram-Schmidt ideas | Very high for Mid-Sem and Comprehensive |
| 4 | Matrix Decomposition I | `lecture_4.pdf` | Determinant, cofactor formula, determinant behaviour under row operations, rank-det relation, eigenvalues/eigenvectors, Cholesky-related positive definite ideas | Very high for Mid-Sem and Comprehensive |
| 5 | Matrix Decomposition II | `lecture_5.pdf` | Diagonal matrices, diagonalisation, eigendecomposition, spectral theorem for symmetric matrices, SVD, matrix approximation | Very high for Mid-Sem and Comprehensive |
| 6 | Vector Calculus I | `lecture_6.pdf` | Differentiation of univariate functions, polynomial derivatives, Taylor polynomial/series, partial derivatives, gradients, vector-valued gradients | Very high for Mid-Sem and Comprehensive |
| 7 | Vector Calculus II | `lecture_7_edited.pdf` | Matrix gradients, useful gradient identities, backpropagation, automatic differentiation, chain rule through neural-network layers | High for Mid-Sem and Comprehensive |
| 8 | Vector Calculus III | `lecture_8.pdf` | Taylor/Maclaurin series theory, remainder term, two-variable Taylor series, Hessian matrix, maxima/minima, unconstrained optimisation preliminaries | Very high for Mid-Sem and Comprehensive |
| 9 | Continuous Optimisation | `Lecture_9.pdf` | Gradient descent, negative gradient direction, local minima, step size, line search, convergence intuition, quadratic examples | Very high for Comprehensive; likely useful for quizzes/problems |
| 10 | Nonlinear Optimisation I | `Lecture_10.pdf` | Initialisation, objective functions in ML, overfitting, feature processing/preprocessing, SGD and practical optimisation behaviour | High for Comprehensive |
| 11 | Nonlinear Optimisation II | `Lecture_11.pdf` | Difficult topologies: cliffs, valleys, flat regions, curvature; momentum, AdaGrad, RMSProp, Adam | High for Comprehensive |
| 12 | PCA I | `Lecture_12.pdf` | Dimensionality reduction, PCA problem setting, centred data, covariance, maximum variance perspective, projection perspective | Very high for Comprehensive |
| 13 | PCA II | `Lecture_13.pdf` | Practical PCA, eigenvector computation, SVD relationship, low-rank approximation, high-dimensional PCA, key PCA steps | Very high for Comprehensive |
| 14 | Mathematical preliminaries for SVM | `Lecture 14.pdf` | Constrained optimisation, Lagrangian, quadratic programming, primal/dual, weak/strong duality, Slater condition, KKT conditions, kernels, linear classifiers | Very high for Comprehensive |
| 15 | Primal/dual linear SVM | `Lecture_15.pdf` | SVM primal problem, dual formulation, KKT conditions, support vectors, hinge loss, linear SVM numerical problem, hard/soft-margin direction | Very high for Comprehensive |
| 16 | Nonlinear SVM / kernels | Not clearly uploaded as a separate Lecture 16 PDF | Kernel functions, nonlinear SVM examples; likely partly covered in Lecture 14/15 and webinars | Very high for Comprehensive; gap to fill if Lecture 16 exists |

## Webinar mapping

| Webinar file | Main role | Best linked lectures | Exam use |
|---|---|---|---|
| `Webinar_1.pdf` | Problem sheet on linear systems, REF/RREF, column space, nullspace, row independence, subspaces, inner products, Cauchy-Schwarz, Cholesky, eigenvalues | Lectures 1-5 | Excellent for Mid-Sem problem practice |
| `Webinar_2.pdf` | Worked problems on maxima/minima, eigenvalues/spectral decomposition, gradient-related calculations and PCA-style examples | Lectures 4-9, 12-13 | Excellent for Mid-Sem revision and Comprehensive practice |
| `Webinar_3.pdf` | Gradient descent algorithm, step-size derivation for quadratic functions, worked gradient descent examples | Lectures 8-11 | Excellent for optimisation exam problems |
| `webinar_4.pdf` | Appears linked to optimisation/SVM/PCA practice based on uploaded set; use as problem-solving supplement after Lecture 12 onwards | Lectures 12-15 | Comprehensive exam practice |

## Mid-Sem focus

The course handout states that the **Mid-Semester Test** covers **Weeks 1-8**.
So for Mid-Sem, focus on:

1. Lecture 1: Linear systems and matrices
2. Lecture 2: Vector spaces, subspaces, linear independence, basis, rank
3. Lecture 3: Norms, inner products, orthogonality, Gram-Schmidt
4. Lecture 4: Determinant, trace, eigenvalues, eigenvectors, Cholesky
5. Lecture 5: Diagonalisation, eigendecomposition, SVD, matrix approximation
6. Lecture 6: Differentiation, partial derivatives, gradients, Taylor series
7. Lecture 7: Matrix gradients, gradient identities, backpropagation, automatic differentiation
8. Lecture 8: Taylor theorem, Hessian, maxima/minima, unconstrained optimisation
9. Webinar 1 and Webinar 2 for worked problem practice

## Comprehensive exam focus

The course handout states that the **Comprehensive Exam** covers all sessions 1 to 16.
So for Comprehensive, add:

1. Lecture 9: Gradient descent and step size
2. Lecture 10: Nonlinear optimisation, initialisation, feature preprocessing, overfitting, SGD
3. Lecture 11: Cliffs, valleys, momentum, AdaGrad, RMSProp, Adam
4. Lecture 12: PCA I
5. Lecture 13: PCA II
6. Lecture 14: SVM mathematical preliminaries, Lagrangian, KKT, duality
7. Lecture 15: Linear SVM, support vectors, hinge loss, primal/dual solution
8. Missing/gap area: nonlinear SVM and kernels if a separate Lecture 16 exists

## Suggested future Hugo Markdown pages

These are good page boundaries for your website and revision notes:

1. `01-linear-systems-and-matrices.md`
2. `02-vector-spaces-subspaces-basis-rank.md`
3. `03-analytic-geometry-norms-inner-products.md`
4. `04-determinants-trace-eigenvalues.md`
5. `05-eigendecomposition-svd-matrix-approximation.md`
6. `06-vector-calculus-gradients.md`
7. `07-backpropagation-automatic-differentiation.md`
8. `08-taylor-series-hessian-maxima-minima.md`
9. `09-gradient-descent-continuous-optimisation.md`
10. `10-nonlinear-optimisation-sgd-feature-preprocessing.md`
11. `11-momentum-adagrad-rmsprop-adam.md`
12. `12-pca-foundations.md`
13. `13-pca-practical-computation-svd.md`
14. `14-lagrangian-duality-kkt.md`
15. `15-support-vector-machines.md`
16. `16-nonlinear-svm-kernels.md`
17. `99-mfml-exam-formula-sheet.md`
18. `99-mfml-webinar-problem-bank.md`

## Important exam-preparation note

For exam preparation, do not only read the lecture slides.
Use this sequence:

1. Read the lecture slides for concept flow.
2. Create a one-page formula sheet for each lecture.
3. Work through the matching webinar problems.
4. Mark every problem by topic: definition, proof, calculation, interpretation, or algorithm.
5. For open-book comprehensive exam, prepare indexed printed notes rather than loose sheets.

