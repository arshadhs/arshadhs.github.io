---
title: "Deep Feedforward Neural Networks (DFNN) for Classification"
date: 2026-02-26
draft: false
weight: 500
tags: ["Deep Learning", "DFNN", "MLP", "Classification"]
categories: ["AI", "Machine Learning"]
---

# Deep Feedforward Neural Networks (DFNN) or Multi Layer Perceptrons (MLP) for Classification

A **Deep Feedforward Neural Network (DFNN)**, also called a **Multi-Layer Perceptron (MLP)**, is a neural network with one or more **hidden layers** where information flows **forward only** (no recurrence).  
For classification, DFNNs learn **non-linear decision boundaries** by combining hidden layers with **non-linear activation functions**.

{{% hint info %}}
Core idea:
- A single neuron can only learn **linear** boundaries.
- Adding **hidden layers + non-linearity** allows DFNNs to solve problems like **XOR**.
{{% /hint %}}

---

## MLP as solution for XOR

A single perceptron fails on XOR because XOR is **not linearly separable**.

An MLP solves XOR by:
- using at least **one hidden layer**
- using a **non-linear activation** in the hidden layer
- combining hidden neurons to create a **non-linear** decision boundary

{{% hint warning %}}
XOR is the simplest “proof by example” that you need hidden layers for non-linear separation.
{{% /hint %}}

---

## Hidden layers and non-linearity

Hidden layers apply:
1) a linear transform (weights + bias)
2) a non-linear activation (ReLU / sigmoid / tanh)

Without non-linearity, stacking layers collapses to a single linear function.

---

## Forward Propagation (vectorised)

Assume:
- input dimension {{< katex >}}d{{< /katex >}}
- output classes {{< katex >}}K{{< /katex >}}
- number of layers {{< katex >}}L{{< /katex >}} (including output layer)
- batch size {{< katex >}}m{{< /katex >}}

Batch matrix:
- {{< katex >}}X \in \mathbb{R}^{m \times d}{{< /katex >}}
- {{< katex >}}A^{[0]} = X{{< /katex >}}

For layer {{< katex >}}l{{< /katex >}}:

{{< colour "green" >}}
{{< katex display=true >}}
Z^{[l]} = A^{[l-1]} W^{[l]} + \mathbf{1} \, (b^{[l]})^T
{{< /katex >}}
{{< /colour >}}

{{< colour "green" >}}
{{< katex display=true >}}
A^{[l]} = g^{[l]}(Z^{[l]})
{{< /katex >}}
{{< /colour >}}

Where:
- {{< katex >}}W^{[l]}{{< /katex >}} is the weight matrix
- {{< katex >}}b^{[l]}{{< /katex >}} is the bias vector
- {{< katex >}}g^{[l]}{{< /katex >}} is the activation function
- {{< katex >}}\mathbf{1}{{< /katex >}} is an all-ones column vector (bias broadcast)

Output layer:
- **Binary**: sigmoid
- **Multi-class**: softmax

---

## Forward pass algorithm (batch)

```text
Input: X, {W[l], b[l]} for l=1..L
A[0] = X
for l = 1..L-1:
    Z[l] = A[l-1] W[l] + b[l]
    A[l] = g(Z[l])                 # ReLU / tanh / sigmoid
Z[L] = A[L-1] W[L] + b[L]
A[L] = output_activation(Z[L])     # sigmoid or softmax
Return A[L] (predictions)
```

---

## Define loss functions and compute the error

### Binary cross-entropy (sigmoid output)

{{< colour "green" >}}
{{< katex display=true >}}
J = -\frac{1}{m}\sum_{i=1}^{m}\left[y^{(i)}\log(\hat{p}^{(i)}) + (1-y^{(i)})\log(1-\hat{p}^{(i)})\right]
{{< /katex >}}
{{< /colour >}}

Where {{< katex >}}\hat{p}{{< /katex >}} is the predicted probability for class 1.

### Multi-class cross-entropy (softmax output)

{{< colour "green" >}}
{{< katex display=true >}}
J = -\frac{1}{m}\sum_{i=1}^{m}\sum_{k=1}^{K} y_k^{(i)} \log(\hat{p}_k^{(i)})
{{< /katex >}}
{{< /colour >}}

Where:
- {{< katex >}}y_k^{(i)}{{< /katex >}} is a one-hot label
- {{< katex >}}\hat{p}_k^{(i)}{{< /katex >}} is predicted probability for class {{< katex >}}k{{< /katex >}}

---

## Backward Propagation (vectorised)

Backprop computes gradients using the **chain rule** layer by layer from output to input.

Let:
- {{< katex >}}dZ^{[l]} = \frac{\partial J}{\partial Z^{[l]}}{{< /katex >}}
- {{< katex >}}dW^{[l]} = \frac{\partial J}{\partial W^{[l]}}{{< /katex >}}
- {{< katex >}}db^{[l]} = \frac{\partial J}{\partial b^{[l]}}{{< /katex >}}
- {{< katex >}}dA^{[l]} = \frac{\partial J}{\partial A^{[l]}}{{< /katex >}}

