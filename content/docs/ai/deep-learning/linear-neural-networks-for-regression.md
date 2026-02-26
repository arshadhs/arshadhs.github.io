---
title: "LNN for Regression"
date: 2026-02-15
draft: false
weight: 300
tags: ["Deep Learning", "Regression", "Optimisation"]
categories: ["AI", "Machine Learning"]
---

# Linear Neural Networks for Regression

A **linear neural network for regression** is a model that predicts a **continuous** target by taking a weighted sum of input features and applying the **identity activation** (so the output can be any real number).

- Single neuron for regression (predicting *how much* / *how many*)
- Data + linear model (single neuron, no hidden layers) + squared loss
- Training using **batch gradient descent** algorithm
- Prediction (inference)
- Eg: Auto MPG (UCI) style prediction with a single neuron (from-scratch code)

## Regression

Regression is a supervised learning task that predicts a continuous-valued output based on input features.

Typical use-cases:
- house prices prediction
- temperature forecasting
- stock price prediction
- other continuous predictions

## Linear Regression

Linear regression assumes the output is a linear combination of the input features.

{{% hint info %}}
**Linear regression as a single-neuron neural network** with an **identity activation**.  
This is the cleanest “first example” of the full ML pipeline: data → model → loss → optimisation → evaluation.
{{% /hint %}}

## ML Pipeline

{{< mermaid >}}
flowchart LR
  D["Data<br/>X, y"] --> M["Model<br/>Single neuron"]
  M --> L["Loss<br/>MSE"]
  L --> O["Optimiser<br/>Batch Gradient Descent"]
  O --> P["Parameters<br/>w, b"]
  P --> I["Inference<br/>Predict y-hat for new x"]

  %% Pastel colour scheme
  style D fill:#E3F2FD,stroke:#1E88E5,stroke-width:1px
  style M fill:#E8F5E9,stroke:#43A047,stroke-width:1px
  style L fill:#FFF3E0,stroke:#FB8C00,stroke-width:1px
  style O fill:#F3E5F5,stroke:#8E24AA,stroke-width:1px
  style P fill:#FCE4EC,stroke:#D81B60,stroke-width:1px
  style I fill:#E0F7FA,stroke:#00838F,stroke-width:1px
{{< /mermaid >}}

## Mathematical Form

{{< colour "green" >}}
{{< katex display=true >}}
\hat{y} = w_0 + w_1x_1 + \cdots + w_dx_d
{{< /katex >}}
{{< /colour >}}

where:
- {{< katex >}}x = [x_1, x_2, \dots, x_d]^T{{< /katex >}} are the input features
- {{< katex >}}w = [w_1, w_2, \dots, w_d]^T{{< /katex >}} are the weights
- {{< katex >}}w_0{{< /katex >}} is the bias term

If you use an augmented feature vector {{< katex >}}\tilde{x} = [1, x_1, \dots, x_d]^T{{< /katex >}}, then the same model is:

{{< colour "green" >}}
{{< katex display=true >}}
\hat{y} = w^T\tilde{x}
{{< /katex >}}
{{< /colour >}}

## The Four Components

A complete ML system has four components:
- **Data**: inputs {{< katex >}}X{{< /katex >}} and targets {{< katex >}}y{{< /katex >}}
- **Model**: single neuron implementing a linear function (maps inputs to predictions)
- **Objective**: measures prediction error (loss)
- **Learning algorithm**: an optimiser (gradient descent to optimise weights)

{{% hint info %}}
This same template extends directly to multi-layer networks:  
only the model {{< katex >}}f_\theta{{< /katex >}} becomes deeper, and backpropagation computes gradients efficiently.
{{% /hint %}}

## Single neuron for regression

{{< mermaid >}}
flowchart LR
  X1["x1"] --> S["Weighted sum<br/>z = w^T x + b"]
  X2["x2"] --> S
  X3["x3"] --> S
  S --> A["Identity activation<br/>f(z)=z"]
  A --> Y["Output<br/>y-hat"]

  %% Pastel colour scheme
  style X1 fill:#E3F2FD,stroke:#1E88E5,stroke-width:1px
  style X2 fill:#E3F2FD,stroke:#1E88E5,stroke-width:1px
  style X3 fill:#E3F2FD,stroke:#1E88E5,stroke-width:1px
  style S  fill:#FFF3E0,stroke:#FB8C00,stroke-width:1px
  style A  fill:#E8F5E9,stroke:#43A047,stroke-width:1px
  style Y  fill:#FCE4EC,stroke:#D81B60,stroke-width:1px
{{< /mermaid >}}

