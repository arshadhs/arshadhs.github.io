---
title: "Direct Solution - Ordinary Least Squares"
date: 2026-02-21
draft: false
weight: 320
tags: ["AI", "ML", "Linear Regression", "OLS"]
categories: ["AI", "ML"]
---

# Direct solution method - Ordinary Least Squares and the Line of Best Fit

It is possible to compute the best parameters for linear regression in one shot, using algebra, instead of iteratively improving them step-by-step.

For linear regression, the direct method is usually **Ordinary Least Squares (OLS)**.

Ordinary Least Squares (OLS) is the standard way to choose the “best” line in linear regression by minimising squared prediction errors.

{{% hint info %}}
Key takeaway:
OLS defines “best fit” as the line that minimises the total squared residual error across all data points.
{{% /hint %}}

---

## Predicted value and residual

For each data point $(x_i, y_i)$, the model predicts:

{{% colour "blue" %}}
{{< katex display=true >}}
\hat{y}_i = \beta_0 + \beta_1 x_i
{{< /katex >}}
{{% /colour %}}

The residual (error) is:

{{% colour "blue" %}}
{{< katex display=true >}}
r_i = y_i - \hat{y}_i
{{< /katex >}}
{{% /colour %}}

Residual intuition:
- Positive residual:
the point is above the line
- Negative residual:
the point is below the line

---

## Sum of squared errors (SSE)

Ordinary Least Squares chooses $\beta_0, \beta_1$ to minimise the sum of squared errors:

{{% colour "blue" %}}
{{< katex display=true >}}
SSE = \sum_{i=1}^{n}(y_i - \hat{y}_i)^2
{{< /katex >}}
{{% /colour %}}

Why square the residuals:
- Prevents positive and negative errors cancelling out
- Penalises large errors more strongly
- Produces a smooth objective that is easier to optimise

---

## What OLS is minimising

OLS chooses parameters to minimise the **sum of squared residuals**:

{{< colour "blue" >}}
{{< katex display=true >}}
SSE=\sum_{i=1}^{n}\left(y_i-\hat{y}_i\right)^2
{{< /katex >}}
{{< /colour >}}

This is why OLS is called “least squares”:
it penalises large errors more heavily than small ones.

---

## Multicollinearity (perfectly correlated features)

If two (or more) input features are perfectly correlated, the design matrix becomes rank-deficient:
$X^T X$ can become singular.

Practical result:
the OLS solution may not be unique (many parameter vectors fit equally well).

What you do in practice:
- remove/reduce redundant features
- or use regularisation (e.g. ridge)

---

## Closed-form solution (single predictor)

When there is a single predictor variable, the solution can be written using covariance and variance:

{{% colour "blue" %}}
{{< katex display=true >}}
\beta_1 = \frac{\mathrm{cov}(x,y)}{\mathrm{var}(x)}
{{< /katex >}}
{{% /colour %}}

And the intercept:

{{% colour "blue" %}}
{{< katex display=true >}}
\beta_0 = \bar{y} - \beta_1 \bar{x}
{{< /katex >}}
{{% /colour %}}

Interpretation:
- $\beta_1$ is the slope (how much $y$ changes per unit change in $x$)
- $\beta_0$ is the intercept (predicted value when $x=0$)

---

## Matrix view (general linear regression framework)

Fitting a model can be expressed as an overdetermined system:

{{% colour "blue" %}}
{{< katex display=true >}}
Ap = b
{{< /katex >}}
{{% /colour %}}

Where:
- $A$ is the design matrix
- $p$ is the parameter vector
- $b$ is the output vector

The least-squares solution satisfies the normal equations:

{{% colour "blue" %}}
{{< katex display=true >}}
A^T A p = A^T b
{{< /katex >}}
{{% /colour %}}

And (when invertible):

{{% colour "blue" %}}
{{< katex display=true >}}
p^* = (A^T A)^{-1}A^T b
{{< /katex >}}
{{% /colour %}}

---

## Summary

Ordinary Least Squares is:
- Fast and standard for many regression problems
- Sensitive to outliers (because squaring makes large errors dominate)
- A foundation for extensions like regularisation (ridge, LASSO)

---

## References

- {{< relref "03-linear-models-regression.md" >}}
- {{< relref "03-cost-function.md" >}}

---

{{< home-link "Home" >}} | {{< section-index >}}
