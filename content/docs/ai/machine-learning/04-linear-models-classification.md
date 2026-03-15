---
title: 'Classification (Linear Models)'
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

Logistic regression is naturally a **binary** classifier.
To handle more than two classes, use strategies like One-vs-Rest or One-vs-One.

#### One-vs-Rest (OvR)

- Train $K$ binary logistic regression models (one per class).
- For class $k$, set labels:
  - $y=1$ if the example belongs to class $k$
  - $y=0$ otherwise
- At prediction time, choose the class with the highest predicted probability.

{{< colour "blue" >}}
{{< katex display=true >}}
\hat{k}=\arg\max_{k\in\{1,\dots,K\}} p_k(x)
{{< /katex >}}
{{< /colour >}}

When to use:
- Works well when $K$ is moderate.
- Simple to implement.

#### One-vs-One (OvO)

- Train a binary classifier for every pair of classes.
- Total classifiers: $K(K-1)/2$.
- At prediction time, each classifier votes; the class with most votes wins.

When to use:
- Useful when $K$ is small.
- Training can be heavier if $K$ is large.

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

### Link function vs sigmoid (common confusion)

Logistic Regression can be described in two linked ways:

1) Linear score:
$z=w\cdot x+b$

2) Probability:
$p=\sigma(z)$

The **logit** is the link between probability and score:

{{< colour "blue" >}}
{{< katex display=true >}}
\log\left(\frac{p}{1-p}\right)=w\cdot x+b
{{< /katex >}}
{{< /colour >}}

- logit:
often called the **link function** in GLM language
- sigmoid:
the **inverse link** that maps $z$ to a probability

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

Solving for $p(x)$ gives the logistic regression probability:

{{< colour "blue" >}}
{{< katex display=true >}}
p(x)=\frac{e^{w\cdot x+b}}{1+e^{w\cdot x+b}}
=
\frac{1}{1+e^{-(w\cdot x+b)}}
{{< /katex >}}
{{< /colour >}}

This is the probability that the input belongs to Class 1.

---

### Likelihood and Log-Likelihood

The goal is to find $w$ and $b$ that maximise the likelihood of the observed data.

For each data point $i$:
- if $y_i=1$:
probability is $p(x_i)$
- if $y_i=0$:
probability is $1-p(x_i)$

Likelihood:

{{< colour "blue" >}}
{{< katex display=true >}}
L(b,w)=\prod_{i=1}^{n} p(x_i)^{y_i}\left(1-p(x_i)\right)^{1-y_i}
{{< /katex >}}
{{< /colour >}}

Log-likelihood:

{{< colour "blue" >}}
{{< katex display=true >}}
\log L(b,w)=\sum_{i=1}^{n}\left[y_i\log p(x_i) + (1-y_i)\log\left(1-p(x_i)\right)\right]
{{< /katex >}}
{{< /colour >}}

---

### Gradient of the Log-Likelihood

To find the best $w$ and $b$ we can use gradient ascent on $\log L$.

For weight component $w_j$:

{{< colour "blue" >}}
{{< katex display=true >}}
\frac{\partial \log L}{\partial w_j}
=
\sum_{i=1}^{n}\left(y_i - p(x_i)\right)x_{ij}
{{< /katex >}}
{{< /colour >}}

---

### Evaluation metrics for classification

To evaluate Logistic Regression (and other classifiers), we often use a **confusion matrix** and derived metrics.

#### Confusion Matrix

For binary classification (positive class = 1):

- True Positive (TP):
actual 1, predicted 1
- False Positive (FP):
actual 0, predicted 1
- False Negative (FN):
actual 1, predicted 0
- True Negative (TN):
actual 0, predicted 0

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

Accuracy measures the overall fraction of correct predictions.

{{< colour "blue" >}}
{{< katex display=true >}}
\text{Accuracy}=\frac{TP+TN}{TP+FP+FN+TN}
{{< /katex >}}
{{< /colour >}}

When to use:
- When classes are fairly balanced.
- When FP and FN have similar cost.

#### Precision

Precision tells you: “When the model predicts 1, how often is it correct?”

{{< colour "blue" >}}
{{< katex display=true >}}
\text{Precision}=\frac{TP}{TP+FP}
{{< /katex >}}
{{< /colour >}}

When to use:
- When false positives are costly.
- Example:
flagging a legitimate customer as fraud.

#### Recall (Sensitivity / True Positive Rate)

Recall tells you: “Out of all actual 1s, how many did we catch?”

{{< colour "blue" >}}
{{< katex display=true >}}
\text{Recall}=\frac{TP}{TP+FN}
{{< /katex >}}
{{< /colour >}}

When to use:
- When false negatives are costly.
- Example:
missing a fraud case / missing a disease.

#### F1 score

{{< colour "blue" >}}
{{< katex display=true >}}
F1=\frac{2\cdot \text{Precision}\cdot \text{Recall}}{\text{Precision}+\text{Recall}}
{{< /katex >}}
{{< /colour >}}

#### Log loss (Cross-entropy)

Log loss evaluates **probabilities**, not just hard class labels.
It penalises confident wrong predictions heavily.

Per-example:

{{< colour "blue" >}}
{{< katex display=true >}}
\mathrm{Cost}\!\left(p,y\right)
=
-y\log(p)-(1-y)\log(1-p)
{{< /katex >}}
{{< /colour >}}

Over the full dataset:

{{< colour "blue" >}}
{{< katex display=true >}}
J=-\frac{1}{m}\sum_{i=1}^{m}\left[y^{(i)}\log(p^{(i)})+(1-y^{(i)})\log(1-p^{(i)})\right]
{{< /katex >}}
{{< /colour >}}

When to use:
- When you care about **well-calibrated probabilities**.
- When comparing probabilistic classifiers (like logistic regression).

---

#### ROC curve and AUC

ROC curve plots:
- True Positive Rate (TPR) vs False Positive Rate (FPR)
as you vary the classification threshold.

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
Area under the ROC curve.

When to use:
- When you want a threshold-independent performance summary.
- Often useful when classes are imbalanced and you want ranking quality.

---

## Decision Tree

A Decision Tree is a non-linear classification model that splits the feature space into regions using if-then rules.

How it works:
- choose a feature and split threshold
- split the data into two (or more) groups
- repeat until a stopping rule is met

Common split criteria:
- Gini impurity
- Entropy / Information Gain

When to use:
- when relationships are non-linear
- when you want interpretable rule-based decisions
- when feature interactions matter

Typical risks:
- can overfit without pruning / depth limits
- can be unstable (small data changes can change the tree)

Evaluation:
Use the same metrics as logistic regression:
confusion matrix, accuracy, precision, recall, ROC/AUC.

---

### Terminology

- Independent variables:
input features / predictors
- Dependent variable:
target variable (categorical)
- Logistic function:
sigmoid mapping to probability
- Odds:
$\frac{p}{1-p}$
- Log-odds (logit):
$\log\left(\frac{p}{1-p}\right)$
- Coefficients:
the weights $w$
- Intercept:
the bias $b$
- Maximum Likelihood Estimation (MLE):
estimate $w,b$ by maximising likelihood of observed data

---

## References

- [Maximum Likelihood Estimation (MLE) and Maximum A Posteriori (MAP)](https://www.geeksforgeeks.org/data-science/mle-vs-map/)

---

{{< home-link "Home" >}} | {{< section-index >}}

