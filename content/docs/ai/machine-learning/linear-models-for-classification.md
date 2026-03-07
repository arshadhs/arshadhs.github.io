---
title: 'Linear models for classification'
draft: false
tags: ["AI", "ML"]
categories: ["AI", "ML"]
weight: 400
---

# Linear models for Classification

## Discriminant Functions

## Decision Theory

## Probabilistic Discriminative Classifiers

---

## Logistic Regression

- Supervised machine learning algorithm
- Binary **classification** algorithm
- requires data to be linearly separable
- predicts the probability that an input belongs to a specific class
- uses **Sigmoid function** to convert inputs into a probability value between 0 and 1

{{% hint info %}}
Key takeaway:
Logistic regression predicts $P(y=1\mid x)$ using a sigmoid of a linear score $z=w\cdot x+b$,
then learns $w,b$ by maximising likelihood (equivalently minimising log-loss).
{{% /hint %}}

---

### Types of Logistic Regression

Logistic regression can be classified into three main types based on the nature of the dependent variable.

{{< mermaid >}}
flowchart TD
T["Logistic Regression<br/>Types"] --> BIN["Binomial<br/>(Binary)"]
T --> MULTI["Multinomial"]
T --> ORD["Ordinal"]

BIN --> BIN1["Two classes<br/>0/1, Yes/No"]
MULTI --> MULTI1["3+ classes<br/>no natural order"]
ORD --> ORD1["3+ classes<br/>ordered categories"]

style T fill:#90CAF9,stroke:#1E88E5,color:#000
style BIN fill:#C8E6C9,stroke:#2E7D32,color:#000
style MULTI fill:#C8E6C9,stroke:#2E7D32,color:#000
style ORD fill:#C8E6C9,stroke:#2E7D32,color:#000
style BIN1 fill:#CE93D8,stroke:#8E24AA,color:#000
style MULTI1 fill:#CE93D8,stroke:#8E24AA,color:#000
style ORD1 fill:#CE93D8,stroke:#8E24AA,color:#000
{{< /mermaid >}}

#### Binomial Logistic Regression
- dependent variable has two possible categories (e.g. Yes/No, Pass/Fail, 0/1)
- most common form
- used for binary classification problems

#### Multinomial Logistic Regression
- dependent variable has three or more categories that are not ordered
- e.g. "cat", "dog", "sheep"
- extends binary logistic regression to multiple classes

#### Ordinal Logistic Regression
- dependent variable has three or more categories with a natural order
- e.g. "low", "medium", "high"
- takes ordering into account when modelling

---

### Multiclass strategies (OvR / OvO)

Logistic regression is naturally binary.
For multi-class problems:

#### One-vs-Rest (OvR)
- Train $K$ binary classifiers (one per class)
- Pick the class with the highest probability

{{< colour "blue" >}}
{{< katex display=true >}}
\hat{k}=\arg\max_{k\in\{1,\dots,K\}} p_k(x)
{{< /katex >}}
{{< /colour >}}

#### One-vs-One (OvO)
- Train one classifier per pair of classes
- Total classifiers: $K(K-1)/2$
- Use voting (majority wins)

---

### Assumptions of Logistic Regression

- Independent observations:
each data point is assumed independent (no dependence between samples)

- Dependent variable type:
binary for standard logistic regression
(for more than two categories, multinomial/softmax is used)

- Linearity in the log-odds:
predictors affect the **log-odds** in a linear way

- No extreme outliers:
outliers can distort coefficient estimates

- Large sample size:
needs enough data for stable, reliable estimates

---

### Understanding the Sigmoid Function

The sigmoid function converts a real-valued input to a probability between 0 and 1.

{{< colour "blue" >}}
{{< katex display=true >}}
\sigma(z)=\frac{1}{1+e^{-z}}
{{< /katex >}}
{{< /colour >}}

Properties:
- $\sigma(z)\to 1$ as $z\to \infty$
- $\sigma(z)\to 0$ as $z\to -\infty$
- $\sigma(z)$ is always in $(0,1)$

Classification using a threshold (commonly $0.5$):
- if $\sigma(z)\ge 0.5$:
predict Class 1
- if $\sigma(z)<0.5$:
predict Class 0

---

### How Logistic Regression Works

#### Input features and labels

Input features (matrix):

{{< colour "blue" >}}
{{< katex display=true >}}
X=
\begin{bmatrix}
x_{11} & \dots & x_{1m}\\
x_{21} & \dots & x_{2m}\\
\vdots & \ddots & \vdots\\
x_{n1} & \dots & x_{nm}
\end{bmatrix}
{{< /katex >}}
{{< /colour >}}

