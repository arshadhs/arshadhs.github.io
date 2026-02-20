---
title: 'Artificial Neuron and Perceptron'
draft: false
tags: ["AI", "ML", "Neural Networks"]
categories: ["AI", "ML"]
weight: 200
menu: main
---

# Artificial Neuron and Perceptron

{{% hint info %}}
knowledge in neural networks is stored in **connection weights**, and learning means **modifying those weights**.
{{% /hint %}}

---

## Biological Neuron

A biological neuron is a specialised cell that processes and transmits information through electrical and chemical signals.

Core components:
- **Dendrites**: receive signals from other neurons
- **Cell body (soma)**: processes incoming signals
- **Axon**: transmits the output signal
- **Synapses**: connection points between neurons

Biological intuition:
- many inputs arrive to one neuron
- one neuron can connect out to many neurons
- massive parallelism enables fast perception and recognition

---

## Artificial Neuron

An artificial neuron is a simplified computational model inspired by biological neurons.

What it does:
- receives multiple input features
- assigns an importance (weight) to each feature
- computes a weighted sum + bias
- applies an activation function
- produces an output

{{< color "red" >}}
{{< katex display=true >}}
z = \sum_{i=1}^{n} w_i x_i + b
{{< /katex >}}
{{< /color >}}

{{< color "red" >}}
{{< katex display=true >}}
\hat{y} = f(z)
{{< /katex >}}
{{< /color >}}

Where:
- inputs: {{< katex >}}x_1, x_2, \dots, x_n{{< /katex >}}
- weights: {{< katex >}}w_1, w_2, \dots, w_n{{< /katex >}}
- bias: {{< katex >}}b{{< /katex >}}
- activation: {{< katex >}}f(\cdot){{< /katex >}}

---

## Connectionism Model

**Connectionism** is the view that intelligence emerges from:
- many simple processing units (neurons)
- connected together in a network
- where learning changes the **strength of connections** (weights)

{{% hint info %}}
Connectionist principle:
- intelligence emerges from simple units
- knowledge is stored in weights
- learning modifies weights
- computation can be parallel and distributed
{{% /hint %}}

---

## Perceptron

A **Perceptron** is the **simplest form of an artificial neural network (ANN)**.  
It is an algorithm for **supervised learning** used mainly for **binary classification** problems.

It forms the **basic building block** of modern neural networks and deep learning models.

Typical use-cases:
- learning simple decision boundaries (like AND/OR logic)
- understanding why deeper networks are needed for harder patterns

{{% hint info %}}
Think of a perceptron as:
- a **single neuron**
- doing a **linear decision** (a hyperplane boundary)
- trained using the **Perceptron Learning Algorithm (PLA)** when data is linearly separable
{{% /hint %}}

---

### What a Perceptron Does

A perceptron:
- takes **multiple real-valued inputs**
- assigns a **weight** to each input
- computes a **weighted sum**
- adds a **bias**
- applies an **activation function**
- produces a **binary output**

In simple terms, it decides between **two classes**.

---

### Properties

1. Linear separability is the key condition

A perceptron can correctly classify a dataset only if the two classes can be separated by a hyperplane (i.e., the data is **linearly separable**).

{{% hint info %}}
The perceptron is powerful for *linear* decision problems (many logic gates), but it fails on patterns like **XOR** which are **not** linearly separable.
{{% /hint %}}

2. Logic gates: what it can and cannot represent
- Can represent **AND**, **OR**, **NAND**, **NOR**, **NOT** using suitable weights and bias.
- Cannot represent **XOR** with a single perceptron.

---

### Mathematical Model

{{< color "red" >}}
{{< katex display=true >}}
\hat{y} = f\left(\sum_{i=1}^{n} w_i x_i + b\right)
{{< /katex >}}
{{< /color >}}

Where:
- {{< katex >}}x_i{{< /katex >}} → input features
- {{< katex >}}w_i{{< /katex >}} → weights
- {{< katex >}}b{{< /katex >}} → bias
- {{< katex >}}f(\cdot){{< /katex >}} → activation function

### Decision boundary

The boundary is where the neuron is exactly “on the fence”:

{{< color "red" >}}
{{< katex display=true >}}
w^T x + b = 0
{{< /katex >}}
{{< /color >}}

In 2D, this is a line; in 3D, a plane; in {{< katex >}}d{{< /katex >}} dimensions, a hyperplane.

### Perceptron as a geometry tool

Training the perceptron is essentially searching for a separating line/plane that places one class on one side and the other class on the other side.

---

### Perceptron Architecture

{{< mermaid >}}
flowchart LR
    X1[x₁]
    X2[x₂]
    X3[x₃]

    SUM((Σ))
    ACT[Activation Function]
    Y((ŷ))

    X1 -- w₁ --> SUM
    X2 -- w₂ --> SUM
    X3 -- w₃ --> SUM

    SUM -- BIAS(+b) --> ACT -- Output --> Y
{{< /mermaid >}}

---

### Core Components

#### 1. Inputs (x_1, x_2, ... , x_n)

Inputs are the **features** or measurable attributes of a data point.