## Linear Model Prediction

Model (inference): a single neuron computes a weighted sum and applies the identity activation.

Given a new input {{< katex >}}x{{< /katex >}}:
- compute the score (weighted sum + bias)
- output that score directly as the prediction

## Identity Activation Function

The identity activation is:

{{< colour "green" >}}
{{< katex display=true >}}
f(z)=z
{{< /katex >}}
{{< /colour >}}

Key properties:
- Output is continuous in {{< katex >}}(-\infty, \infty){{< /katex >}}
- Differentiable everywhere with derivative {{< katex >}}f'(z)=1{{< /katex >}}

Why identity for regression?
- targets (prices, MPG, temperature) are continuous
- we need outputs that can be any real number, not just {{< katex >}}\{0,1\}{{< /katex >}}
- derivative {{< katex >}}f'(z)=1{{< /katex >}} keeps gradient flow simple for this first model

## Objective Function: Squared Error Loss

Measure how well our model fits the data.

A common choice is mean squared error (MSE):

{{< colour "green" >}}
{{< katex display=true >}}
J(w)=\frac{1}{N}\sum_{i=1}^{N}\left(\hat{y}^{(i)}-y^{(i)}\right)^2
{{< /katex >}}
{{< /colour >}}

Equivalently, using the error vector {{< katex >}}e=\hat{y}-y{{< /katex >}}:

{{< colour "green" >}}
{{< katex display=true >}}
J(w)=\frac{1}{N}e^Te
{{< /katex >}}
{{< /colour >}}

### Why Squared Error?

Advantages of squared error loss:
- differentiable and smooth (easy to optimise)
- convex for linear models (single global minimum)
- penalises large errors more strongly (quadratic growth)
- statistical interpretation: maximum likelihood under Gaussian noise

{{% hint info %}}
In practice, you still need to watch generalisation: low training loss does not guarantee low test loss.
{{% /hint %}}

---

## Gradient Descent Algorithm

Gradient descent updates weights in the direction of steepest descent (downhill on the loss surface).

Update rule:

{{< colour "green" >}}
{{< katex display=true >}}
w \leftarrow w - \eta \,\nabla J(w)
{{< /katex >}}
{{< /colour >}}

where:
- {{< katex >}}\eta{{< /katex >}} is the learning rate
- {{< katex >}}\nabla J(w){{< /katex >}} is the gradient of the loss with respect to the weights

### Computing GDA

In a neural network, the gradient tells you:
- which direction increases the loss the fastest
- so you update in the opposite direction to reduce the loss

Both Batch Gradient Descent and Stochastic Gradient Descent are useful optimisation algorithms that serve different purposes depending on the problem.

### Variations of GDA

Choosing between the algorithms depends on dataset size, compute, and the noise/stability you want during training.

#### Batch GDA
Uses the entire dataset to calculate the gradient and update parameters in one go.  
More stable, but slower and more computationally expensive.

#### Stochastic GDA (SGD)
Updates parameters using one training sample at a time.  
Faster and can escape shallow traps, but the updates are noisy.

#### Mini-batch Gradient Descent
The most common middle ground, using small random subsets (e.g., 32–512 samples).  
Balances speed (GPU-friendly) with stability.

### Which to choose?
- Small/medium data: batch GD can be fine.
- Deep learning / large data: mini-batch is the standard approach.
- Online learning / streaming: SGD is useful.

---

## Computing Gradient

This is the forward → loss → gradient → update pipeline:

1. **Forward**: {{< katex >}}\hat{y} = Xw{{< /katex >}}  
2. **Error**: {{< katex >}}e = \hat{y} - y{{< /katex >}}  
3. **Loss**: {{< katex >}}J(w) = \frac{1}{N} e^T e{{< /katex >}}  
4. **Gradient**: {{< katex >}}\nabla J(w) = \frac{2}{N}X^Te{{< /katex >}}  
5. **Update**: {{< katex >}}w \leftarrow w - \eta \nabla J(w){{< /katex >}}

The vector-form gradient used above is:

{{< colour "green" >}}
{{< katex display=true >}}
\nabla J(w)=\frac{2}{N}X^T(Xw-y)
{{< /katex >}}
{{< /colour >}}

{{% hint info %}}
Practical tip:
feature scaling (e.g., standardisation) often makes gradient descent converge faster and more reliably.
{{% /hint %}}

---

## Evaluation (quick)

Common regression checks:
- compare **training** vs **validation/test** error
- monitor MSE (or RMSE)
- optionally report {{< katex >}}R^2{{< /katex >}} (coefficient of determination)

