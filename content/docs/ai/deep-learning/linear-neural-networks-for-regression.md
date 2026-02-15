---
title: "Linear NN for Regression"
date: 2026-02-15
draft: false
weight: 345
tags: ["Deep Learning", "Regression", "Optimisation"]
categories: ["AI", "Machine Learning"]
---

# Linear Neural Networks for Regression

A **linear neural network for regression** is a model that predicts a **continuous** target by taking a weighted sum of input features and applying the **identity activation** (so the output can be any real number).

Typical use-cases: house prices, temperature forecasting, and other continuous predictions.

{{% hint info %}}
T**linear regression as a single-neuron neural network** with an **identity activation**.
It is the cleanest “first example” of the full ML pipeline: data → model → objective → learning algorithm → evaluation.
{{% /hint %}}

## General Form

{{% hint danger %}}
**Regression task**
Given an input feature vector \(x \in \mathbb{R}^d\), predict a real-valued target \(y \in \mathbb{R}\) by learning a function \(f:\mathbb{R}^d \to \mathbb{R}\).

**Linear regression form**
$$
\hat{y} = w_0 + w_1x_1 + \cdots + w_dx_d
$$

**Single-neuron (augmented vector) form**
Let \(\tilde{x} = [1, x_1, \dots, x_d]^T\) and \(w = [w_0, w_1, \dots, w_d]^T\).
With identity activation \(f(z)=z\):
$$
\hat{y} = f(w^T\tilde{x}) = w^T\tilde{x}
$$

**Vectorised predictions**
For \(N\) examples, design matrix \(X \in \mathbb{R}^{N \times (d+1)}\), targets \(y \in \mathbb{R}^N\):
$$
\hat{y} = Xw
$$

**Squared error (MSE) objective**
$$
J(w) = \frac{1}{N}\sum_{i=1}^{N}\big(\hat{y}^{(i)} - y^{(i)}\big)^2
$$
Equivalently, with error vector \(e = \hat{y} - y\):
$$
J(w) = \frac{1}{N} e^T e
$$
{{% /hint %}}

{{% hint danger %}}
**Gradient for MSE (vector form)**
$$
\nabla J(w) = \frac{2}{N}X^T(\hat{y}-y) = \frac{2}{N}X^T e
$$

**Gradient descent update**
$$
w^{(t+1)} = w^{(t)} - \eta \, \nabla J\big(w^{(t)}\big)
$$
where \(\eta > 0\) is the learning rate.
{{% /hint %}}

## Properties

### Identity activation is the right match for regression
- Output range is \((-\infty, \infty)\), so the model can predict any real number.
- It is differentiable everywhere, and its derivative is constant.

{{% hint danger %}}
Identity activation:
$$
f(z)=z,\qquad f'(z)=1
$$
{{% /hint %}}

{{% hint info %}}
Because \(f'(z)=1\), the gradient signal passes through this neuron unchanged.
This is one reason linear regression is a great “first training example”.
{{% /hint %}}

### Squared error gives a well-behaved objective for a linear model
- Differentiable and smooth.
- Convex for linear models (so there is a single global optimum for \(J(w)\)).
- Penalises large errors more strongly (quadratic growth).

### Vectorisation simplifies implementation
- Writing \(\hat{y}=Xw\) makes gradients and updates compact and efficient.

## Intuition

### One neuron = a weighted “mixing” of features
Each weight \(w_j\) tells the model how strongly feature \(x_j\) influences the prediction.
The bias \(w_0\) shifts the output up or down.

### Why gradient descent works here
You can imagine an “error surface” over weight space.
Gradient descent repeatedly:
- computes the slope (gradient),
- steps downhill (negative gradient direction),
- and moves closer to the minimum.

{{% hint info %}}
Each gradient step is perpendicular to the contours of the error surface, moving toward lower loss.
{{% /hint %}}

### Computational graph view (one gradient step)
This is the forward → loss → gradient → update pipeline:

1. **Forward**: \( \hat{y} = Xw \)  
2. **Error**: \( e = \hat{y} - y \)  
3. **Loss**: \( J(w) = \frac{1}{N} e^T e \)  
4. **Gradient**: \( \nabla J(w) = \frac{2}{N}X^Te \)  
5. **Update**: \( w \leftarrow w - \eta \nabla J(w) \)

## ML relevance

### This is the “template” for training deeper networks
A complete ML system has four components:
- **Data**: inputs \(X\) and targets \(y\)
- **Model**: the function that maps inputs to predictions
- **Objective**: a loss that measures error
- **Learning algorithm**: an optimiser (gradient descent here)

{{% hint info %}}
This same template extends directly to multi-layer networks:
only the model \(f_\theta\) becomes deeper, and backpropagation computes gradients efficiently.
{{% /hint %}}

### Training vs test error (generalisation)
You should always compare:
- **Training error**: how well you fit the data you trained on
- **Test/validation error**: how well you generalise to unseen data

If training error keeps decreasing but test error stops improving or worsens, you are likely overfitting.

### Common evaluation metric: \(R^2\)
\(R^2\) (coefficient of determination) compares your model to a naive baseline (predicting the mean).

{{% hint danger %}}
Let \(y_i\) be true targets, \(\hat{y}_i\) predictions, and \(\bar{y}\) the mean of \(y\).

Residual sum of squares:
$$
SS_{\text{res}}=\sum_{i=1}^{N}(y_i-\hat{y}_i)^2
$$

Total sum of squares:
$$
SS_{\text{tot}}=\sum_{i=1}^{N}(y_i-\bar{y})^2
$$

Coefficient of determination:
$$
R^2 = 1 - \frac{SS_{\text{res}}}{SS_{\text{tot}}}
$$
{{% /hint %}}

{{% hint info %}}
Interpretation (rule of thumb):
- \(R^2 \approx 1\): excellent fit
- \(R^2 \approx 0\): no better than predicting the mean
- \(R^2 < 0\): worse than predicting the mean
{{% /hint %}}

## Summary
- Linear regression can be viewed as a **single-neuron neural network** with **identity activation**.
- The training objective is typically **mean squared error**, which is smooth and convex for linear models.
- **Gradient descent** updates weights by moving downhill on the error surface.
- Evaluation should include **training vs test error**, and metrics such as **MSE** and **\(R^2\)**.
- This “data → model → objective → optimiser” pipeline generalises to deep networks.

## Reference
- Course deck: **DNN Module #3 — Linear Neural Networks for Regression**.
- Zhang, Lipton, Li, Smola — *Dive into Deep Learning* (sections on linear regression).
- Goodfellow, Bengio, Courville — *Deep Learning* (optimisation and regression basics).

- [Activation Functions]({{< relref "activation-functions" >}})
- [Loss Functions]({{< relref "loss-functions" >}})
- [Optimisation: SGD and Adam]({{< relref "optimisation-sgd-adam" >}})
- [Computation Graphs]({{< relref "computational-graphs" >}})

---

{{< home-link "Home" >}} | {{< section-index >}}

