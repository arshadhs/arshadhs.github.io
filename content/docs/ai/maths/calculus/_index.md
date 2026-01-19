---
title: "Calculus"
draft: false
tags: ["Machine Learning", "Mathematics", "Calculus"]
categories: ["AI", "ML"]
weight: 1140
menu: main
bookCollapseSection: true
---
# Calculus

Calculus is the mathematical framework for understanding and controlling how quantities change.

- How fast is something changing **right now**?
- What happens to a system when inputs change **slightly**?
- Where is something **maximum or minimum**?

---

{{< section_tree >}}

---

{{% steps }}
1. Differential Calculus (Rates of Change)
This part studies **how things change**.

- How steep is a curve at a point?
- Is something increasing or decreasing?
- Where are the maxima and minima?
- The key idea is the derivative.

A **derivative** measures how a small change in input affects the output.

Example intuition:
- Slope of a curve
- Instantaneous speed
- Gradient of a loss function

2. Integral Calculus (Accumulation)
This part studies **how things add up**.

- What is the total effect over time?
- How much area lies under a curve?
- How do small changes accumulate?

The key idea is the **integral**.

Example intuition:
- Total distance from speed
- Area under a curve
- Summing many tiny contributions

{{% /steps }}

---

## What is Multivariate Calculus?

Multivariate calculus is the branch of calculus that deals with functions of more than one variable.

Instead of working with functions like:
y = f(x)

it works with functions like:
z = f(x, y)
f(x, y, z)
L(w₁, w₂, ..., wₙ)

In Machine Learning, almost every function is multivariate.

---

## Why Multivariate Calculus Matters in Machine Learning

Machine learning models do not learn one parameter at a time.
They optimise many parameters simultaneously.

Example:
Loss(w₁, w₂, w₃, ..., wₙ)

### Multivariate calculus tells us:
- How changing each parameter affects the output
- Which direction reduces the error fastest
- Whether a solution is a minimum, maximum, or saddle point

---
- Univariate differentiation (revision)
- Partial derivatives
- Gradients
- Jacobian & Hessian
- Gradients of vectors & matrices
- Useful gradient identities
- Backpropagation (conceptual)
- Automatic differentiation

---

## Focus

- Compute gradients correctly
- Hessian-based minima/maxima
- Taylor series (multivariate)

---

## ML connection:

Training neural networks

---

{{< home-link "Home" >}} | {{< section-index >}}