Example (OR gate):

{{< color "red" >}}
{{< katex display=true >}}
(x_1, x_2) \in \{0,1\}^2
{{< /katex >}}
{{< /color >}}

Inputs by themselves have **no influence** unless multiplied by weights.

---

#### 2. Weights (w_1, w_2, ... , w_n)

- Weights determine **how strongly each input influences the output**
- Larger weights → higher importance
- Learned during training
- Act as **importance scores** for features

---

#### 3. Bias (b)

The bias is a constant added to the weighted sum.

- Shifts the **decision boundary**
- Allows classification even when all inputs are zero
- Prevents the boundary from being forced through the origin

**Geometric intuition:**
- Weights **tilt** the decision line
- Bias **shifts** the line

---

#### 4. Net Input (Weighted Sum)

{{< color "red" >}}
{{< katex display=true >}}
z = \sum_{i=1}^{n} w_i x_i + b
{{< /katex >}}
{{< /color >}}

This value determines whether the perceptron activates.

---

#### 5. Activation Function (Step Function)

The classic perceptron uses a **step function**:

{{< color "red" >}}
{{< katex display=true >}}
\hat{y} =
\begin{cases}
1 & \text{if } z \ge 0 \\
0 & \text{otherwise}
\end{cases}
{{< /katex >}}
{{< /color >}}

- Output is **binary**
- Decision boundary is **linear**
- Suitable only for **linearly separable data**

{{% hint warning %}}
A single perceptron **cannot** solve problems like XOR because they are not linearly separable.
{{% /hint %}}

---

### Why the Perceptron Is Limited

- Uses a **linear decision boundary**
- Can only solve **linearly separable** problems
- Cannot model complex, non-linear relationships

This limitation led to **multi-layer neural networks**.

---

### Perceptron Learning Algorithm for logic gates

Goal:
learn parameters (weights and bias) so that predicted output matches the target.

Training ingredients:
- **Data**: inputs {{< katex >}}x{{< /katex >}} and targets {{< katex >}}t{{< /katex >}}
- **Model**: perceptron
- **Objective**: reduce mismatch between {{< katex >}}t{{< /katex >}} and {{< katex >}}\hat{y}{{< /katex >}}
- **Learning algorithm**: Perceptron Learning Algorithm (PLA)

Update intuition:
- if prediction is wrong, move the boundary to correct it.

A common PLA rule (for targets {{< katex >}}t \in \{-1,+1\}{{< /katex >}}):

{{< color "red" >}}
{{< katex display=true >}}
\text{If } \hat{y} \ne t:\quad w \leftarrow w + \eta\, t\, x,\qquad b \leftarrow b + \eta\, t
{{< /katex >}}
{{< /color >}}

Where:
- {{< katex >}}\eta > 0{{< /katex >}} is the learning rate.

{{% hint info %}}
PLA converges (finds a separating hyperplane) **only if** the dataset is linearly separable.
{{% /hint %}}

---

### Perceptron for NOT, AND, OR gates

We can model simple Boolean logic using a perceptron by choosing weights and bias.

#### NOT gate
One input {{< katex >}}x \in \{0,1\}{{< /katex >}} and output should flip the value.

Example parameters (one workable choice):
- weight negative, bias positive

#### AND gate
Truth table:
- {{< katex >}}(0,0)\mapsto 0{{< /katex >}}
- {{< katex >}}(0,1)\mapsto 0{{< /katex >}}
- {{< katex >}}(1,0)\mapsto 0{{< /katex >}}
- {{< katex >}}(1,1)\mapsto 1{{< /katex >}}

Geometric meaning:
- only the point {{< katex >}}(1,1){{< /katex >}} should fall on the “positive” side of the decision boundary.

#### OR gate
Truth table:
- {{< katex >}}(0,0)\mapsto 0{{< /katex >}}
- {{< katex >}}(0,1)\mapsto 1{{< /katex >}}
- {{< katex >}}(1,0)\mapsto 1{{< /katex >}}
- {{< katex >}}(1,1)\mapsto 1{{< /katex >}}

Geometric meaning:
- only {{< katex >}}(0,0){{< /katex >}} should fall on the “negative” side.

{{% hint info %}}
AND and OR are **linearly separable**, so a single perceptron can represent them.
{{% /hint %}}

---

### Perceptron for linearly separable data

Definition (course view):
two classes are linearly separable if there exists a hyperplane that separates all positive examples from all negative examples.

In 2D:
- there exists a straight line that separates the two classes.

In higher dimensions:
- there exists an {{< katex >}}(n-1){{< /katex >}} dimensional hyperplane separating classes in {{< katex >}}n{{< /katex >}} dimensions.

---

## XOR Problem: perceptron fails on non-linearly separable data

XOR truth table:
- {{< katex >}}(0,0)\mapsto 0{{< /katex >}}
- {{< katex >}}(0,1)\mapsto 1{{< /katex >}}
- {{< katex >}}(1,0)\mapsto 1{{< /katex >}}
- {{< katex >}}(1,1)\mapsto 0{{< /katex >}}

