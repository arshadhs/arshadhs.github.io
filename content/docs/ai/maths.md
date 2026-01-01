---
title: "Mathematical Foundation"
draft: false
tags: ["Machine Learning", "Mathematics", "Linear Algebra", "Calculus"]
categories: ["AI", "ML"]
weight: 1100
menu: main
---

# Mathematical Foundations for Machine Learning

Machine Learning is built on **mathematical principles** that allow models to represent data, learn patterns, and optimise performance.

ML needs **core mathematical tools** required to understand how ML algorithms work internally.

---

## Why Mathematics Matters in ML

- Machine learning models are **mathematical functions**
- Training a model means **optimising equations**
- Understanding maths helps:
  - Debug models
  - Improve performance
  - Choose the right algorithms

---

## Key Mathematical Areas

### 1. Linear Algebra
Used to represent and manipulate data.

- Vectors and matrices
- Matrix multiplication
- Eigenvalues and eigenvectors
- Vector spaces and projections

**Used in:**
- Neural networks
- Embeddings
- Principal Component Analysis (PCA)

---

### 2. Calculus
Used to **optimise models** during training.

- Derivatives and gradients
- Partial derivatives
- Chain rule
- Gradient descent

**Used in:**
- Backpropagation
- Loss minimisation
- Model training

---

### 3. Probability Theory
Used to model **uncertainty**.

- Random variables
- Probability distributions
- Expectation and variance
- Conditional probability

**Used in:**
- Classification
- Bayesian models
- Uncertainty estimation

---

### 4. Optimisation
Used to find **best model parameters**.

- Cost functions
- Convex vs non-convex optimisation
- Local vs global minima
- Regularisation techniques

**Used in:**
- Training deep neural networks
- Preventing overfitting

---

## Conceptual View

{{< mermaid >}}
flowchart LR
    DATA[Data]
    MATH[Math Models]
    OPT[Optimisation]
    MODEL[Trained Model]

    DATA --> MATH
    MATH --> OPT
    OPT --> MODEL
{{< /mermaid >}}

---

## Outcome

- Understand the maths behind ML algorithms
- Interpret optimisation behaviour
- Read and implement ML research papers more confidently

---

{{< home-link "Home" >}} | {{< section-index >}}