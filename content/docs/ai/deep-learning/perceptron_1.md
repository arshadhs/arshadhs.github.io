---
title: "Perceptron"
date: 2026-02-15
draft: false
weight: 220
tags: ["Deep Learning", "ANN", "Perceptron", "Classification"]
categories: ["AI", "Machine Learning"]
---

# Perceptron

A **perceptron** is the simplest artificial neuron used for **binary classification**, where the output is decided by whether a weighted sum of inputs crosses a threshold.

Typical use-cases: learning simple decision boundaries (like AND/OR logic) and understanding why deeper networks are needed for harder patterns.

{{% hint info %}}
Think of a perceptron as:
- a **single neuron**,
- doing a **linear decision** (a hyperplane boundary),
- trained using the **Perceptron Learning Algorithm (PLA)** when data is linearly separable.
{{% /hint %}}

## General Form

{{% hint danger %}}
**Artificial neuron (perceptron) computation**  
Given features {{< katex >}}x \in \mathbb{R}^d{{< /katex >}} and weights {{< katex >}}w \in \mathbb{R}^d{{< /katex >}}, plus bias {{< katex >}}b \in \mathbb{R}{{< /katex >}}:

{{< katex display=true >}}
z = w^T x + b
{{< /katex >}}

**Step (threshold) activation for binary output**  
Using labels in {{< katex >}}\{0,1\}{{< /katex >}}:

{{< katex display=true >}}
\hat{y} =
\begin{cases}
1 & \text{if } z \ge 0\\
0 & \text{if } z < 0
\end{cases}
{{< /katex >}}

(Equivalently, with labels in {{< katex >}}\{-1,+1\}{{< /katex >}} you often write {{< katex >}}\hat{y}=\operatorname{sign}(z){{< /katex >}}.)
{{% /hint %}}

{{% hint danger %}}
**Decision boundary**  
The boundary is where the neuron is exactly “on the fence”:

{{< katex display=true >}}
w^T x + b = 0
{{< /katex >}}

In 2D, this is a line; in 3D, a plane; in {{< katex >}}d{{< /katex >}} dimensions, a hyperplane.
{{% /hint %}}

## Properties

### Linear separability is the key condition
A perceptron can correctly classify a dataset only if the two classes can be separated by a hyperplane (i.e., the data is **linearly separable**).

{{% hint info %}}
The perceptron is powerful for *linear* decision problems (many logic gates), but it fails on patterns like **XOR** which are **not** linearly separable.
{{% /hint %}}

### Logic gates: what it can and cannot represent
- Can represent **AND**, **OR**, **NAND**, **NOR**, **NOT** using suitable weights and bias.  
- Cannot represent **XOR** with a single perceptron.

### Connectionism perspective
Neural networks are “connectionist machines”: knowledge is stored in **connection weights**, and learning modifies those connection strengths.

## Intuition

### Biological vs artificial neuron (high-level intuition)
The course frames artificial neurons as a simplified, mathematical abstraction inspired by the brain’s interconnected neurons and their weighted connections.

### What the perceptron is “doing”
- It forms a **weighted vote** across features.
- The bias shifts the boundary left/right (or up/down).
- The step activation turns a continuous score into a hard class decision.

### Perceptron as a geometry tool
Training the perceptron is essentially searching for a separating line/plane that places one class on one side and the other class on the other side.

## ML relevance

### Where the perceptron sits in modern ML
- It is the conceptual building block for **multi-layer perceptrons (MLPs)**.
- It introduces the full pipeline: data → model → objective idea → learning rule → decision boundary.

### Why XOR matters
XOR is the clean demonstration that **linear models are limited**, which motivates adding **hidden layers** (moving from a single perceptron to an MLP).

{{% hint info %}}
If your data is not linearly separable, you typically need:
- feature transforms, or
- multiple layers (hidden units), or
- a different model family.
The perceptron story is the “why” behind depth.
{{% /hint %}}

## Summary
- A perceptron is the **simplest artificial neuron** for **binary classification**.
- It computes a weighted sum and applies a **threshold** to decide the class.
- It learns a **linear decision boundary** and works well only for **linearly separable** data.
- It can model many logic gates (AND/OR/etc.) but **fails on XOR**, motivating hidden layers.

## Reference
- Course deck: **AIML Module 2 — ANN Perceptron**.
- Rosenblatt, F. (1958) — *The Perceptron: A Probabilistic Model*.
- Goodfellow, Bengio, Courville — *Deep Learning* (Ch. 1–6).
- Zhang et al. — *Dive into Deep Learning* (Intro/linear models).

---

{{< home-link "Home" >}} | {{< section-index >}}