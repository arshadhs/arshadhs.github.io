---
title: "Gradient Descent for Linear Regression"
date: 2026-02-21
draft: false
weight: 360
tags: ["AI", "ML", "Linear Regression", "Gradient Descent"]
categories: ["AI", "ML"]
---

# Gradient Descent for Linear Regression

Gradient descent is an iterative optimisation method used to minimise the regression cost function by repeatedly updating parameters in the direction that reduces error.

- **Iterative method**
- Types: batch / stochastic / mini-batch

{{% hint info %}}
Key takeaway:
Gradient descent starts with initial parameter values and repeatedly updates them using the gradient until the cost stops decreasing.
{{% /hint %}}

{{< mermaid >}}
flowchart TD
GD["Gradient<br/>Descent"] -->|minimises| CF["Cost<br/>function"]
GD -->|updates| W["Parameters<br/>(weights)"]
GD -->|uses| GR["Gradient<br/>(slope)"]

GD --> H["Hyperparameters"]
H --> LR["Learning<br/>rate"]
H --> BS["Batch<br/>size"]
H --> EP["Epochs"]

style GD fill:#90CAF9,stroke:#1E88E5,color:#000

style CF fill:#CE93D8,stroke:#8E24AA,color:#000
style W fill:#CE93D8,stroke:#8E24AA,color:#000
style GR fill:#CE93D8,stroke:#8E24AA,color:#000
style H fill:#CE93D8,stroke:#8E24AA,color:#000
style LR fill:#CE93D8,stroke:#8E24AA,color:#000
style BS fill:#CE93D8,stroke:#8E24AA,color:#000
style EP fill:#CE93D8,stroke:#8E24AA,color:#000
{{< /mermaid >}}

---

## Types of GD

{{< mermaid >}}
flowchart TD
T["Gradient Descent<br/>types"] --> BGD["Batch<br/>GD"]
T --> SGD["Stochastic<br/>GD"]
T --> MGD["Mini-batch<br/>GD"]

BGD --> ALL["All data<br/>per step"]
BGD --> STB["Smooth<br/>updates"]

SGD --> ONE["1 sample<br/>per step"]
SGD --> FAST["Quick<br/>progress"]
SGD --> NOISE["Noisy<br/>updates"]

MGD --> MB["Small batch<br/>per step"]
MGD --> PRACT["Practical<br/>default"]

style T fill:#90CAF9,stroke:#1E88E5,color:#000

style BGD fill:#C8E6C9,stroke:#2E7D32,color:#000
style SGD fill:#C8E6C9,stroke:#2E7D32,color:#000
style MGD fill:#C8E6C9,stroke:#2E7D32,color:#000

style ALL fill:#CE93D8,stroke:#8E24AA,color:#000
style STB fill:#CE93D8,stroke:#8E24AA,color:#000
style ONE fill:#CE93D8,stroke:#8E24AA,color:#000
style FAST fill:#CE93D8,stroke:#8E24AA,color:#000
style NOISE fill:#CE93D8,stroke:#8E24AA,color:#000
style MB fill:#CE93D8,stroke:#8E24AA,color:#000
style PRACT fill:#CE93D8,stroke:#8E24AA,color:#000
{{< /mermaid >}}

### Batch

- Use only if you have huge compute and a lot of time to train

### SGD

- go-to solution
- takes random samples and updates parameters
- since randon → all updates are noisy
- does not converge perfectly to sharpest minima
- settles near to flat minima → better generalisation → helps prevent over fitting

- if learning rate is too high → can cause under fitting
- if learning rate is too high → can cause over fitting

### Mini-batch

- choose a batch size
- pick mini-batches from training data
- randomly take (32, 64, ...) points in every epoch across iterations

---

## Terminology

### Epochs

- hyperparameter
- one full pass of the entire training dataset through the learning algorithm
- dictates how many times the model updates its internal parameters by seeing every sample
- multiple epochs are generally needed for convergence
  - too few cause underfitting
  - too many can cause overfitting

### Batch

- a subset of the training data processed together during one training step
- used because the entire dataset is often too large to process at once

### Batch size

- hyperparameter that defines the number of training examples in one batch
- determines how much data is processed before the model updates its parameters

### Iterations

- a single training step where the model processes one batch and updates its parameters (weights and biases)

Each iteration includes:
- Forward pass: compute predictions and cost
- Backward pass: compute gradients and update model’s parameters to reduce the loss

---

## Gradients (Partial Derivatives)

We minimise the cost $J(w,b)$ defined here:
- {{< relref "03-cost-function.md" >}}

The partial derivatives are:

{{% colour "blue" %}}
{{< katex display=true >}}
\frac{\partial J}{\partial w}
=
\frac{1}{m}\sum_{i=0}^{m-1}\left(f_{w,b}\!\left(x^{(i)}\right)-y^{(i)}\right)x^{(i)}
{{< /katex >}}
{{% /colour %}}

{{% colour "blue" %}}
{{< katex display=true >}}
\frac{\partial J}{\partial b}
=
\frac{1}{m}\sum_{i=0}^{m-1}\left(f_{w,b}\!\left(x^{(i)}\right)-y^{(i)}\right)
{{< /katex >}}
{{% /colour %}}

Gradient meaning:
It tells you how to change parameters to reduce the cost fastest.

---

## Update Equations

Starting from initial values, parameters are updated each iteration:

{{% colour "blue" %}}
{{< katex display=true >}}
w^{(t+1)} := w^{(t)} - \alpha \frac{\partial J}{\partial w}
{{< /katex >}}
{{% /colour %}}

{{% colour "blue" %}}
{{< katex display=true >}}
b^{(t+1)} := b^{(t)} - \alpha \frac{\partial J}{\partial b}
{{< /katex >}}
{{% /colour %}}

Where:
- $\alpha$ is the learning rate
- $t$ is the iteration number

---

## Matrix/Vector Form (Least Squares + Gradient Descent)

For an overdetermined system $Ap=b$, the squared error can be written as:

{{% colour "blue" %}}
{{< katex display=true >}}
SSE = (Ap-b)^T(Ap-b)
{{< /katex >}}
{{% /colour %}}

Its gradient:

{{% colour "blue" %}}
{{< katex display=true >}}
\nabla_p(SSE) = 2A^TAp - 2A^Tb
{{< /katex >}}
{{% /colour %}}

Gradient descent update:

{{% colour "blue" %}}
{{< katex display=true >}}
p^{(t+1)} = p^{(t)} - \alpha\left(2A^TAp^{(t)} - 2A^Tb\right)
{{< /katex >}}
{{% /colour %}}

---

## Choosing the learning rate $\alpha$

If $\alpha$ is too large:
- you can overshoot the minimum
- the cost may oscillate or diverge

If $\alpha$ is too small:
- training is very slow
- you may need many iterations

Practical habit:
monitor the cost $J$ across iterations and ensure it decreases steadily.

---

## References

- [Also see DNN - GD](/docs/ai/deep-learning/gradient-descent-algorithm.md)
- {{< relref "03-linear-models-regression.md" >}}
- {{< relref "03-ordinary-least-squares.md" >}}
- {{< relref "03-cost-function.md" >}}

---

{{< home-link "Home" >}} | {{< section-index >}}

---
