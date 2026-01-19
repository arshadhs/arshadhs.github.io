---
title: "Mathematical Foundation"
draft: false
tags: ["Machine Learning", "Mathematics", "Linear Algebra", "Probability", "Statistics", "Calculus"]
categories: ["AI", "ML"]
weight: 1100
menu: main
bookCollapseSection: true
---

# Mathematical Foundations for Machine Learning

Machine Learning is built on **mathematical principles** that allow models to represent data, learn patterns, and optimise performance.

ML requires **core mathematical tools** to understand how ML algorithms work internally.

---

## Core Mathematical Pillars in Machine Learning

The following **five mathematical areas** form the foundation of Machine Learning.

{{< mermaid >}}
flowchart TB
    ML[Maths for Machine Learning]

    LA[Linear Algebra]
    PR[Probability]
    ST[Statistics]
    CA[Calculus]
    GR[Graphs]

    ML --> LA
    ML --> PR
    ML --> ST
    ML --> CA
    ML --> GR

    %% Colours (your pastel scheme)
    style ML fill:#E1F5FE,stroke:#333
    style LA fill:#C8E6C9,stroke:#333
    style PR fill:#BBDEFB,stroke:#333
    style ST fill:#90CAF9,stroke:#333
    style CA fill:#64B5F6,stroke:#333
    style GR fill:#FFCCBC,stroke:#333
{{< /mermaid >}}

---

## Why Mathematics Matters in ML

- Machine learning models are **mathematical functions**
- Training a model means **optimising equations**
- Understanding maths helps to:
  - Debug models
  - Improve performance
  - Choose the right algorithms

---

## Key Mathematical Areas

{{< mermaid >}}
mindmap
  Mathematics for Machine Learning
    Linear Algebra
      Vectors
      Matrices
      Eigenvalues
      Matrix Multiplication

    Probability
      Random Variables
      Probability Distributions
      Conditional Probability
      Bayes Theorem

    Statistics
      Mean
      Variance
      Sampling
      Hypothesis Testing

    Calculus
      Derivatives
      Gradients
      Chain Rule
      Optimisation

    Graphs
      Line Plots
      Loss Curves
      Decision Boundaries
      Data Visualisation
{{< /mermaid >}}

---

### 1. Linear Algebra
Used to represent and manipulate data.

- Scalars, vectors, and matrices
- Matrix multiplication
- Eigenvalues and eigenvectors
- Vector spaces and projections

**Used in:**
- Neural networks
- Embeddings
- Principal Component Analysis (PCA)

---

### 2. Probability Theory
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

### 3. Statistics
Statistics enables **estimation, inference, and model evaluation** from data samples.

**Key Topics**
- Mean, variance, and standard deviation
- Sampling and estimation
- Hypothesis testing
- Bias–variance trade-off

**Used in:**
- Model evaluation
- Error analysis
- Experimental validation

---

### 4. Calculus
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

### 5. Graphs
Graphs are used to **visualise data and learning behaviour**.

**Key Topics**
- Line plots and scatter plots
- Loss curves
- Decision boundaries
- Convergence plots

**Used in:**
- Analysing training behaviour
- Debugging models
- Explaining results

---

## Optimisation (Cross-Cutting Concept)

Optimisation is the **process of finding the best model parameters** by minimising or maximising an objective function.

> Optimisation is **not an independent mathematical pillar**, but a mechanism that relies on multiple mathematical areas.

**Depends on:**
- **Calculus** → gradient computation  
- **Linear Algebra** → parameter updates  
- **Probability & Statistics** → loss functions  
- **Graphs** → convergence analysis  

**Used in:**
- Gradient descent and its variants
- Training deep neural networks
- Regularisation and generalisation

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