Definition of {{< katex >}}R^2{{< /katex >}}:

{{< colour "green" >}}
{{< katex display=true >}}
R^2 = 1 - \frac{\sum_{i=1}^{N}(y_i-\hat{y}_i)^2}{\sum_{i=1}^{N}(y_i-\bar{y})^2}
{{< /katex >}}
{{< /colour >}}

where {{< katex >}}\bar{y}{{< /katex >}} is the mean of the true targets.

---

## Example: Auto MPG-style single neuron

Below is a compact “from scratch” implementation of:
- standardisation (recommended for GD)
- batch gradient descent
- prediction and MSE
- optional {{< katex >}}R^2{{< /katex >}}

```python
# Linear NN (single neuron) for regression from scratch
# - batch gradient descent
# - feature standardisation
# - MSE + optional R^2
#
# Replace X_raw, y with your dataset (e.g., Auto MPG features and MPG target).

import math
import random

def standardise_columns(X):
    # X: list of rows, each row is list of floats
    n = len(X)
    d = len(X[0])
    means = [0.0] * d
    stds  = [0.0] * d

    for j in range(d):
        means[j] = sum(X[i][j] for i in range(n)) / n
        var = sum((X[i][j] - means[j])**2 for i in range(n)) / n
        stds[j] = math.sqrt(var) if var > 1e-12 else 1.0

    Xs = []
    for i in range(n):
        Xs.append([(X[i][j] - means[j]) / stds[j] for j in range(d)])
    return Xs, means, stds

def add_bias_feature(X):
    # prepend 1.0 for bias as x0
    return [[1.0] + row for row in X]

def predict_row(w, x):
    # w and x are same length (including bias)
    return sum(wj * xj for wj, xj in zip(w, x))

def mse(w, X, y):
    n = len(X)
    err = 0.0
    for xi, yi in zip(X, y):
        yhat = predict_row(w, xi)
        err += (yhat - yi) ** 2
    return err / n

def r2_score(w, X, y):
    n = len(X)
    ybar = sum(y) / n
    ss_res = 0.0
    ss_tot = 0.0
    for xi, yi in zip(X, y):
        yhat = predict_row(w, xi)
        ss_res += (yi - yhat) ** 2
        ss_tot += (yi - ybar) ** 2
    return 1.0 - (ss_res / ss_tot) if ss_tot > 1e-12 else 0.0

def batch_gradient_descent(X, y, lr=0.05, epochs=500):
    n = len(X)
    d = len(X[0])
    w = [0.0] * d  # includes bias weight

    for _ in range(epochs):
        # gradient = (2/N) X^T (Xw - y)
        grad = [0.0] * d
        for xi, yi in zip(X, y):
            yhat = predict_row(w, xi)
            diff = (yhat - yi)
            for j in range(d):
                grad[j] += diff * xi[j]

        for j in range(d):
            grad[j] = (2.0 / n) * grad[j]
            w[j] -= lr * grad[j]
    return w

# --- Example usage (replace with real Auto MPG data) ---
# X_raw = [[...], [...], ...]  # features
# y     = [...]                # target

# Xs, means, stds = standardise_columns(X_raw)
# X  = add_bias_feature(Xs)
# w  = batch_gradient_descent(X, y, lr=0.05, epochs=1000)
# print("MSE:", mse(w, X, y))
# print("R^2:", r2_score(w, X, y))
```

## Summary

- Linear regression can be viewed as a single-neuron neural network with identity activation.

- The training objective is typically mean squared error, which is smooth and convex for linear models.

- Gradient descent updates weights by moving downhill on the error surface.

- Always compare training vs test/validation performance; you can report MSE and {{< katex >}}R^2{{< /katex >}}.

- This “data → model → objective → optimiser” pipeline generalises to deeper networks.

## Reference
- **DNN Module #3 — Linear Neural Networks for Regression**. (T1 – Ch 3, T1 - Ch 12)
- Zhang, Lipton, Li, Smola — *Dive into Deep Learning* (sections on linear regression).
- Goodfellow, Bengio, Courville — *Deep Learning* (optimisation and regression basics).

- [Why Square the Error?](https://medium.com/@mail.alireza.a/why-square-the-error-in-machine-learning-153067c1ef64)
- [Variations of GDA](https://www.geeksforgeeks.org/machine-learning/difference-between-batch-gradient-descent-and-stochastic-gradient-descent/)

---

{{< home-link "Home" >}} | {{< section-index >}}
