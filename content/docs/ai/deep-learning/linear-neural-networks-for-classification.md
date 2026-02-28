---
title: "LNN for Classification"
date: 2026-02-15
draft: false
weight: 400
tags: ["Deep Learning", "Classification", "Optimisation"]
categories: ["AI", "Machine Learning"]
---

# Linear NN for Classification

A **Linear Neural Network (LNN) for classification** uses **no hidden layers**.  
It learns a **linear decision boundary** and outputs **class probabilities**, then converts them into predicted classes.

{{% hint info %}}
Neural-network view:
- **Binary classification** → logistic regression (single neuron + sigmoid)
- **Multi-class classification** → softmax regression (K output neurons + softmax)
{{% /hint %}}

---

## Classification

Classification predicts a **discrete class label**.  
Common settings:
- **Binary classification**: {{< katex >}}y \in \{0,1\}{{< /katex >}}
- **Multi-class classification**: {{< katex >}}y \in \{1,2,\dots,K\}{{< /katex >}} (exactly one class)
- **Multi-label classification**: one example can belong to multiple classes (handled differently)

---

## The Four Components

A complete ML system for **linear classification (single neuron)** has four components:
- **Data**: input feature matrix {{< katex >}}X \in \mathbb{R}^{N \times d}{{< /katex >}} and binary targets {{< katex >}}y \in \{0,1\}^N{{< /katex >}}
- **Model**: single neuron implementing a linear function followed by a sigmoid activation (maps inputs to probabilities)
- **Objective**: binary cross-entropy loss (measures prediction error)
- **Learning algorithm**: Stochastic gradient descent (SGD) to optimise weights

{{% hint info %}}
This same template extends directly to multi-class classification and deep neural networks.  
Only the model becomes multi-output (softmax) or deeper (MLP), and backpropagation computes gradients efficiently.
{{% /hint %}}

---

## Binary Classification: Logistic Regression as a Single Neuron

{{< mermaid >}}
flowchart LR

  %% Inputs (features + bias)
  B["1 (bias input)"] -->|w0| SUM["Weighted sum<br/>z = w^T x + b"]
  X1["x1"] -->|w1| SUM
  X2["x2"] -->|w2| SUM
  XD["x_d"] -->|w_d| SUM

  %% Activation + output
  SUM --> SIG["Sigmoid<br/>ŷ = σ(z)"]
  SIG --> OUT["Output probability<br/>ŷ = P(y=1|x) ∈ [0,1]"]

  %% Pastel colour scheme (same as your regression diagram)
  style B fill:#E3F2FD,stroke:#1E88E5,stroke-width:1px
  style X1 fill:#E3F2FD,stroke:#1E88E5,stroke-width:1px
  style X2 fill:#E3F2FD,stroke:#1E88E5,stroke-width:1px
  style XD fill:#E3F2FD,stroke:#1E88E5,stroke-width:1px

  style SUM fill:#FFF3E0,stroke:#FB8C00,stroke-width:1px
  style SIG fill:#E8F5E9,stroke:#43A047,stroke-width:1px
  style OUT fill:#FCE4EC,stroke:#D81B60,stroke-width:1px

{{< /mermaid >}}

### Model (logit → probability)

{{< colour "green" >}}
{{< katex display=true >}}
z = w^T x + b
{{< /katex >}}
{{< /colour >}}

{{< colour "green" >}}
{{< katex display=true >}}
\hat{p} = \sigma(z) = \frac{1}{1 + e^{-z}}
{{< /katex >}}
{{< /colour >}}

- {{< katex >}}\hat{p}{{< /katex >}} is interpreted as {{< katex >}}P(y=1 \mid x){{< /katex >}}.
- A simple decision rule uses a threshold:
  - predict class 1 if {{< katex >}}\hat{p} \ge 0.5{{< /katex >}}
  - else class 0

### Why not use linear regression for classification?

Linear regression outputs any real number, which is not a probability and does not naturally map to classes.  
Sigmoid fixes this by mapping the logit to a valid probability in {{< katex >}}(0,1){{< /katex >}}.

---

## Sigmoid Function (quick properties)

- monotonic increasing (larger {{< katex >}}z{{< /katex >}} → larger probability)
- smooth and differentiable
- gives confident outputs when {{< katex >}}z{{< /katex >}} has large magnitude

Derivative (useful in learning):

{{< colour "green" >}}
{{< katex display=true >}}
\sigma'(z) = \sigma(z)\big(1-\sigma(z)\big)
{{< /katex >}}
{{< /colour >}}

---

## Binary Loss: Cross-Entropy

Binary cross-entropy compares predicted probabilities to true labels.

{{< colour "green" >}}
{{< katex display=true >}}
J(w,b) = -\frac{1}{N}\sum_{i=1}^{N}\left[y^{(i)}\log(\hat{p}^{(i)}) + (1-y^{(i)})\log(1-\hat{p}^{(i)})\right]
{{< /katex >}}
{{< /colour >}}

Why cross-entropy (intuition):
- probabilistic interpretation (maximum likelihood)
- heavily penalises **confident wrong** predictions
- well-behaved gradients for sigmoid-based learning

---

## Training: Gradient Descent (usually SGD / Mini-batch)

Parameter update (generic):

{{< colour "green" >}}
{{< katex display=true >}}
\theta \leftarrow \theta - \eta \nabla_{\theta} J(\theta)
{{< /katex >}}
{{< /colour >}}

In practice for deep learning, you usually train with:
- **mini-batch gradient descent** (standard)
- often with **Adam** or **SGD + momentum**

