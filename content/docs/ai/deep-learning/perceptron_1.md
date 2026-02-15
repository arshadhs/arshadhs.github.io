<!-- File: content/docs/deep-learning/perceptron.md -->
---
title: "Perceptron"
date: 2026-02-15
draft: false
weight: 220
tags: ["Deep Learning", "ANN", "Perceptron", "Classification"]
categories: ["AI", "Machine Learning"]
---

# Perceptron

A **perceptron** is the simplest artificial neuron used for **binary classification**, where the output is decided by whether a weighted sum of inputs crosses a threshold. :contentReference[oaicite:0]{index=0}

Typical use-cases: learning simple decision boundaries (like AND/OR logic) and understanding why deeper networks are needed for harder patterns. :contentReference[oaicite:1]{index=1}

{{% hint info %}}
Think of a perceptron as:
- a **single neuron**,
- doing a **linear decision** (a hyperplane boundary),
- trained using the **Perceptron Learning Algorithm (PLA)** when data is linearly separable. :contentReference[oaicite:2]{index=2}
{{% /hint %}}

## General Form

{{% hint danger %}}
**Artificial neuron (perceptron) computation**
Given features \(x \in \mathbb{R}^d\) and weights \(w \in \mathbb{R}^d\), plus bias \(b \in \mathbb{R}\):
$$
z = w^T x + b
$$

**Step (threshold) activation for binary output**
Using labels in \(\{0,1\}\):
$$
\hat{y} =
\begin{cases}
1 & \text{if } z \ge 0\\
0 & \text{if } z < 0
\end{cases}
$$

(Equivalently, with labels in \(\{-1,+1\}\) you often write \(\hat{y}=\operatorname{sign}(z)\).)
{{% /hint %}}

{{% hint danger %}}
**Decision boundary**
The boundary is where the neuron is exactly “on the fence”:
$$
w^T x + b = 0
$$
In 2D, this is a line; in 3D, a plane; in \(d\) dimensions, a hyperplane.
{{% /hint %}}

## Properties

### Linear separability is the key condition
A perceptron can correctly classify a dataset only if the two classes can be separated by a hyperplane (i.e., the data is **linearly separable**). :contentReference[oaicite:3]{index=3}

{{% hint info %}}
The perceptron is powerful for *linear* decision problems (many logic gates), but it fails on patterns like **XOR** which are **not** linearly separable. :contentReference[oaicite:4]{index=4}
{{% /hint %}}

### Logic gates: what it can and cannot represent
- Can represent **AND**, **OR**, **NAND**, **NOR**, **NOT** using suitable weights and bias. :contentReference[oaicite:5]{index=5}  
- Cannot represent **XOR** with a single perceptron. :contentReference[oaicite:6]{index=6}

### Connectionism perspective
Neural networks are “connectionist machines”: knowledge is stored in **connection weights**, and learning modifies those connection strengths. :contentReference[oaicite:7]{index=7}

## Intuition

### Biological vs artificial neuron (high-level intuition)
The course frames artificial neurons as a simplified, mathematical abstraction inspired by the brain’s interconnected neurons and their weighted connections. :contentReference[oaicite:8]{index=8}

### What the perceptron is “doing”
- It forms a **weighted vote** across features.
- The bias shifts the boundary left/right (or up/down).
- The step activation turns a continuous score into a hard class decision.

### Perceptron as a geometry tool
Training the perceptron is essentially searching for a separating line/plane that places one class on one side and the other class on the other side. :contentReference[oaicite:9]{index=9}

## ML relevance

### Where the perceptron sits in modern ML
- It is the conceptual building block for **multi-layer perceptrons (MLPs)**.
- It introduces the full pipeline: data → model → objective idea → learning rule → decision boundary. :contentReference[oaicite:10]{index=10}

### Why XOR matters
XOR is the clean demonstration that **linear models are limited**, which motivates adding **hidden layers** (moving from a single perceptron to an MLP). :contentReference[oaicite:11]{index=11}

{{% hint info %}}
If your data is not linearly separable, you typically need:
- feature transforms, or
- multiple layers (hidden units), or
- a different model family.
The perceptron story is the “why” behind depth.
{{% /hint %}}

## Summary
- A perceptron is the **simplest artificial neuron** for **binary classification**. :contentReference[oaicite:12]{index=12}
- It computes a weighted sum and applies a **threshold** to decide the class.
- It learns a **linear decision boundary** and works well only for **linearly separable** data. :contentReference[oaicite:13]{index=13}
- It can model many logic gates (AND/OR/etc.) but **fails on XOR**, motivating hidden layers. :contentReference[oaicite:14]{index=14}

## Reference
- Course deck: **AIML Module 2 — ANN Perceptron**. :contentReference[oaicite:15]{index=15}
- Rosenblatt, F. (1958) — *The Perceptron: A Probabilistic Model*.
- Goodfellow, Bengio, Courville — *Deep Learning* (Ch. 1–6).
- Zhang et al. — *Dive into Deep Learning* (Intro/linear models).

---

{{< home-link "Home" >}} | {{< section-index >}}