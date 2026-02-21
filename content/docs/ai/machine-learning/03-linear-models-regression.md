---
title: 'Linear models for Regression'
draft: false
tags: ["AI", "ML"]
categories: ["AI", "ML"]
weight: 430
---
# Linear models for Regression
 
Linear regression is a method used to predict a **numerical** output by fitting a model that is a linear combination of parameters.

{{% hint info %}}
Key takeaway:
Linear regression fits a simple, interpretable baseline model that is often a good starting point before using more complex methods.
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

## Checklist

When you build a linear regression model, always check:
- Is a line a sensible shape for this relationship?
- Are there outliers strongly influencing the fit?
- Does the model generalise (train vs test performance)?
- Are the features properly scaled/encoded where needed?

---

## Direct Solution Method 

## Iterative Method – Gradient Descent (batch/stochastic/mini-batch)  

## Linear basis function models  

## Bias-variance decomposition 

---

## References

- [STAT 501 (Penn State) lesson used in the example](https://online.stat.psu.edu/stat501/lesson/1/1.7)

{{< relref "03-ordinary-least-squares.md" >}}
{{< relref "03-gradient-descent-linear-regression.md" >}}

---

{{< home-link "Home" >}} | {{< section-index >}}

