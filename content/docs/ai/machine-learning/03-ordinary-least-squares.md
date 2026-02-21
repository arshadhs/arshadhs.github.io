---
title: "Ordinary Least Squares and the Line of Best Fit"
date: 2026-02-21
draft: false
weight: 432
tags: ["AI", "ML", "Linear Regression", "OLS"]
categories: ["AI", "ML"]
---

# Ordinary Least Squares and the Line of Best Fit

**Direct Solution Method**

Ordinary Least Squares (OLS) is the standard way to choose the “best” line in linear regression by minimising squared prediction errors.

{{% hint info %}}
Key takeaway:
OLS defines “best fit” as the line that minimises the total squared residual error across all data points.
{{% /hint %}}

---

## Predicted Value and Residual

For each data point \((x_i, y_i)\), the model predicts:

{{% colour %}}
{{< katex display=true >}}
\hat{y}_i = \beta_0 + \beta_1 x_i
{{< /katex >}}
{{% /colour %}}

The residual (error) is:

{{% colour %}}
{{< katex display=true >}}
r_i = y_i - \hat{y}_i
{{< /katex >}}
{{% /colour %}}

Residual intuition:
Positive residual means the point is above the line.
Negative residual means the point is below the line.

---

## Sum of Squared Errors (SSE)

OLS chooses \(\beta_0, \beta_1\) to minimise the Sum of Squared Errors:

{{% colour %}}
{{< katex display=true >}}
SSE = \sum_{i=1}^{n}(y_i - \hat{y}_i)^2
{{< /katex >}}
{{% /colour %}}

Why square the residuals:
- Prevents positive and negative errors cancelling out.
- Penalises large errors more strongly.
- Produces a smooth objective that is easier to optimise.

---

## Closed-Form Solution (Single Predictor)

When there is a single predictor variable, the solution can be written using covariance and variance:

{{% colour %}}
{{< katex display=true >}}
\beta_1 = \frac{\mathrm{cov}(x,y)}{\mathrm{var}(x)}
{{< /katex >}}
{{% /colour %}}

And the intercept:

{{% colour %}}
{{< katex display=true >}}
\beta_0 = \bar{y} - \beta_1 \bar{x}
{{< /katex >}}
{{% /colour %}}

Interpretation:
\(\beta_1\) is the slope (how much \(y\) changes per unit change in \(x\)).
\(\beta_0\) is the intercept (predicted value when \(x=0\)).

---

## Matrix View (General Linear Regression Framework)

Fitting a model can be expressed as an overdetermined system:

{{% colour %}}
{{< katex display=true >}}
Ap = b
{{< /katex >}}
{{% /colour %}}

Where:
- \(A\) is the design matrix.
- \(p\) is the parameter vector.
- \(b\) is the output vector.

The least-squares solution satisfies the normal equations:

{{% colour %}}
{{< katex display=true >}}
A^T A p = A^T b
{{< /katex >}}
{{% /colour %}}

And (when invertible):

{{% colour %}}
{{< katex display=true >}}
p^* = (A^T A)^{-1}A^T b
{{< /katex >}}
{{% /colour %}}

---

## Practical Meaning

OLS is:
- Fast and standard for many regression problems.
- Sensitive to outliers (because squaring makes large errors dominate).
- A foundation for extensions like regularisation (ridge, LASSO).

---

## References

- {{< relref "03-linear-models-regression.md" >}}
- {{< relref "03-gradient-descent-linear-regression.md" >}}

---

{{< home-link "Home" >}} | {{< section-index >}}

---