Why a single perceptron fails:
- there is **no single straight line** (hyperplane) that separates the 1s from the 0s in XOR.

{{% hint warning %}}
XOR is the clean demonstration that **linear models are limited**.  
To solve XOR, you need **non-linearity**, typically via **hidden layers** (an MLP).
{{% /hint %}}

---

## Code: AND, OR, XOR from scratch

Below is a minimal “from scratch” perceptron (no ML libraries) that:
- learns AND / OR (works)
- fails to learn XOR (does not converge to perfect accuracy)

```python
# Perceptron from scratch for logic gates (AND, OR, XOR)
# Labels use {0,1}. We implement a simple PLA-style update.

def step(z):
    return 1 if z >= 0 else 0

def predict(x, w, b):
    # x is a list/tuple of features
    z = sum(wi * xi for wi, xi in zip(w, x)) + b
    return step(z)

def train_perceptron(X, y, lr=0.1, epochs=50):
    # Initialise weights and bias
    w = [0.0 for _ in range(len(X[0]))]
    b = 0.0

    for _ in range(epochs):
        errors = 0
        for xi, ti in zip(X, y):
            yi = predict(xi, w, b)
            err = ti - yi
            if err != 0:
                # Update rule for {0,1} labels
                for j in range(len(w)):
                    w[j] += lr * err * xi[j]
                b += lr * err
                errors += 1
        # Early stop if perfect
        if errors == 0:
            break
    return w, b

def evaluate_gate(name, X, y, w, b):
    preds = [predict(xi, w, b) for xi in X]
    acc = sum(int(pi == ti) for pi, ti in zip(preds, y)) / len(y)
    print(f"{name}: w={w}, b={b:.3f}, preds={preds}, acc={acc:.2f}")

# Inputs for 2-input gates
X = [(0,0), (0,1), (1,0), (1,1)]

# AND gate
y_and = [0,0,0,1]
w_and, b_and = train_perceptron(X, y_and, lr=0.2, epochs=50)
evaluate_gate("AND", X, y_and, w_and, b_and)

# OR gate
y_or = [0,1,1,1]
w_or, b_or = train_perceptron(X, y_or, lr=0.2, epochs=50)
evaluate_gate("OR", X, y_or, w_or, b_or)

# XOR gate (not linearly separable)
y_xor = [0,1,1,0]
w_xor, b_xor = train_perceptron(X, y_xor, lr=0.2, epochs=200)
evaluate_gate("XOR", X, y_xor, w_xor, b_xor)

print("Note: XOR typically does not reach acc=1.00 with a single perceptron.")
```

## From Perceptron to Neural Networks

A **neural network** extends the perceptron by stacking many neurons across layers.

---

### 1. Input Layer

- Receives raw feature vector
- No computation happens

{{< katex display=true >}}
\mathbf{x} = (x_1, x_2, \dots, x_n)
{{< /katex >}}

---

### 2. Hidden Layers

Hidden layers contain multiple perceptrons that learn **intermediate representations**.

Hidden layer computation:

{{< katex display=true >}}
\mathbf{z}^{(1)} = W^{(1)}\mathbf{x} + \mathbf{b}^{(1)}
{{< /katex >}}

{{< katex display=true >}}
\mathbf{a}^{(1)} = \sigma(\mathbf{z}^{(1)})
{{< /katex >}}

Where:
- \(W^{(1)}\) → weight matrix  
- \(\mathbf{b}^{(1)}\) → bias vector  
- \(\sigma\) → non-linear activation (ReLU, Sigmoid, Tanh)

Hidden layers allow the network to learn **complex patterns**.

---

### 3. Output Layer

{{< katex display=true >}}
\mathbf{z}^{(2)} = W^{(2)}\mathbf{a}^{(1)} + \mathbf{b}^{(2)}
{{< /katex >}}

{{< katex display=true >}}
\hat{y} = \sigma(\mathbf{z}^{(2)})
{{< /katex >}}

Activation depends on the task:

- **Sigmoid** → binary classification  
- **Softmax** → multi-class classification  
- **Linear** → regression  

---

## Summary

- A **perceptron** models a straight decision line
- A perceptron is the **simplest artificial neuron** for **binary classification**
- It computes a weighted sum and applies a **threshold** to decide the class.
- It learns a **linear decision boundary** and works well only for **linearly separable** data.
- It can model many logic gates (AND/OR/etc.) but **fails on XOR**, motivating hidden layers.
- **Neural networks** combine many perceptrons
- Non-linear activations allow complex decision boundaries
- Deep learning is built by stacking perceptrons

## Reference
- **AIML Module 2 — ANN Perceptron**.
- Rosenblatt, F. (1958) — *The Perceptron: A Probabilistic Model*.
- Goodfellow, Bengio, Courville — *Deep Learning* (Ch. 1–6).
- Zhang et al. — *Dive into Deep Learning* (Intro/linear models).

- [Perceptron – GeeksforGeeks](https://www.geeksforgeeks.org/machine-learning/what-is-perceptron-the-simplest-artificial-neural-network/)

---

{{< home-link "Home" >}} | {{< section-index >}}
