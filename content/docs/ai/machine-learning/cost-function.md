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
- **how far the predicted values are from the actual ones**
- measure of the difference between predicted values and actual values
- quantifies the error between a model's predicted values and actual values
- measures the model’s error on a group of datapoints
- method used to predict values by drawing the best-fit line through the data
- used to evaluate the accuracy of a model’s predictions
- guides the process of adjusting the model’s parameters in order to minimise the difference between predicted and actual values

{{< mermaid >}}
flowchart TD
CF["Cost<br/>function"] -->|measures| ERR["Prediction<br/>error"]
CF -->|guides| OPT["Optimisation<br/>(training)"]

CF -->|includes| DATA["Data<br/>fit"]
CF -->|may include| PEN["Penalty<br/>(regularisation)"]

style CF fill:#90CAF9,stroke:#1E88E5,color:#000

style ERR fill:#CE93D8,stroke:#8E24AA,color:#000
style OPT fill:#CE93D8,stroke:#8E24AA,color:#000

style DATA fill:#C8E6C9,stroke:#2E7D32,color:#000
style PEN fill:#C8E6C9,stroke:#2E7D32,color:#000
{{< /mermaid >}}

{{% hint info %}}
Key takeaway:
In {{< colour "blue" >}}ML{{< /colour >}}, we choose model parameters to minimise a cost $J$.
For linear regression, the most common choice is the squared error cost.
{{% /hint %}}

---

## Training set and model

You have a training set with:
- input features $x$
- output targets $y$

The linear regression model is:

{{< colour "blue" >}}
{{< katex display=true >}}
f_{w,b}(x)=wx+b
{{< /katex >}}
{{< /colour >}}

---

## Computing Cost

{{< colour "blue" >}}
{{< katex display=true >}}
J(w,b)=\frac{1}{2m}\sum_{i=0}^{m-1}\left(f_{w,b}\!\left(x^{(i)}\right)-y^{(i)}\right)^2
{{< /katex >}}
{{< /colour >}}

![Cost Fn](/images/ai/cost-function.png)

---

## Squared Error & Mean Squared Error (MSE)

| Feature | Squared Error | Mean Squared Error (MSE) |
|---|---|---|
| Scope | Individual data point (residual) | Entire dataset |
| Formula | {{< katex >}}(\hat{y}_i - y_i)^2{{< /katex >}} | {{< katex >}}\frac{1}{n}\sum_{i=1}^{n}(\hat{y}_i - y_i)^2{{< /katex >}} |
| Output | One value per observation | One single value for the model |
| Purpose | Measures individual error | Measures overall model performance |

---

## Types

{{< mermaid >}}
flowchart TD
T["Cost function<br/>types"] --> REG["Regression"]
T --> CLS["Classification"]
T --> PROB["Probabilistic<br/>models"]
T --> REGZ["Regularisation"]

REG --> MSE["MSE"]
REG --> MAE["MAE"]
REG --> HUB["Huber"]

CLS --> CE["Cross-entropy<br/>(log loss)"]
CLS --> HNG["Hinge"]

PROB --> NLL["Negative<br/>log-likelihood"]

REGZ --> L2["L2 (Ridge)"]
REGZ --> L1["L1 (Lasso)"]
REGZ --> EN["Elastic<br/>Net"]

style T fill:#90CAF9,stroke:#1E88E5,color:#000

style REG fill:#C8E6C9,stroke:#2E7D32,color:#000
style CLS fill:#C8E6C9,stroke:#2E7D32,color:#000
style PROB fill:#C8E6C9,stroke:#2E7D32,color:#000
style REGZ fill:#C8E6C9,stroke:#2E7D32,color:#000

