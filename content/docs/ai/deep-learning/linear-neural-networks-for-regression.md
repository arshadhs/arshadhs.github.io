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
**Linear regression as a single-neuron neural network** with an **identity activation**.  
It is the cleanest “first example” of the full ML pipeline: data → model → objective → learning algorithm → evaluation.
{{% /hint %}}

## General Form

{{% hint danger %}}
**Regression task**  
Given an input feature vector {{< katex >}}x \in \mathbb{R}^d{{< /katex >}}, predict a real-valued target {{< katex >}}y \in \mathbb{R}{{< /katex >}} by learning a function {{< katex >}}f:\mathbb{R}^d \to \mathbb{R}{{< /katex >}}.

**Linear regression form**

{{< katex display=true >}}
\hat{y} = w_0 + w_1x_1 + \cdots + w_dx_d
{{< /katex >}}

**Single-neuron (augmented vector) form**  
Let {{< katex >}}\tilde{x} = [1, x_1, \dots, x_d]^T{{< /katex >}} and {{< katex >}}w = [w_0, w_1, \dots, w_d]^T{{< /katex >}}.  
With identity activation {{< katex >}}f(z)=z{{< /katex >}}:

{{< katex display=true >}}
\hat{y} = f(w^T\tilde{x}) = w^T\tilde{x}
{{< /katex >}}

**Vectorised predictions**  
For {{< katex >}}N{{< /katex >}} examples, design matrix {{< katex >}}X \in \mathbb{R}^{N \times (d+1)}{{< /katex >}}, targets {{< katex >}}y \in \mathbb{R}^N{{< /katex >}}:

{{< katex display=true >}}
\hat{y} = Xw
{{< /katex >}}

**Squared error (MSE) objective**

{{< katex display=true >}}
J(w) = \frac{1}{N}\sum_{i=1}^{N}\big(\hat{y}^{(i)} - y^{(i)}\big)^2
{{< /katex >}}

Equivalently, with error vector {{< katex >}}e = \hat{y} - y{{< /katex >}}:

{{< katex display=true >}}
J(w) = \frac{1}{N} e^T e
{{< /katex >}}
{{% /hint %}}

{{% hint danger %}}
**Gradient for MSE (vector form)**

{{< katex display=true >}}
\nabla J(w) = \frac{2}{N}X^T(\hat{y}-y) = \frac{2}{N}X^T e
{{< /katex >}}

**Gradient descent update**

{{< katex display=true >}}
w^{(t+1)} = w^{(t)} - \eta \, \nabla J\big(w^{(t)}\big)
{{< /katex >}}

where {{< katex >}}\eta > 0{{< /katex >}} is the learning rate.
{{% /hint %}}

## Properties

### Identity activation is the right match for regression
- Output range is {{< katex >}}(-\infty, \infty){{< /katex >}}, so the model can predict any real number.
- It is differentiable everywhere, and its derivative is constant.

{{% hint danger %}}
Identity activation:

{{< katex display=true >}}
f(z)=z,\qquad f'(z)=1
{{< /katex >}}
{{% /hint %}}

{{% hint info %}}
Because {{< katex >}}f'(z)=1{{< /katex >}}, the gradient signal passes through this neuron unchanged.  
This is one reason linear regression is a great “first training example”.
{{% /hint %}}

### Squared error gives a well-behaved objective for a linear model
- Differentiable and smooth.
- Convex for linear models (so there is a single global optimum for {{< katex >}}J(w){{< /katex >}}).
- Penalises large errors more strongly (quadratic growth).

### Vectorisation simplifies implementation
- Writing {{< katex >}}\hat{y}=Xw{{< /katex >}} makes gradients and updates compact and efficient.

## Intuition

### One neuron = a weighted “mixing” of features
Each weight {{< katex >}}w_j{{< /katex >}} tells the model how strongly feature {{< katex >}}x_j{{< /katex >}} influences the prediction.  
The bias {{< katex >}}w_0{{< /katex >}} shifts the output up or down.

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

1. **Forward**: {{< katex >}}\hat{y} = Xw{{< /katex >}}  
2. **Error**: {{< katex >}}e = \hat{y} - y{{< /katex >}}  
3. **Loss**: {{< katex >}}J(w) = \frac{1}{N} e^T e{{< /katex >}}  
4. **Gradient**: {{< katex >}}\nabla J(w) = \frac{2}{N}X^Te{{< /katex >}}  
5. **Update**: {{< katex >}}w \leftarrow w - \eta \nabla J(w){{< /katex >}}

## ML relevance

### This is the “template” for training deeper networks
A complete ML system has four components:
- **Data**: inputs {{< katex >}}X{{< /katex >}} and targets {{< katex >}}y{{< /katex >}}
- **Model**: the function that maps inputs to predictions
- **Objective**: a loss that measures error
- **Learning algorithm**: an optimiser (gradient descent here)

{{% hint info %}}
This same template extends directly to multi-layer networks:  
only the model {{< katex >}}f_\theta{{< /katex >}} becomes deeper, and backpropagation computes gradients efficiently.
{{% /hint %}}

### Training vs test error (generalisation)
You should always compare:
- **Training error**: how well you fit the data you trained on
- **Test/validation error**: how well you generalise to unseen data

If training error keeps decreasing but test error stops improving or worsens, you are likely overfitting.

### Common evaluation metric: R-squared (R^2)
{{< katex >}}R^2{{< /katex >}} (coefficient of determination) compares your model to a naive baseline (predicting the mean).

{{% hint danger %}}
Let {{< katex >}}y_i{{< /katex >}} be true targets, {{< katex >}}\hat{y}_i{{< /katex >}} predictions, and {{< katex >}}\bar{y}{{< /katex >}} the mean of {{< katex >}}y{{< /katex >}}.

Residual sum of squares:

{{< katex display=true >}}
SS_{\text{res}}=\sum_{i=1}^{N}(y_i-\hat{y}_i)^2
{{< /katex >}}

Total sum of squares:

{{< katex display=true >}}
SS_{\text{tot}}=\sum_{i=1}^{N}(y_i-\bar{y})^2
{{< /katex >}}

Coefficient of determination:

{{< katex display=true >}}
R^2 = 1 - \frac{SS_{\text{res}}}{SS_{\text{tot}}}
{{< /katex >}}
{{% /hint %}}

{{% hint info %}}
Interpretation (rule of thumb):
- {{< katex >}}R^2 \approx 1{{< /katex >}}: excellent fit
- {{< katex >}}R^2 \approx 0{{< /katex >}}: no better than predicting the mean
- {{< katex >}}R^2 < 0{{< /katex >}}: worse than predicting the mean
{{% /hint %}}

## Summary
- Linear regression can be viewed as a **single-neuron neural network** with **identity activation**.
- The training objective is typically **mean squared error**, which is smooth and convex for linear models.
- **Gradient descent** updates weights by moving downhill on the error surface.
- Evaluation should include **training vs test error**, and metrics such as **MSE** and {{< katex >}}R^2{{< /katex >}}.
- This “data → model → objective → optimiser” pipeline generalises to deep networks.

## Reference
- **DNN Module #3 — Linear Neural Networks for Regression**.
- Zhang, Lipton, Li, Smola — *Dive into Deep Learning* (sections on linear regression).
- Goodfellow, Bengio, Courville — *Deep Learning* (optimisation and regression basics).

- [Why Square the Error?](https://medium.com/@mail.alireza.a/why-square-the-error-in-machine-learning-153067c1ef64)

---

{{< home-link "Home" >}} | {{< section-index >}}