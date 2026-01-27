---
title: 'Perceptron'
draft: false
tags: ["AI", "ML", "Neural Networks"]
categories: ["AI", "ML"]
weight: 412
menu: main
---

# Perceptron

A **Perceptron** is the **simplest form of an artificial neural network (ANN)**.  
It is an algorithm for **supervised learning** used mainly for **binary classification** problems.

It forms the **basic building block** of modern neural networks and deep learning models.

---

## What a Perceptron Does

A perceptron:

- Takes **multiple real-valued inputs**
- Assigns a **weight** to each input
- Computes a **weighted sum**
- Adds a **bias**
- Applies an **activation function**
- Produces a **binary output**

In simple terms, it decides between **two classes**.

---

## Mathematical Model

{{< katex display=true >}}
\hat{y} = f\left(\sum_{i=1}^{n} w_i x_i + b\right)
{{< /katex >}}

Where:
- \(x_i\) → input features  
- \(w_i\) → weights  
- \(b\) → bias  
- {{< katex display=true >}}f(\cdot){{< /katex >}} → activation function  

---

## Perceptron Architecture

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

## Core Components

### 1. Inputs (x_1, x_2, ... , x_n)

Inputs are the **features** or measurable attributes of a data point.

Example (OR gate):

{{< katex display=true >}}
(x_1, x_2) \in \{0,1\}^2
{{< /katex >}}

Inputs by themselves have **no influence** unless multiplied by weights.

---

### 2. Weights (w_1, w_2, ... , w_n)

- Weights determine **how strongly each input influences the output**
- Larger weights → higher importance
- Learned during training
- Act as **importance scores** for features

---

### 3. Bias ((b)

The bias is a constant added to the weighted sum.

- Shifts the **decision boundary**
- Allows classification even when all inputs are zero
- Prevents the boundary from being forced through the origin

**Geometric intuition:**
- Weights **tilt** the decision line
- Bias **shifts** the line

---

### 4. Net Input (Weighted Sum)

{{< katex display=true >}}
z = \sum_{i=1}^{n} w_i x_i + b
{{< /katex >}}

This value determines whether the perceptron activates.

---

### 5. Activation Function (Step Function)

The classic perceptron uses a **step function**:

{{< katex display=true >}}
\hat{y} =
\begin{cases}
1 & \text{if } z \ge 0 \\
0 & \text{otherwise}
\end{cases}
{{< /katex >}}

- Output is **binary**
- Decision boundary is **linear**
- Suitable only for **linearly separable data**

{{% hint warning %}}
A single perceptron **cannot** solve problems like XOR because they are not linearly separable.
{{% /hint %}}

---

## Why the Perceptron Is Limited

- Uses a **linear decision boundary**
- Can only solve **linearly separable** problems
- Cannot model complex, non-linear relationships

This limitation led to **multi-layer neural networks**.

---

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

## Key Takeaway

- A **perceptron** models a straight decision line
- **Neural networks** combine many perceptrons
- Non-linear activations allow complex decision boundaries
- Deep learning is built by stacking perceptrons

---

## Further Reading

- [Perceptron – GeeksforGeeks](https://www.geeksforgeeks.org/machine-learning/what-is-perceptron-the-simplest-artificial-neural-network/)

---

{{< home-link "Home" >}} | {{< section-index >}}
