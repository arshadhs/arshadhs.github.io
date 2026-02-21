---
title: "Gradient Descent for Linear Regression"
date: 2026-02-21
draft: false
weight: 435
tags: ["AI", "ML", "Linear Regression", "Gradient Descent"]
categories: ["AI", "ML"]
---

# Gradient Descent for Linear Regression

**Iterative Method – Gradient Descent (batch/stochastic/mini-batch)**

Gradient descent is an iterative optimisation method used to minimise the regression cost function by repeatedly updating parameters in the direction that reduces error.

{{% hint info %}}
Key takeaway:
Gradient descent starts with initial parameter values and repeatedly updates them using the gradient until the cost stops decreasing.
{{% /hint %}}

---

## Cost Function

A common cost function for linear regression is the Sum of Squared Errors:

{{% colour %}}
{{< katex display=true >}}
J(\beta_1,\beta_0) = \sum_{i=1}^{n}(\beta_1 x_i + \beta_0 - y_i)^2
{{< /katex >}}
{{% /colour %}}

Equivalent forms often used:
Mean Squared Error:

{{% colour %}}
{{< katex display=true >}}
J(w,b) = \frac{1}{n}\sum_{i=1}^{n}(w x_i + b - y_i)^2
{{< /katex >}}
{{% /colour %}}

Sometimes \(\frac{1}{2n}\) is used to simplify derivatives.

---

## Gradients (Partial Derivatives)

The partial derivatives are:

{{% colour %}}
{{< katex display=true >}}
\frac{\partial J}{\partial \beta_1}
=
\frac{1}{n}\sum_{i=1}^{n}(\beta_1 x_i + \beta_0 - y_i)x_i
{{< /katex >}}
{{% /colour %}}

{{% colour %}}
{{< katex display=true >}}
\frac{\partial J}{\partial \beta_0}
=
\frac{1}{n}\sum_{i=1}^{n}(\beta_1 x_i + \beta_0 - y_i)
{{< /katex >}}
{{% /colour %}}

Gradient meaning:
It tells you how to change parameters to reduce the cost fastest.

---

## Update Equations

Starting from initial values, parameters are updated each iteration:

{{% colour %}}
{{< katex display=true >}}
\beta_1^{(i+1)} := \beta_1^{(i)} - \alpha \frac{\partial J}{\partial \beta_1}
{{< /katex >}}
{{% /colour %}}

{{% colour %}}
{{< katex display=true >}}
\beta_0^{(i+1)} := \beta_0^{(i)} - \alpha \frac{\partial J}{\partial \beta_0}
{{< /katex >}}
{{% /colour %}}

Where:
- \(\alpha\) is the learning rate.
- \(i\) is the iteration number.

---

## Matrix/Vector Form (Least Squares + Gradient Descent)

For the overdetermined system \(Ap=b\), SSE can be written as:

{{% colour %}}
{{< katex display=true >}}
SSE = (Ap-b)^T(Ap-b)
{{< /katex >}}
{{% /colour %}}

Its gradient:

{{% colour %}}
{{< katex display=true >}}
\nabla_p(SSE) = 2A^TAp - 2A^Tb
{{< /katex >}}
{{% /colour %}}

Gradient descent update:

{{% colour %}}
{{< katex display=true >}}
p^{(i+1)} = p^{(i)} - \alpha\left(2A^TAp^{(i)} - 2A^Tb\right)
{{< /katex >}}
{{% /colour %}}

---

## Choosing the Learning Rate \(\alpha\)

If \(\alpha\) is too large:
You can overshoot the minimum.
The cost may oscillate or diverge.

If \(\alpha\) is too small:
Training is very slow.
You may need many iterations.

Practical habit:
Monitor the cost \(J\) across iterations and ensure it decreases steadily.

---

## References

- {{< relref "03-linear-models-regression.md" >}}
- {{< relref "03-ordinary-least-squares.md" >}}

---

{{< home-link "Home" >}} | {{< section-index >}}

---