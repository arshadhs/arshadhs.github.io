---
title: "Ordinary Least Squares"
date: 2026-02-21
draft: false
weight: 320
tags: ["AI", "ML", "Linear Regression", "OLS"]
categories: ["AI", "ML"]
---

# Direct solution method - Ordinary Least Squares and the Line of Best Fit

It is possible to compute the best parameters for linear regression **in one shot** (closed-form),
instead of iteratively improving them step-by-step. fileciteturn34file10turn34file6

For linear regression, the direct method is usually **Ordinary Least Squares (OLS)**.

Ordinary Least Squares (OLS) chooses the “best” line by **minimising squared prediction errors**.

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

## OLS numerical (simple linear regression)

If the question gives a small table of $(x_i, y_i)$ and asks
“find the best-fit line / optimal $\theta_0,\theta_1$” (with **no** learning rate / iteration info),
use OLS.

### Step-by-step (what you write in the exam)

1) Compute means:

First, calculate the mean of x and the mean of y.

{{% colour "blue" %}}
{{< katex display=true >}}
\bar{x}=\frac{1}{n}\sum_{i=1}^{n}x_i,
\qquad
\bar{y}=\frac{1}{n}\sum_{i=1}^{n}y_i
{{< /katex >}}
{{% /colour %}}

2) Compute the two sums:

{{% colour "blue" %}}
{{< katex display=true >}}
S_{xx}=\sum_{i=1}^{n}(x_i-\bar{x})^2,
\qquad
S_{xy}=\sum_{i=1}^{n}(x_i-\bar{x})(y_i-\bar{y})
{{< /katex >}}
{{% /colour %}}

3) Slope and intercept:

{{% colour "blue" %}}
{{< katex display=true >}}
\beta_1=\frac{S_{xy}}{S_{xx}},
\qquad
\beta_0=\bar{y}-\beta_1\bar{x}
{{< /katex >}}
{{% /colour %}}

4) Final regression line:
$\hat{y}=\beta_0+\beta_1 x$.

---

### Step-by-step (OLS for simple linear regression) - alternate representation

1. **Identify the given data**
   - Independent variable: $X$ (Minutes Studied)
   - Dependent variable: $Y$ (Score)

2. **Calculate the means**
   - Compute the mean of $X$:
     $\,\bar{x}=\frac{1}{n}\sum_{i=1}^{n}x_i$
   - Compute the mean of $Y$:
     $\,\bar{y}=\frac{1}{n}\sum_{i=1}^{n}y_i$

3. **Determine the slope ($\theta_1$)**
   OLS finds the slope:

   {{% colour "blue" %}}
   {{< katex display=true >}}
   \theta_1=\frac{\sum_{i=1}^{n}(x_i-\bar{x})(y_i-\bar{y})}{\sum_{i=1}^{n}(x_i-\bar{x})^2}
   {{< /katex >}}
   {{% /colour %}}

   Compute the components:

   | $x_i$ | $y_i$ | $x_i-\bar{x}$ | $y_i-\bar{y}$ | $(x_i-\bar{x})(y_i-\bar{y})$ | $(x_i-\bar{x})^2$ |
   |---:|---:|---:|---:|---:|---:|
   | 10 | 60 | -15 | -11.25 | 168.75 | 225 |
   | 20 | 65 | -5  | -6.25  | 31.25  | 25  |
   | 30 | 75 | 5   | 3.75   | 18.75  | 25  |
   | 40 | 85 | 15  | 13.75  | 206.25 | 225 |
   | **Sum** |  |  |  | **425** | **500** |

   So:
   $\theta_1=\frac{425}{500}$

4. **Determine the intercept ($\theta_0$)**
   Using the calculated $\theta_1$:

   {{% colour "blue" %}}
   {{< katex display=true >}}
   \theta_0=\bar{y}-\theta_1\bar{x}
   {{< /katex >}}
   {{% /colour %}}

---

## Matrix view (general linear regression framework)

**Matrix calculation is very important for exam numericals**
can compute using either:
- direct matrix multiplication, or
- covariance/variance form (single-feature case).

### Design matrix (bias term)

For linear regression, we include a bias term by setting the first feature $x_0=1$ for every record. fileciteturn34file17

Example (one feature):
- parameters: $\theta = [\theta_0,\theta_1]^T$
- design matrix:
$X = \begin{bmatrix} 1 & x_1 \\ 1 & x_2 \\ \dots & \dots \\ 1 & x_n \end{bmatrix}$
- target vector:
$y = [y_1,\dots,y_n]^T$

### Normal equation (closed-form)

{{% colour "blue" %}}
{{< katex display=true >}}
\theta^* = (X^T X)^{-1}X^T y
{{< /katex >}}
{{% /colour %}}

Dimension check (quick sanity):
- $X$: $n\times d$
- $X^T X$: $d\times d$
- $(X^T X)^{-1}X^T y$: $d\times 1$ (same shape as $\theta$)

---

## Multicollinearity (perfectly correlated features)

If two (or more) input features are perfectly correlated, the design matrix becomes rank-deficient:
$X^T X$ can become singular.

Practical result:
the OLS solution may not be unique (many parameter vectors fit equally well). fileciteturn34file5turn34file0

What you do in practice:
- remove/reduce redundant features
- or use regularisation (e.g. ridge)

---

## OLS vs Gradient Descent (how to recognise which one to use)

Your lecturer explained:
- gradient descent is **iterative** and needs a learning rate $\alpha$,
- closed-form OLS is **non-iterative** and does **not** need $\alpha$,
- for large datasets / many features, closed-form becomes heavy because of matrix inversion,
  so gradient descent is preferred in practice. fileciteturn34file13turn34file6turn34file2

### Use OLS (direct / closed form) when the question includes:
- “find the **optimal** / best-fit $\theta_0,\theta_1$”
- “using **OLS** / **normal equation** / **matrix method**”
- “compute $\theta=(X^T X)^{-1}X^T y$”
- no learning rate $\alpha$ and no “iterations” wording fileciteturn34file9turn34file10turn34file13

### Use gradient descent (iterative) when the question includes:
- initial values (e.g. $\theta_0=0.1,\theta_1=0.5$)
- learning rate $\alpha$ (or $\eta$)
- “show only the first iteration / two iterations / update rule”
- “batch vs stochastic vs mini-batch” fileciteturn34file7turn34file6turn34file19

### What to watch out for in numerical problems (quick cues)

- If you see $\alpha$ and “one iteration” → it is **definitely gradient descent**.
- If you see $(X^T X)^{-1}$ or “normal equation” → it is **definitely OLS**.
- If it is a small table and they ask “optimal line” → use OLS unless told “use GD”.
- If features are large-scale (e.g. $x$ in thousands), GD updates may jump a lot → feature scaling helps. fileciteturn34file14turn34file15

---

## Summary

Ordinary Least Squares is:
- Fast and standard for many regression problems
- Sensitive to outliers (because squaring makes large errors dominate)
- A foundation for extensions like regularisation (ridge, LASSO)

---

## References

- [My Notes: Cost Function]({{% relref "03-cost-function.md" %}})
- [My Notes: Linear Models]({{% relref "03-linear-models-regression.md" %}})
- [My Notes: Gradient Descent]({{% relref "03-gradient-descent-linear-regression.md" %}})

---

{{< home-link "Home" >}} | {{< section-index >}}
