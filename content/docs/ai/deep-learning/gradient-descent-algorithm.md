---
title: "Gradient Descent Algorithm"
date: 2026-02-26
draft: false
weight: 335
tags: ["Deep Learning", "Optimisation"]
categories: ["AI", "Machine Learning"]
---

# Gradient Descent Algorithm

Gradient Descent Algorithm (GDA) is 
- an **optimisation method**
- used to **train models** 
- by repeatedly updating parameters (weights and biases) to **reduce the loss**

{{% hint info %}}
In deep learning, the default training approach is almost always **mini-batch gradient descent**, usually with **Adam** or **SGD + momentum**.
{{% /hint %}}

---

## What gradient descent does

You start with some parameters, measure the loss, and update parameters to reduce that loss.

Update rule (conceptual):

{{< colour "green" >}}
{{< katex display=true >}}
\theta \leftarrow \theta - \eta \nabla_{\theta} J(\theta)
{{< /katex >}}
{{< /colour >}}

Where:
- {{< katex >}}\theta{{< /katex >}} = parameters (weights/biases)
- {{< katex >}}J(\theta){{< /katex >}} = loss (objective)
- {{< katex >}}\nabla_{\theta} J(\theta){{< /katex >}} = gradient (direction of steepest increase)
- {{< katex >}}\eta{{< /katex >}} = learning rate (step size)

---

## Types of Gradient Descent

There are three common “data usage” types:

- **Batch Gradient Descent**: uses the entire dataset per update
- **Stochastic Gradient Descent (SGD)**: uses one example per update
- **Mini-batch Gradient Descent**: uses a small batch per update (the deep learning standard)

{{< mermaid >}}
flowchart TD
  A["Gradient Descent<br/>Algorithm (GDA)"] --> B["Batch GD<br/>(full dataset)"]
  A --> C["Stochastic GD<br/>(one sample)"]
  A --> D["Mini-batch GD<br/>(small batch)"]

  D --> E["Often paired with:<br/>Momentum / Adam"]

  %% Pastel colour scheme
  style A fill:#E3F2FD,stroke:#1E88E5,stroke-width:1px
  style B fill:#FFF3E0,stroke:#FB8C00,stroke-width:1px
  style C fill:#FCE4EC,stroke:#D81B60,stroke-width:1px
  style D fill:#E8F5E9,stroke:#43A047,stroke-width:1px
  style E fill:#F3E5F5,stroke:#8E24AA,stroke-width:1px
{{< /mermaid >}}

---

## Batch Gradient Descent

**How it works:** one update uses all {{< katex >}}N{{< /katex >}} training examples.

Pros:
- stable, smooth loss curve
- good for small datasets

Cons:
- slow when {{< katex >}}N{{< /katex >}} is large
- one update can be expensive

---

## Stochastic Gradient Descent (SGD)

**How it works:** one update uses a single training example.

Pros:
- very fast updates
- can “jitter” out of shallow traps

Cons:
- noisy updates (loss may bounce)
- can require more iterations to settle

---

## Mini-batch Gradient Descent

Mini-batch GD is the **most widely used** training method in deep learning.

**How it works:** each update uses a small batch of size {{< katex >}}B{{< /katex >}} (often 32–512).

Why it is popular:
- runs efficiently on GPUs (matrix operations)
- less noisy than SGD
- much cheaper per update than full-batch
- best practical balance of speed and stability

{{% hint info %}}
If you remember one thing:
**Mini-batch GD is the default for deep learning** because it scales well with data and compute.
{{% /hint %}}

### Mini-batch update (same idea, just different data)

You compute the gradient using only the mini-batch:

{{< colour "green" >}}
{{< katex display=true >}}
\theta \leftarrow \theta - \eta \nabla_{\theta} J_{\text{batch}}(\theta)
{{< /katex >}}
{{< /colour >}}

---

## Popular “upgrades” used with mini-batches

These are still gradient descent, but with smarter updates.

### Momentum (very common with SGD)

Momentum keeps a running “velocity” so updates build up in consistent directions.

{{< colour "green" >}}
{{< katex display=true >}}
v_t = \beta v_{t-1} + (1-\beta) g_t
{{< /katex >}}
{{< /colour >}}

{{< colour "green" >}}
{{< katex display=true >}}
\theta_{t+1} = \theta_t - \eta v_t
{{< /katex >}}
{{< /colour >}}

Where {{< katex >}}g_t{{< /katex >}} is the mini-batch gradient and {{< katex >}}\beta{{< /katex >}} controls smoothing.

### Adam (most popular “default” optimiser)

Adam adapts the learning rate per parameter using running averages of:
- gradients (first moment)
- squared gradients (second moment)

Core idea:
- parameters that consistently see large gradients get normalised
- parameters with small gradients can still move meaningfully

{{% hint info %}}
In practice, Adam is often the quickest way to get a model training well with minimal tuning, which is why it is so commonly used as a default.
{{% /hint %}}

(Implementation details vary by framework, but conceptually Adam = momentum + adaptive step sizes.)

---

## Which one should you use?

- **Learning / small toy datasets:** Batch GD is easy to understand.
- **Deep learning / real training:** Mini-batch GD is the standard.
- **Default optimiser choice:** Adam (or SGD + momentum when you want strong generalisation and can tune schedules).

---

## Summary

- GDA minimises loss by updating parameters using gradients.
- Types: Batch GD, SGD, Mini-batch GD.
- **Most popular in deep learning:** Mini-batch GD, usually with **Adam** or **SGD + momentum**.
- Learning rate and batch size strongly affect speed and stability.

---

## Reference

- DNN lecture notes: optimisation content across regression/classification training slides.
- Singh & Raj — *Deep Learning* (training and optimisation fundamentals).
- Zhang, Lipton, Li, Smola — *Dive into Deep Learning* (optimisation chapters).

- [GDA in ML](https://www.geeksforgeeks.org/machine-learning/gradient-descent-algorithm-and-its-variants/)
- [Different Variants of Gradient Descent](https://www.geeksforgeeks.org/machine-learning/different-variants-of-gradient-descent/)
- [Mastering Gradient Descent: A Comprehensive Guide with Real-World Applications](https://medium.com/@danailkhan1999/gradient-descent-explained-from-theory-to-practice-in-machine-learning-b00840d0ba0e)

---

{{< home-link "Home" >}} | {{< section-index >}}