---

## Binary Classification Metrics

Use metrics that match the cost of mistakes.

- **Accuracy**: good when classes are balanced
- **Precision**: important when false positives are costly (e.g., spam flagging)
- **Recall**: important when false negatives are costly (e.g., disease screening)
- **F1 score**: balances precision and recall
- **Confusion matrix**: shows TP/FP/FN/TN counts

{{< colour "green" >}}
{{< katex display=true >}}
\text{Accuracy} = \frac{TP + TN}{TP + TN + FP + FN}
{{< /katex >}}
{{< /colour >}}

{{< colour "green" >}}
{{< katex display=true >}}
\text{Precision} = \frac{TP}{TP + FP}
{{< /katex >}}
{{< /colour >}}

{{< colour "green" >}}
{{< katex display=true >}}
\text{Recall} = \frac{TP}{TP + FN}
{{< /katex >}}
{{< /colour >}}

{{< colour "green" >}}
{{< katex display=true >}}
F_1 = \frac{2 \cdot \text{Precision} \cdot \text{Recall}}{\text{Precision} + \text{Recall}}
{{< /katex >}}
{{< /colour >}}

---

## Multi-class Classification: Softmax Regression

When there are {{< katex >}}K{{< /katex >}} classes, we use **K output neurons** (one per class).  
The model outputs logits, then softmax converts them into a probability distribution.

### Model

For each class {{< katex >}}k{{< /katex >}}:

{{< colour "green" >}}
{{< katex display=true >}}
z_k = w_k^T x + b_k
{{< /katex >}}
{{< /colour >}}

Softmax:

{{< colour "green" >}}
{{< katex display=true >}}
\hat{p}_k = \frac{e^{z_k}}{\sum_{j=1}^{K} e^{z_j}}
{{< /katex >}}
{{< /colour >}}

Properties:
- {{< katex >}}\hat{p}_k \in [0,1]{{< /katex >}}
- {{< katex >}}\sum_{k=1}^{K}\hat{p}_k = 1{{< /katex >}}
- prediction: choose the class with the largest probability

---

## Multi-class Loss: Categorical Cross-Entropy

With one-hot labels {{< katex >}}y_k^{(i)}{{< /katex >}}:

{{< colour "green" >}}
{{< katex display=true >}}
J(W,b) = -\frac{1}{N}\sum_{i=1}^{N}\sum_{k=1}^{K} y_k^{(i)}\log(\hat{p}_k^{(i)})
{{< /katex >}}
{{< /colour >}}

---

## Making Predictions (Inference)

Binary:
- compute {{< katex >}}\hat{p}{{< /katex >}}
- output probability, then threshold to get class

Multi-class:
- compute {{< katex >}}\hat{p}_1,\dots,\hat{p}_K{{< /katex >}}
- choose {{< katex >}}\arg\max_k \hat{p}_k{{< /katex >}}

---

## Pipeline view

{{< mermaid >}}
flowchart LR
  D["Data<br/>X, y"] --> M["Linear model<br/>w, b"]
  M --> A["Activation<br/>Sigmoid / Softmax"]
  A --> L["Loss<br/>Cross-entropy"]
  L --> O["Optimiser<br/>Mini-batch GD / Adam"]
  O --> P["Updated parameters<br/>w, b"]
  P --> I["Inference<br/>Probabilities → class"]

  %% Pastel colour scheme
  style D fill:#E3F2FD,stroke:#1E88E5,stroke-width:1px
  style M fill:#E8F5E9,stroke:#43A047,stroke-width:1px
  style A fill:#FFF3E0,stroke:#FB8C00,stroke-width:1px
  style L fill:#FCE4EC,stroke:#D81B60,stroke-width:1px
  style O fill:#F3E5F5,stroke:#8E24AA,stroke-width:1px
  style P fill:#E0F7FA,stroke:#00838F,stroke-width:1px
  style I fill:#F1F8E9,stroke:#558B2F,stroke-width:1px
{{< /mermaid >}}

---

## Implementation tips and debugging

Implementation tips:
- scale features (helps convergence and stability)
- check dimensions carefully ({{< katex >}}XW + b{{< /katex >}} shape issues are common)
- start with a tiny dataset and overfit on purpose (sanity check)
- monitor loss curve (should generally trend downward)

Debugging checklist:
- does loss decrease for a small batch?
- are labels encoded correctly (0/1 for binary, one-hot for multi-class)?
- are you using the right activation + loss pairing?
  - sigmoid + binary cross-entropy
  - softmax + categorical cross-entropy
- is learning rate too large (divergence) or too small (no learning)?

---

## Summary

- LNN classification uses **no hidden layers** and learns a **linear boundary**.
- **Binary**: sigmoid outputs a probability; train with binary cross-entropy.
- **Multi-class**: softmax outputs a probability distribution; train with categorical cross-entropy.
- Train with **mini-batch GD** (often Adam / SGD+momentum).
- Evaluate with the right metrics (accuracy vs precision/recall/F1 based on the problem).

## Reference
- Course slides: **DNN_M4_Linear NN Classification**.
- Zhang, Lipton, Li, Smola — *Dive into Deep Learning* (Linear Neural Networks for Classification: softmax + cross-entropy).
- Singh & Raj — Deep Learning notes (classification and learning fundamentals).

- **DNN Module #3 — Linear Neural Networks for Regression**. (T1 – Ch 4, T1 - Ch 12)

---

{{< home-link "Home" >}} | {{< section-index >}}
