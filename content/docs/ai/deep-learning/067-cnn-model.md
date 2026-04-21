---
title: "Convolutional Neural Networks (CNN) - Complete Guide"
draft: false
tags: ["Machine Learning", "Deep Learning", "CNN"]
categories: ["AI", "ML"]
weight: 1325
---

# Convolutional Neural Networks (CNN)

Convolutional Neural Networks are specialised neural networks designed for **grid-like data such as images**.

They exploit:
- Spatial structure
- Local connectivity
- Parameter sharing

---

# Image Representation

An image is represented as a tensor:

{{% colour "green" %}}
{{< katex display=true >}}
X \in \mathbb{R}^{H \times W \times C}
{{< /katex >}}
{{% /colour %}}

Where:
- H = height
- W = width
- C = channels

---

# Convolution Operation

A filter (kernel) slides over the image:

{{% colour "green" %}}
{{< katex display=true >}}
Z(i,j) = \sum_{m}\sum_{n} X(i+m, j+n)K(m,n)
{{< /katex >}}
{{% /colour %}}

- Produces a **feature map**
- Detects patterns like edges

---

# Stride and Padding

## Stride
Controls movement of filter

## Padding
Adds zeros to preserve size

{{% colour "green" %}}
{{< katex display=true >}}
Output = \frac{N - F + 2P}{S} + 1
{{< /katex >}}
{{% /colour %}}

---

# Activation Functions

## ReLU

{{% colour "green" %}}
{{< katex display=true >}}
ReLU(x) = max(0, x)
{{< /katex >}}
{{% /colour %}}

---

# Pooling

## Max Pooling
- Takes maximum value

## Average Pooling
- Takes mean

Reduces:
- Computation
- Overfitting

---

# Global Average Pooling (GAP)

Replaces flatten layer:

{{% colour "green" %}}
{{< katex display=true >}}
y_k = \frac{1}{HW} \sum_{i,j} x_{i,j,k}
{{< /katex >}}
{{% /colour %}}

Advantages:
- Fewer parameters
- Better generalisation

---

# CNN Architecture

```mermaid
graph LR
A[Input Image] --> B[Conv]
B --> C[ReLU]
C --> D[Pooling]
D --> E[Conv Layers]
E --> F[GAP]
F --> G[Softmax]
```

---

# Loss Function (Classification)

## Cross Entropy

{{% colour "green" %}}
{{< katex display=true >}}
L = - \sum y \log(\hat{y})
{{< /katex >}}
{{% /colour %}}

---

# Backpropagation in CNN

Gradients computed via chain rule:

{{% colour "green" %}}
{{< katex display=true >}}
\frac{\partial L}{\partial W}
{{< /katex >}}
{{% /colour %}}

---

# Why CNN Works

- Local feature detection
- Translation invariance
- Hierarchical learning

---

# Deep CNN Architectures

## VGG
- Stacked 3x3 convolutions

## ResNet
- Skip connections

{{% colour "green" %}}
{{< katex display=true >}}
y = F(x) + x
{{< /katex >}}
{{% /colour %}}

## Inception
- Parallel convolutions

---

# Training Pipeline

```mermaid
graph LR
A[Input] --> B[Forward Pass]
B --> C[Loss]
C --> D[Backprop]
D --> E[Update Weights]
```

---

# Summary

- CNNs process images using convolution
- Use pooling for reduction
- GAP replaces dense layers
- Deep architectures improve performance

---

{{< home-link "Home" >}} | {{< section-index >}}