Dependent variable (binary):

{{< colour "blue" >}}
{{< katex display=true >}}
y \in \{0,1\}
{{< /katex >}}
{{< /colour >}}

#### Linear score

Compute a linear score:

{{< colour "blue" >}}
{{< katex display=true >}}
z=\left(\sum_{i=1}^{n}w_i x_i\right)+b
{{< /katex >}}
{{< /colour >}}

Vector form:

{{< colour "blue" >}}
{{< katex display=true >}}
z=w\cdot x+b
{{< /katex >}}
{{< /colour >}}

Here:
- $w$ are weights / coefficients
- $b$ is the bias (intercept)

#### Probability prediction

Convert $z$ to a probability:

{{< colour "blue" >}}
{{< katex display=true >}}
p(x)=P(y=1\mid x)=\sigma(z)
{{< /katex >}}
{{< /colour >}}

So:
- $P(y=1)=p(x)$
- $P(y=0)=1-p(x)$

---

### Odds, Log-Odds (Logit), and the Logistic Equation

Odds of the event:

{{< colour "blue" >}}
{{< katex display=true >}}
\frac{p(x)}{1-p(x)} = e^{z}
{{< /katex >}}
{{< /colour >}}

Log-odds (logit):

{{< colour "blue" >}}
{{< katex display=true >}}
\log\left(\frac{p(x)}{1-p(x)}\right)=z=w\cdot x+b
{{< /katex >}}
{{< /colour >}}

Solving for $p(x)$ gives:

{{< colour "blue" >}}
{{< katex display=true >}}
p(x)=\frac{e^{w\cdot x+b}}{1+e^{w\cdot x+b}}
=
\frac{1}{1+e^{-(w\cdot x+b)}}
{{< /katex >}}
{{< /colour >}}

---

### Cost Function (Log Loss / Cross-Entropy)

Per-example cost:

{{< colour "blue" >}}
{{< katex display=true >}}
\mathrm{Cost}(p,y)=-y\log(p)-(1-y)\log(1-p)
{{< /katex >}}
{{< /colour >}}

Dataset cost:

{{< colour "blue" >}}
{{< katex display=true >}}
J=-\frac{1}{m}\sum_{i=1}^{m}\left[y^{(i)}\log(p^{(i)})+(1-y^{(i)})\log(1-p^{(i)})\right]
{{< /katex >}}
{{< /colour >}}

---

### Evaluation metrics

#### Confusion Matrix

Positive class = 1

{{< colour "blue" >}}
{{< katex display=true >}}
\begin{array}{c|cc}
 & \text{Pred }1 & \text{Pred }0\\ \hline
\text{Actual }1 & TP & FN\\
\text{Actual }0 & FP & TN
\end{array}
{{< /katex >}}
{{< /colour >}}

#### Accuracy

{{< colour "blue" >}}
{{< katex display=true >}}
\text{Accuracy}=\frac{TP+TN}{TP+FP+FN+TN}
{{< /katex >}}
{{< /colour >}}

#### Precision

{{< colour "blue" >}}
{{< katex display=true >}}
\text{Precision}=\frac{TP}{TP+FP}
{{< /katex >}}
{{< /colour >}}

#### Recall

{{< colour "blue" >}}
{{< katex display=true >}}
\text{Recall}=\frac{TP}{TP+FN}
{{< /katex >}}
{{< /colour >}}

#### F1 score

{{< colour "blue" >}}
{{< katex display=true >}}
F1=\frac{2\cdot \text{Precision}\cdot \text{Recall}}{\text{Precision}+\text{Recall}}
{{< /katex >}}
{{< /colour >}}

---

### ROC Curve and AUC

{{< colour "blue" >}}
{{< katex display=true >}}
TPR=\frac{TP}{TP+FN}
{{< /katex >}}
{{< /colour >}}

{{< colour "blue" >}}
{{< katex display=true >}}
FPR=\frac{FP}{FP+TN}
{{< /katex >}}
{{< /colour >}}

AUC:
area under the ROC curve.

---

## Decision Tree

Decision Tree is a non-linear classifier that splits the feature space using if-then rules.

When to use:
- relationships are non-linear
- feature interactions matter
- you want rule-based interpretability

Common risks:
- can overfit without pruning / depth limits
- small changes in data can change the tree significantly

---

## References

- [Maximum Likelihood Estimation (MLE) and Maximum A Posteriori (MAP)](https://www.geeksforgeeks.org/data-science/mle-vs-map/)

---

{{< home-link "Home" >}} | {{< section-index >}}