style MSE fill:#CE93D8,stroke:#8E24AA,color:#000
style MAE fill:#CE93D8,stroke:#8E24AA,color:#000
style HUB fill:#CE93D8,stroke:#8E24AA,color:#000
style CE fill:#CE93D8,stroke:#8E24AA,color:#000
style HNG fill:#CE93D8,stroke:#8E24AA,color:#000
style NLL fill:#CE93D8,stroke:#8E24AA,color:#000
style L2 fill:#CE93D8,stroke:#8E24AA,color:#000
style L1 fill:#CE93D8,stroke:#8E24AA,color:#000
style EN fill:#CE93D8,stroke:#8E24AA,color:#000
{{< /mermaid >}}

---

### Cross-Entropy (Log Loss)

{{< colour "blue" >}}
{{< katex display=true >}}
\mathrm{Cost}(p,y)=-y\log(p)-(1-y)\log(1-p)
{{< /katex >}}
{{< /colour >}}

{{< colour "blue" >}}
{{< katex display=true >}}
J=-\frac{1}{m}\sum_{i=1}^{m}\left[y^{(i)}\log(p^{(i)})+(1-y^{(i)})\log(1-p^{(i)})\right]
{{< /katex >}}
{{< /colour >}}

---

## Linear vs Logistic (5-Step Comparison)

Model → Predict → Cost → Gradient → Update

| Step | Linear Regression (MSE / Squared Error) | Logistic Regression (Log Loss / Cross-Entropy) |
|---|---|---|
| 1) Model | {{< colour "blue" >}}{{< katex >}}\hat{y}=wx+b{{< /katex >}}{{< /colour >}} | {{< colour "blue" >}}{{< katex >}}z=w\cdot x+b,\quad p=\sigma(z)=\frac{1}{1+e^{-z}} {{< /katex >}}{{< /colour >}} |
| 2) Predict | {{< colour "blue" >}}{{< katex >}}\hat{y}^{(i)}=wx^{(i)}+b{{< /katex >}}{{< /colour >}} | {{< colour "blue" >}}{{< katex >}}z^{(i)}=w\cdot x^{(i)}+b,\quad p^{(i)}=\sigma(z^{(i)}){{< /katex >}}{{< /colour >}} |
| 3) Cost | {{< colour "blue" >}}{{< katex >}}J(w,b)=\frac{1}{2m}\sum_{i=1}^{m}\left(\hat{y}^{(i)}-y^{(i)}\right)^2{{< /katex >}}{{< /colour >}} | {{< colour "blue" >}}{{< katex >}}J(w,b)=\frac{1}{m}\sum_{i=1}^{m}\left[-y^{(i)}\log p^{(i)}-(1-y^{(i)})\log(1-p^{(i)})\right]{{< /katex >}}{{< /colour >}} |
| 4) Gradients | {{< colour "blue" >}}{{< katex >}}\frac{\partial J}{\partial w}=\frac{1}{m}\sum_{i=1}^{m}\left(\hat{y}^{(i)}-y^{(i)}\right)x^{(i)},\quad \frac{\partial J}{\partial b}=\frac{1}{m}\sum_{i=1}^{m}\left(\hat{y}^{(i)}-y^{(i)}\right){{< /katex >}}{{< /colour >}} | {{< colour "blue" >}}{{< katex >}}\frac{\partial J}{\partial w}=\frac{1}{m}\sum_{i=1}^{m}\left(p^{(i)}-y^{(i)}\right)x^{(i)},\quad \frac{\partial J}{\partial b}=\frac{1}{m}\sum_{i=1}^{m}\left(p^{(i)}-y^{(i)}\right){{< /katex >}}{{< /colour >}} |
| 5) Update | {{< colour "blue" >}}{{< katex >}}w:=w-\alpha\frac{\partial J}{\partial w},\quad b:=b-\alpha\frac{\partial J}{\partial b}{{< /katex >}}{{< /colour >}} | {{< colour "blue" >}}{{< katex >}}w:=w-\alpha\frac{\partial J}{\partial w},\quad b:=b-\alpha\frac{\partial J}{\partial b}{{< /katex >}}{{< /colour >}} |

---

{{< home-link "Home" >}} | {{< section-index >}}