### Output layer gradients (softmax + cross-entropy shortcut)

{{< colour "green" >}}
{{< katex display=true >}}
dZ^{[L]} = A^{[L]} - Y
{{< /katex >}}
{{< /colour >}}

Then:

{{< colour "green" >}}
{{< katex display=true >}}
dW^{[L]} = \frac{1}{m}(A^{[L-1]})^T dZ^{[L]}
{{< /katex >}}
{{< /colour >}}

{{< colour "green" >}}
{{< katex display=true >}}
db^{[L]} = \frac{1}{m}\sum_{i=1}^{m} dZ^{[L]}_{(i,:)}
{{< /katex >}}
{{< /colour >}}

Propagate backward:

{{< colour "green" >}}
{{< katex display=true >}}
dA^{[L-1]} = dZ^{[L]} (W^{[L]})^T
{{< /katex >}}
{{< /colour >}}

### Hidden layer gradients

For {{< katex >}}l = L-1, \dots, 1{{< /katex >}}:

{{< colour "green" >}}
{{< katex display=true >}}
dZ^{[l]} = dA^{[l]} \odot g'^{[l]}(Z^{[l]})
{{< /katex >}}
{{< /colour >}}

{{< colour "green" >}}
{{< katex display=true >}}
dW^{[l]} = \frac{1}{m}(A^{[l-1]})^T dZ^{[l]}
{{< /katex >}}
{{< /colour >}}

{{< colour "green" >}}
{{< katex display=true >}}
db^{[l]} = \frac{1}{m}\sum_{i=1}^{m} dZ^{[l]}_{(i,:)}
{{< /katex >}}
{{< /colour >}}

{{< colour "green" >}}
{{< katex display=true >}}
dA^{[l-1]} = dZ^{[l]} (W^{[l]})^T
{{< /katex >}}
{{< /colour >}}

Where {{< katex >}}\odot{{< /katex >}} is element-wise multiplication.

---

## Backprop algorithm (batch)

```text
Input: cached {A[l], Z[l]} from forward pass, labels Y
Compute dZ[L] = A[L] - Y                      # softmax + CE shortcut
Compute dW[L] = (1/m) A[L-1]^T dZ[L]
Compute db[L] = (1/m) sum_rows(dZ[L])
dA[L-1] = dZ[L] W[L]^T

for l = L-1 down to 1:
    dZ[l] = dA[l] ⊙ g'(Z[l])
    dW[l] = (1/m) A[l-1]^T dZ[l]
    db[l] = (1/m) sum_rows(dZ[l])
    dA[l-1] = dZ[l] W[l]^T
Return {dW[l], db[l]}
```

---

## Weight updation using gradients (vectorised)

Using learning rate {{< katex >}}\eta{{< /katex >}}:

{{< colour "green" >}}
{{< katex display=true >}}
W^{[l]} \leftarrow W^{[l]} - \eta \, dW^{[l]}
{{< /katex >}}
{{< /colour >}}

{{< colour "green" >}}
{{< katex display=true >}}
b^{[l]} \leftarrow b^{[l]} - \eta \, db^{[l]}
{{< /katex >}}
{{< /colour >}}

---

## Impact of depth and width in DFNN

### Depth (more layers)
- Can represent complex functions more efficiently than shallow networks
- Encourages hierarchical feature learning (simple → complex)
- Can be harder to train without good initialisation/normalisation (vanishing/exploding gradients)

### Width (more neurons per layer)
- Increases capacity within a layer
- With limited data, higher width can increase overfitting risk

{{% hint info %}}
Rule of thumb:
- depth helps expressivity and feature hierarchy
- width helps capacity inside a layer
- both require good optimisation and regularisation
{{% /hint %}}

---

## Code implementation from scratch (webinar)

Below is a minimal “from scratch” XOR MLP (2–2–1) showing:
- vectorised forward pass
- backprop gradients
- gradient descent updates

