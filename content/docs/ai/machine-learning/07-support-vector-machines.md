---
title: "Support Vector Machine (SVM)"
draft: false
tags: ["AI", "ML", "SVM", "Classification"]
categories: ["AI", "ML"]
weight: 470
---

# Support Vector Machine (SVM)

A **Support Vector Machine (SVM)** is a **supervised machine learning algorithm** used for:

- **Classification** (most common)
- **Regression** (SVR – Support Vector Regression)

> Find the decision boundary that separates classes with the **maximum margin**.

{{% hint %}}
A Support Vector Machine is a supervised learning algorithm that finds an optimal hyperplane by maximising the margin between classes, using support vectors and kernel functions to handle non-linear data.
{{% /hint %}}

## Intuition

Imagine two groups of points on a graph.

Many lines can separate them, but SVM asks:

> **Which boundary stays as far away as possible from both groups?**

That safest boundary is the **optimal hyperplane**.

A larger margin usually leads to **better generalisation** on unseen data.

---

## Key Concepts

{{% steps %}}

1. ## Hyperplane
  - The **decision boundary**
  - In 2D → a line  
  - In 3D → a plane  
  - In higher dimensions → a hyperplane  

2.  ## Margin
  - Distance between the hyperplane and the nearest points
  - SVM **maximises** this margin

  Larger margin = better generalisation

3.  ## Support Vectors
  - The **closest points** to the hyperplane
  - They **define** the decision boundary
  - Removing them changes the model

{{% /steps %}}

---

## Linearly Separable Data

Data is **linearly separable** if a straight line (or hyperplane) can perfectly separate the classes.

- No misclassification
- Clean decision boundary
- Ideal case for SVM

This leads to **Hard-Margin SVM**.

---

## Hard-Margin vs Soft-Margin SVM

### Hard-Margin SVM
- Perfect separation
- No errors allowed
- Works only when data is clean and separable

---

### Soft-Margin SVM
- Allows some misclassification
- Controlled by parameter **C**

| C value | Behaviour |
|-------|----------|
| Large C | Strict fit, low error, risk of overfitting |
| Small C | More tolerant, better generalisation |

Soft-margin SVM is used **in practice**.

---

## Non-Linearly Separable Data

Many real-world datasets **cannot** be separated using a straight line.

Examples:
- Concentric circles
- XOR-like patterns
- Complex text or image data

In such cases, a linear boundary is insufficient.

---

## Kernel Trick (Mercer’s Idea)

### The Problem
Data is **not linearly separable** in the original feature space.

### The Solution
Map the data into a **higher-dimensional space** where it *becomes* separable.

### The Key Insight
This mapping can be done **implicitly** using a **kernel function**, without explicitly computing the higher dimension.

This idea is known as the **Kernel Trick**.

---

### Common Kernels

- **Linear** – when data is already separable
- **Polynomial** – captures interactions
- (**RBF** - Radial Basis Function Gaussian)[https://www.geeksforgeeks.org/machine-learning/radial-basis-function-kernel-machine-learning/] - most popular  
- **Sigmoid**

{{% hint info %}}
The kernel trick allows SVMs to create **non-linear decision boundaries** while still using a linear algorithm in a transformed space.
{{% /hint %}}

---

## SVM for Regression (SVR)

SVM can also predict **continuous values**.

- Fits a function inside an **ε-tube**
- Errors inside the tube are ignored
- Only points outside the margin influence the model

---

## Applications of SVM

SVMs are effective for both **structured** and **unstructured** data.

### Structured Data
- Tabular datasets
- Finance and risk scoring
- Bioinformatics

### Unstructured Data
- Text classification
- Image recognition
- Handwriting and OCR
- Spam detection

They work especially well in **high-dimensional spaces**.

---

## SVM vs Neural Networks

| Aspect | SVM | Neural Networks |
|-----|-----|----------------|
| Data size | Small–medium | Large |
| Training | Convex (global optimum) | Non-convex |
| Feature learning | ❌ No | ✅ Yes |
| Interpretability | Medium | Low |

---

## Intuition

**SVM draws the safest possible boundary between classes.**

---

{{< home-link "Home" >}} | {{< section-index >}}
