---
title: 'Linear models for Regression'
draft: false
tags: ["AI", "ML"]
categories: ["AI", "ML"]
weight: 300
---

# Linear Regression

Linear Regression is a supervised {{< colour "blue" >}}ML{{< /colour >}} method used to predict a **numerical** target by fitting a model that is **linear in its parameters**.

In {{< colour "blue" >}}ML{{< /colour >}}, linear models are a core baseline:
they’re fast, often surprisingly strong, and usually easy to interpret.

{{% hint info %}}
Key takeaway:
Linear Regression learns parameters by minimising a squared-error cost.
You can solve it directly (closed form) or iteratively (gradient descent),
and you can extend it using basis functions and regularisation.
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

{{% colour "blue" %}}
{{< katex display=true >}}
y = \beta_0 + \beta_1 x
{{< /katex >}}
{{% /colour %}}

**Multiple linear regression** (many predictors):

{{% colour "blue" %}}
{{< katex display=true >}}
y = \beta_0 + \beta_1 x_1 + \dots + \beta_n x_n
{{< /katex >}}
{{% /colour %}}

Matrix form (useful for implementation in {{< colour "blue" >}}ML{{< /colour >}}):

- Design matrix:
$X \in \mathbb{R}^{n \times (d+1)}$
(with a column of 1s for the intercept)
- Parameters:
$w \in \mathbb{R}^{d+1}$
- Targets:
$y \in \mathbb{R}^{n}$

{{% colour "blue" %}}
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

{{% colour "blue" %}}
{{< katex display=true >}}
\text{dist} = -3.0068(\text{Age}) + 576.6819
{{< /katex >}}
{{% /colour %}}

Interpretation:
For every 1 unit increase in age, the visibility distance decreases by approximately 3 units.

---

## Correlation

Correlation measures **linear** dependence only.
A low correlation does not mean “no relationship”:
It may mean the data is poorly explained by a linear model.

Also:
Being able to fit a line does not necessarily mean the model is good.

---
## Direct Solution Method

In regression we pick parameters to minimise prediction error.
A common choice is the **least squares method**.

- mathematical optimisation method used to find the best-fitting curve or line through a set of data points
- by minimising the sum of the squared residuals (differences) between observed and predicted values
- commonly used in regression analysis to determine the line of best fit 

For the full OLS derivation and closed-form solutions:
- {{< relref "03-ordinary-least-squares.md" >}}

---

## Iterative Method

### Gradient Descent (batch/stochastic/mini-batch)

When a direct solution is expensive (or you prefer iterative optimisation),
you can minimise the cost using gradient descent.

Full gradient descent notes (types, gradients, updates):
- {{< relref "03-gradient-descent-linear-regression.md" >}}

Cost function definition:
- {{< relref "03-cost-function.md" >}}


---

## Linear basis function models

A key idea in {{< colour "blue" >}}ML{{< /colour >}} is:
you can make linear regression more powerful by transforming inputs.

Instead of using $x$ directly, we use a feature mapping $\phi(x)$:

{{% colour "blue" %}}
{{< katex display=true >}}
\phi(x) = [\phi_0(x), \phi_1(x), \dots, \phi_M(x)]
{{< /katex >}}
{{% /colour %}}

Then the model becomes:

{{% colour "blue" %}}
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

{{% colour "blue" %}}
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

{{% colour "blue" %}}
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

