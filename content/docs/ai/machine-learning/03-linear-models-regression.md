---
title: '03 - Linear models for Regression'
draft: false
tags: ["AI", "ML"]
categories: ["AI", "ML"]
weight: 430
---

# Linear models for Regression

Linear regression is a method used to predict a **numerical** output by fitting a model that is **linear in its parameters**.

In {{< color color="blue" >}}ML{{< /color >}}, linear models are a core baseline:
they’re fast, often surprisingly strong, and usually easy to interpret.

{{% hint info %}}
Key takeaway:
A “linear model” can still model **non-linear patterns** by using **basis functions**,
as long as the model remains linear in the parameters.
{{% /hint %}}

---

## Why Linear Regression

Common reasons to use linear regression:
- It can be built relatively easily and serves as a strong baseline.
- It is often more interpretable than “black-box” models.
- In practice, business/client constraints may require interpretability, even if that trades off some accuracy.

---

## Simple vs Multiple Linear Regression

**Simple linear regression** (one predictor variable):

{{% colour %}}
{{< katex display=true >}}
y = \beta_0 + \beta_1 x
{{< /katex >}}
{{% /colour %}}

**Multiple linear regression** (many predictors):

{{% colour %}}
{{< katex display=true >}}
y = \beta_0 + \beta_1 x_1 + \dots + \beta_n x_n
{{< /katex >}}
{{% /colour %}}

Matrix form (useful for implementation in {{< color color="blue" >}}ML{{< /color >}}):

- Design matrix:
$X \in \mathbb{R}^{n \times (d+1)}$
(with a column of 1s for the intercept)
- Parameters:
$w \in \mathbb{R}^{d+1}$
- Targets:
$y \in \mathbb{R}^{n}$

{{% colour %}}
{{< katex display=true >}}
\hat{y} = Xw
{{< /katex >}}
{{% /colour %}}

---

## Worked Example: Age vs Distance Visible

A sample of drivers was collected to study:
Question:
How strong is the linear relationship between **age** and the **distance a driver can see**?

- Predictor (x-axis):
Age (years)
- Response (y-axis):
Distance visible (ft)

A fitted “line of best fit” from the lecture is:

{{% colour %}}
{{< katex display=true >}}
\text{dist} = -3.0068(\text{Age}) + 576.6819
{{< /katex >}}
{{% /colour %}}

Interpretation:
For every 1 unit increase in age, the visibility distance decreases by approximately 3 units.

---

## Important Note About Correlation

Correlation measures **linear** dependence only.
A low correlation does not mean “no relationship”:
It may mean the data is poorly explained by a linear model.

Also:
Being able to fit a line does not necessarily mean the model is good.

---

## Direct Solution Method

In regression we pick parameters to minimise prediction error.
A common choice is **least squares**.

### Objective (least squares)

For each example $(x_i, y_i)$, the prediction is $\hat{y}_i$.
The residual is:
$r_i = y_i - \hat{y}_i$

Sum of squared errors (SSE):

{{% colour %}}
{{< katex display=true >}}
SSE(w) = \sum_{i=1}^{n}(y_i - \hat{y}_i)^2
{{< /katex >}}
{{% /colour %}}

A common scaled version is Mean Squared Error (MSE):

{{% colour %}}
{{< katex display=true >}}
J(w) = \frac{1}{n}\sum_{i=1}^{n}(Xw - y)_i^2
{{< /katex >}}
{{% /colour %}}

### Normal equation (closed-form)

If $X^T X$ is invertible, the least-squares solution is:

{{% colour %}}
{{< katex display=true >}}
w^* = (X^T X)^{-1}X^T y
{{< /katex >}}
{{% /colour %}}

Practical notes:
- If $X^T X$ is not invertible (e.g., redundant features), use a pseudoinverse or regularisation.
- Direct solutions can be fast for small/medium problems, but may be expensive for very large $d$.

---

## Iterative Method – Gradient Descent (batch/stochastic/mini-batch)

Instead of solving in one step, we can minimise $J(w)$ by iterative updates.

### Cost function (MSE style)

{{% colour %}}
{{< katex display=true >}}
J(w) = \frac{1}{n}\lVert Xw - y \rVert^2
{{< /katex >}}
{{% /colour %}}

Gradient (direction of steepest increase):

{{% colour %}}
{{< katex display=true >}}
\nabla J(w) = \frac{2}{n}X^T(Xw - y)
{{< /katex >}}
{{% /colour %}}

Update rule:

{{% colour %}}
{{< katex display=true >}}
w^{(t+1)} = w^{(t)} - \alpha \nabla J\left(w^{(t)}\right)
{{< /katex >}}
{{% /colour %}}