```python
# XOR with a tiny MLP (2-2-1) from scratch (educational)
# - forward pass
# - backprop
# - gradient descent
#
# No ML libraries used.

import math
import random

def sigmoid(z):
    return 1.0 / (1.0 + math.exp(-z))

def dsigmoid(a):
    # derivative wrt z when a = sigmoid(z)
    return a * (1.0 - a)

def matmul(A, B):
    m, n = len(A), len(A[0])
    n2, p = len(B), len(B[0])
    assert n == n2
    out = [[0.0]*p for _ in range(m)]
    for i in range(m):
        for k in range(n):
            aik = A[i][k]
            for j in range(p):
                out[i][j] += aik * B[k][j]
    return out

def add_bias(M, b):
    return [[M[i][j] + b[j] for j in range(len(b))] for i in range(len(M))]

def transpose(A):
    return [list(row) for row in zip(*A)]

def elemwise(A, B):
    return [[A[i][j]*B[i][j] for j in range(len(A[0]))] for i in range(len(A))]

def sub(A, B):
    return [[A[i][j]-B[i][j] for j in range(len(A[0]))] for i in range(len(A))]

def scale(A, s):
    return [[A[i][j]*s for j in range(len(A[0]))] for i in range(len(A))]

def sum_rows(A):
    p = len(A[0])
    out = [0.0]*p
    for i in range(len(A)):
        for j in range(p):
            out[j] += A[i][j]
    return out

def sigmoid_mat(Z):
    return [[sigmoid(Z[i][j]) for j in range(len(Z[0]))] for i in range(len(Z))]

def dsigmoid_mat(A):
    return [[dsigmoid(A[i][j]) for j in range(len(A[0]))] for i in range(len(A))]

# XOR data
X = [[0.0,0.0],[0.0,1.0],[1.0,0.0],[1.0,1.0]]   # 4 x 2
Y = [[0.0],[1.0],[1.0],[0.0]]                   # 4 x 1

m = len(X)

def randmat(r, c, scale=1.0):
    return [[(random.random()-0.5)*scale for _ in range(c)] for _ in range(r)]

W1 = randmat(2, 2, scale=1.0)   # 2 x 2
b1 = [0.0, 0.0]                 # 2
W2 = randmat(2, 1, scale=1.0)   # 2 x 1
b2 = [0.0]                      # 1

lr = 0.5
epochs = 5000

for _ in range(epochs):
    # forward
    Z1 = add_bias(matmul(X, W1), b1)     # 4 x 2
    A1 = sigmoid_mat(Z1)                # 4 x 2
    Z2 = add_bias(matmul(A1, W2), b2)   # 4 x 1
    A2 = sigmoid_mat(Z2)                # 4 x 1

    # simple MSE gradient for demo: dA2 = (2/m) (A2 - Y)
    dA2 = scale(sub(A2, Y), 2.0/m)

    # backprop output
    dZ2 = elemwise(dA2, dsigmoid_mat(A2))          # 4 x 1
    dW2 = matmul(transpose(A1), dZ2)               # 2 x 1
    db2 = [v/m for v in sum_rows(dZ2)]             # 1

    # backprop hidden
    dA1 = matmul(dZ2, transpose(W2))               # 4 x 2
    dZ1 = elemwise(dA1, dsigmoid_mat(A1))          # 4 x 2
    dW1 = matmul(transpose(X), dZ1)                # 2 x 2
    db1 = [v/m for v in sum_rows(dZ1)]             # 2

    # update
    W2 = sub(W2, scale(dW2, lr))
    b2 = [b2[j] - lr*db2[j] for j in range(len(b2))]
    W1 = sub(W1, scale(dW1, lr))
    b1 = [b1[j] - lr*db1[j] for j in range(len(b1))]

def predict(x):
    z1 = [x[0]*W1[0][j] + x[1]*W1[1][j] + b1[j] for j in range(2)]
    a1 = [sigmoid(z) for z in z1]
    z2 = a1[0]*W2[0][0] + a1[1]*W2[1][0] + b2[0]
    a2 = sigmoid(z2)
    return 1 if a2 >= 0.5 else 0, a2

for x, y in zip(X, Y):
    cls, prob = predict(x)
    print(x, "pred:", cls, "prob:", round(prob, 3), "true:", int(y[0]))
```

{{% hint warning %}}
This code is intentionally minimal for learning.
In practice, DFNN classification typically uses cross-entropy loss, mini-batches, and more stable initialisation/normalisation.
{{% /hint %}}

---

## Summary

- DFNN/MLP adds **hidden layers** + **non-linear activations** to learn non-linear decision boundaries.
- This makes it a solution to **XOR** (which a single perceptron cannot solve).
- Training uses **forward propagation** to compute predictions, then **backpropagation** to compute gradients.
- Weights are updated using gradients and a learning rate.
- Depth and width both increase capacity, but affect optimisation and overfitting differently.

## Reference

- Course slides: **DNN_M5_DFNN**.
- Singh & Raj — Deep Learning notes (Ch. 1–3).
- Zhang, Lipton, Li, Smola — *Dive into Deep Learning* (MLP, forward/backprop).
- **Dive into deep learning. Cambridge University Press.**. ([T1 – Ch 5](https://d2l.ai/chapter_introduction/index.html))
- Jurafsky, D., & Martin, J. H. (Third Edition) Speech and Language Processing: An Introduction to Natural Language Processing, Computational Linguistics, and Speech Recognition. (R4 - Ch 7.3, 7.5)

---

{{< home-link "Home" >}} | {{< section-index >}}
