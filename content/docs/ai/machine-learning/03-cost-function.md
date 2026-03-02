---
title: "Cost Function"
date: 2026-02-21
draft: false
weight: 330
tags: ["AI", "ML", "Linear Regression", "OLS"]
categories: ["AI", "ML"]
---

# Cost Function

- also known as an objective function

- measure of the difference between predicted values and actual values
- quantifies the error between a model's predicted values and actual values
- measures the model’s error on a group of datapoints

- method used to predict values by drawing the best-fit line through the data

- used to evaluate the accuracy of a model’s predictions 
- to guide the process of adjusting the model’s parameters in order to minimise the difference between predicted and actual values

{{< mermaid >}}
flowchart TD
CF["Cost<br/>function"] -->|measures| ERR["Prediction<br/>error"]
CF -->|guides| OPT["Optimisation<br/>(training)"]

CF --> T["Types"]

T --> REG["Regression"]
REG --> MSE["MSE"]
REG --> MAE["MAE"]
REG --> HUB["Huber"]

T --> CLS["Classification"]
CLS --> LOG["Log loss<br/>(Cross-entropy)"]
CLS --> HNG["Hinge"]

T --> PROB["Probabilistic<br/>models"]
PROB --> NLL["Negative<br/>log-likelihood"]

T --> REGZ["Regularisation"]
REGZ --> L2["L2 (Ridge)"]
REGZ --> L1["L1 (Lasso)"]
REGZ --> EN["Elastic<br/>Net"]

style CF fill:#90CAF9,stroke:#1E88E5,color:#000
style ERR fill:#CE93D8,stroke:#8E24AA,color:#000
style OPT fill:#CE93D8,stroke:#8E24AA,color:#000
style T fill:#CE93D8,stroke:#8E24AA,color:#000

style REG fill:#C8E6C9,stroke:#2E7D32,color:#000
style CLS fill:#C8E6C9,stroke:#2E7D32,color:#000
style PROB fill:#C8E6C9,stroke:#2E7D32,color:#000
style REGZ fill:#C8E6C9,stroke:#2E7D32,color:#000

style MSE fill:#C8E6C9,stroke:#2E7D32,color:#000
style MAE fill:#C8E6C9,stroke:#2E7D32,color:#000
style HUB fill:#C8E6C9,stroke:#2E7D32,color:#000
style LOG fill:#C8E6C9,stroke:#2E7D32,color:#000
style HNG fill:#C8E6C9,stroke:#2E7D32,color:#000
style NLL fill:#C8E6C9,stroke:#2E7D32,color:#000
style L2 fill:#C8E6C9,stroke:#2E7D32,color:#000
style L1 fill:#C8E6C9,stroke:#2E7D32,color:#000
style EN fill:#C8E6C9,stroke:#2E7D32,color:#000
{{< /mermaid >}}

**Squared Error Function**
![Zebrato](/images/ai/cost-function.png)

---

## Loss Function

- defined on a single training example.
- measures how well your model performing on a single training exampls

But if we consider the entire training set and try to measure how well is our model performing on it, we define a function called the cost function.

---

## Role of Gradient Descent in Updating the Weights

Gradient descent is an optimisation algorithm used to minimize the cost function and find the best-fit line for the model.

- Iteratively adjust the weights of the model to reduce the error. 
- each iteration updates the weights in the direction that minimises the cost function leading to the optimal set of parameters.

---

## References

- [Cost Function](https://www.geeksforgeeks.org/machine-learning/what-is-the-cost-function-in-linear-regression/)

---

{{< home-link "Home" >}} | {{< section-index >}}

---