---
title: 'Support Vector Machines'
draft: false
tags: ["AI", "ML"]
categories: ["AI", "ML"]
weight: 470
---
# Support Vector Machine (SVM)

SVM is a **supervised machine learning algorithm** used for:

- **Classification** (most common)
- **Regression** (called SVR)

> Find the decision boundary that separates classes with the **maximum margin**.

{{% hint %}}
A Support Vector Machine is a supervised learning algorithm that finds an optimal hyperplane by maximising the margin between classes, using support vectors and kernel functions to handle non-linear data.
{{% /hint %}}

---

## Intuition

Imagine two groups of points on a graph.

Many lines can separate them, but SVM asks:

> **Which line stays as far away as possible from both groups?**

That safest line is the **optimal hyperplane**.

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

{{< mermaid >}}
flowchart LR
    A1((●)) -->| | H[Hyperplane]
    A2((●)) -->| | H
    B1((○)) -->| | H
    B2((○)) -->| | H

    SV1[Support Vector] --- H
    SV2[Support Vector] --- H
{{< /mermaid >}}

---

## Hard Margin vs Soft Margin

### Hard-Margin SVM
- Perfect separation
- No misclassification allowed
- Works only if data is clean and separable

---

### Soft-Margin SVM
- Allows some misclassification
- Controlled by parameter **C**

| C value | Behaviour |
|-------|----------|
| Large C | Strict fit, low error, risk of overfitting |
| Small C | More tolerant, better generalisation |

---

## The Kernel Trick

### Problem
Some data is **not linearly separable**.

### Solution
Map data into a **higher-dimensional space** where it *is* separable.

**Kernels do this implicitly**, without computing the higher dimension.

### Common kernels
- Linear  
- Polynomial  
- **RBF (Radial Basis Function) Gaussian** - most popular  
- Sigmoid  

---

## SVM for Regression (SVR)

SVM can also predict **continuous values**.

- Fits a function inside an **ε-tube**
- Errors inside the tube are ignored
- Only points outside the margin matter

---

## When Should You Use SVM?

### Works well when
- Medium-sized datasets
- High-dimensional data
- Clear margins exist

### Not ideal when
- Very large datasets
- Very noisy data (without tuning)
- You need probabilistic outputs (by default)

---

## SVM vs Neural Networks

| Aspect | SVM | Neural Networks |
|-----|-----|----------------|
| Data size | Small–medium | Large |
| Training | Convex (global optimum) | Non-convex |
| Feature learning | ❌ No | ✅ Yes |
| Interpretability | Medium | Low |

---

**SVM draws the safest possible boundary between classes.**

---

## Linearly separable data  

---

## Non-linearly separable data 

---

## Kernel Trick (Mercer) 

---

## Applications to both structured and unstructured data

---

{{< home-link "Home" >}} | {{< section-index >}}