Where:
- $\alpha$:
learning rate
- $t$:
iteration number

### Batch gradient descent

Batch gradient descent:
uses all $n$ training examples to compute each update.
It is stable, but can be slow when $n$ is large.

### Stochastic gradient descent (SGD)

SGD:
uses one training example per update.
For a single example $(x_i, y_i)$:

{{% colour %}}
{{< katex display=true >}}
w \leftarrow w - \alpha \cdot 2x_i\big(x_i^T w - y_i\big)
{{< /katex >}}
{{% /colour %}}

SGD is:
- fast per update
- noisy (the loss may bounce around)
- often good for large-scale learning

### Mini-batch gradient descent

Mini-batch gradient descent:
uses a small batch $B$ (e.g., 32/64/128 examples) per update.
This is the most common in practical {{< color color="blue" >}}ML{{< /color >}} workflows.

Mini-batch gradient estimate:

{{% colour %}}
{{< katex display=true >}}
\nabla J(w) \approx \frac{2}{|B|}X_B^T(X_B w - y_B)
{{< /katex >}}
{{% /colour %}}

### What makes gradient descent work well

Feature scaling matters:
if features have very different scales, convergence can be slow.

Learning rate $\alpha$ matters:
- too large:
diverges or oscillates
- too small:
converges very slowly

Stopping criteria:
- max iterations
- cost improvement below a threshold
- validation error stops improving (early stopping)

---

## Linear basis function models

A key idea in {{< color color="blue" >}}ML{{< /color >}} is:
you can make linear regression more powerful by transforming inputs.

Instead of using $x$ directly, we use a feature mapping $\phi(x)$:

{{% colour %}}
{{< katex display=true >}}
\phi(x) = [\phi_0(x), \phi_1(x), \dots, \phi_M(x)]
{{< /katex >}}
{{% /colour %}}

Then the model becomes:

{{% colour %}}
{{< katex display=true >}}
y \approx \sum_{j=0}^{M} w_j \phi_j(x)
{{< /katex >}}
{{% /colour %}}

Important:
This can be non-linear in $x$,
but it is still linear in the parameters $w$.

### Common basis functions

Polynomial basis:
$\phi(x) = [1, x, x^2, \dots, x^M]$

Radial basis function (Gaussian / RBF):
centres $\mu_j$, width $\sigma$

{{% colour %}}
{{< katex display=true >}}
\phi_j(x) = \exp\left(-\frac{(x-\mu_j)^2}{2\sigma^2}\right)
{{< /katex >}}
{{% /colour %}}

Piecewise / spline-like basis:
useful when the relationship changes behaviour across different input ranges.

### Why this matters

More basis functions:
- usually reduces bias (more flexibility)
- can increase variance (risk of overfitting)

This leads directly to:
bias–variance decomposition.

---

## Bias-variance decomposition

For squared error, expected prediction error at input $x$ can be decomposed into:

{{% colour %}}
{{< katex display=true >}}
\mathbb{E}\left[(y-\hat{f}(x))^2\right]
=
\underbrace{\left(\text{Bias}[\hat{f}(x)]\right)^2}_{\text{systematic error}}
+
\underbrace{\text{Var}[\hat{f}(x)]}_{\text{sensitivity to training data}}
+
\underbrace{\sigma^2}_{\text{irreducible noise}}
{{< /katex >}}
{{% /colour %}}

Meaning:
- Bias:
error from overly simple assumptions (underfitting)
- Variance:
error from sensitivity to the particular training sample (overfitting)
- Noise:
error you cannot remove (measurement noise, missing variables)

### Underfitting vs overfitting

Underfitting:
high bias, low variance

Overfitting:
low bias, high variance

Typical pattern:
as model complexity increases (e.g., higher-degree polynomial basis),
bias tends to go down and variance tends to go up.

### Practical diagnosis

Compare:
training error vs test error

- high training error and high test error:
likely high bias (model too simple)
- low training error but high test error:
likely high variance (model too complex)

---

## Checklist

When you build a linear regression model, always check:
- Is a linear model shape sensible for this relationship?
- Do residuals show patterns (suggesting non-linearity)?
- Are there outliers strongly influencing the fit?
- Does the model generalise (train vs test performance)?
- Are features scaled (especially for gradient descent)?
- If you use basis functions:
are you controlling overfitting (validation, regularisation, early stopping)?

---

## References

- [STAT 501 (Penn State) lesson used in the example](https://online.stat.psu.edu/stat501/lesson/1/1.7)
- Tom Mitchell: Machine Learning (textbook reference for core concepts)

---

{{< home-link "Home" >}} | {{< section-index >}}