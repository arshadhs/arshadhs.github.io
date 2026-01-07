---
title: 'Supervised Learning'
featured_image: '/images/zebrato.jpeg'
date: 2026-01-03T10:29:52+01:00
draft: false
tags: ["AI", "ML"]
categories: ["AI", "ML"]
weight: 401
---

# Supervised Learning

Trained using **labelled data**.  
Each example in the training set includes the **correct output**.  
The algorithm learns to **generalise** and make predictions on unseen data.  
Generally more **accurate** than unsupervised methods.  
Requires **human intervention** for labelling and setup.  
Widely used due to its **accuracy and efficiency**.  
Produces **highly accurate results** when trained on good-quality labelled data.  

---
{{< mermaid >}}
mindmap
  root(Supervised Learning)
      Regression
        Linear Regression
        Multiple Linear Regression
        Polynomial Regression
        Logistic Regression
      Classification
        Binary Classification
        Multi Class Classification
        Multi Label Classification
{{< /mermaid >}}

---

## Classification

Output is **discrete** (e.g. Yes/No, Spam/Not Spam).  
Used for **categorising data** into predefined classes.  
Support Vector Machine (SVM) is a common classifier (a linear classifier with margin-based separation).  

### Common Types of Classification

- **Binary Classification** – Two possible outcomes (e.g. Spam / Not Spam).  
- **Multi-Class Classification** – More than two outcomes (e.g. Classifying animals as Cat, Dog, Bird).  
- **Multi-Label Classification** – One instance can belong to multiple categories (e.g. A movie tagged as Action and Comedy).  

---

## Regression

Output is a **continuous value** (e.g. price, temperature).  
Predicts **numeric outcomes** rather than categories.  

### Common Types of Regression

- **Linear Regression** – Straight-line relationship between input and output.  
- **Multiple Linear Regression** – Multiple features predict one output.  
- **Polynomial Regression** – Curved relationships.  
- **Logistic Regression** – Used for classification (predicting probabilities).  

---

{{< home-link "Home" >}} | {{< section-index >}}
