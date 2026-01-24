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

Machine Learning is built on **mathematical principles** that allow models to:
- represent data
- learn patterns
- optimise performance

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

ML requires **core mathematical tools** to understand how ML algorithms work internally. Algebra deals with relationships between variables and quantities, while Calculus focuses on change and optimization.

---

## Core Mathematical Pillars in Machine Learning

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

{{% details title="Key Mathematical Areas"%}}

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

{{% /details %}}

---

## Optimisation (Cross-Cutting Concept)

Optimisation is the **process of finding the best model parameters** by **minimising** or **maximising** an objective function.

- a mechanism that relies on multiple mathematical areas.

**Depends on:**
- **Linear Algebra** → parameter updates  
- **Calculus** → gradient computation  
- **Probability & Statistics** → loss functions  
- **Graphs** → convergence analysis  

**Used in:**
- Gradient descent and its variants
- Training deep neural networks
- Regularisation and generalisation

---

## Refrences

[MML-Book](https://mml-book.github.io/book/mml-book.pdf)

[Calculus & Algebra](https://medium.com/@rajat01221/calculus-and-algebra-for-data-science-beginner-to-advanced-062018c7f8ad)

[Essence of linear algebra](https://www.youtube.com/playlist?list=PLZHQObOWTQDPD3MizzM2xVFitgF8hE_ab)

---

{{< home-link "Home" >}} | {{< section-index >}}

