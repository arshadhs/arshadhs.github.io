---
title: "Gradient Descent and Mini-Batch Gradient Descent"
draft: false
tags: ["AI", "Deep Learning", "DNN", "Gradient Descent", "Mini-Batch Gradient Descent"]
categories: ["AI", "Deep Learning"]
weight: 1010
menu: main
---

# Optimisation: Gradient Descent and Mini-Batch Gradient Descent

Gradient descent is the core optimisation idea behind neural network training.
It updates the model parameters by moving in the opposite direction of the gradient of the loss.

{{% hint info %}}
**Key takeaway:**  
Gradient descent uses the gradient to decide how to change the parameters.
The learning rate controls how large each update step is.
{{% /hint %}}

---

{{< mermaid >}}
flowchart TD
    A["Gradient Descent Variants"] --> B["Batch Gradient Descent"]
    A --> C["Stochastic Gradient Descent"]
    A --> D["Mini-batch Gradient Descent"]

    B --> B1["Uses full dataset"]
    B --> B2["One update per epoch"]
    B --> B3["Smooth but slow"]

    C --> C1["Uses one example at a time"]
    C --> C2["Frequent updates"]
    C --> C3["Fast but noisy"]

    D --> D1["Uses small batches"]
    D --> D2["Efficient on hardware"]
    D --> D3["Balanced and practical"]

    style A fill:#E1F5FE,stroke:#4A90E2,stroke-width:2px
    style B fill:#EDE7F6,stroke:#7E57C2
    style C fill:#C8E6C9,stroke:#43A047
    style D fill:#FFF9C4,stroke:#FBC02D
{{< /mermaid >}}

---
	
## Gradient Descent Rule ☆

The gradient tells us the direction in which the loss increases fastest.
To reduce the loss, we move in the opposite direction.

{{% colour "blue" %}}
{{< katex display=true >}}
\theta_{t+1} = \theta_t - \eta \nabla \mathcal{L}(\theta_t)
{{< /katex >}}
{{% /colour %}}

Where:

| Symbol | Meaning |
|---|---|
| {{< katex >}} \theta_t {{< /katex >}} | parameters at iteration {{< katex >}} t {{< /katex >}} |
| {{< katex >}} \eta {{< /katex >}} | learning rate |
| {{< katex >}} \nabla \mathcal{L}(\theta_t) {{< /katex >}} | gradient of the loss |
| {{< katex >}} \theta_{t+1} {{< /katex >}} | updated parameters |

## One Iteration Numerical Example ☆

Consider the simple function:

{{% colour "blue" %}}
{{< katex display=true >}}
f(x,y) = x^2 + y^2
{{< /katex >}}
{{% /colour %}}

The gradient is:

{{% colour "blue" %}}
{{< katex display=true >}}
\nabla f = [2x, 2y]
{{< /katex >}}
{{% /colour %}}

Start at:

{{% colour "blue" %}}
{{< katex display=true >}}
(x_0, y_0) = (3,4), \qquad \eta = 0.1
{{< /katex >}}
{{% /colour %}}

Before update:

| Quantity | Value |
|---|---|
| Position | {{< katex >}} (3,4) {{< /katex >}} |
| Loss | {{< katex >}} 3^2 + 4^2 = 25 {{< /katex >}} |
| Gradient | {{< katex >}} [6,8] {{< /katex >}} |

Update:

{{% colour "blue" %}}
{{< katex display=true >}}
x_1 = 3 - 0.1(6) = 2.4
{{< /katex >}}
{{% /colour %}}

{{% colour "blue" %}}
{{< katex display=true >}}
y_1 = 4 - 0.1(8) = 3.2
{{< /katex >}}
{{% /colour %}}

New loss:

{{% colour "blue" %}}
{{< katex display=true >}}
f(2.4,3.2) = 2.4^2 + 3.2^2 = 16
{{< /katex >}}
{{% /colour %}}

The loss decreases from {{< katex >}} 25 {{< /katex >}} to {{< katex >}} 16 {{< /katex >}}.
This is a {{< katex >}} 36\% {{< /katex >}} reduction in one step.

## Batch GD, SGD, and Mini-Batch GD ☆

Gradient descent can use different amounts of data per update.

| Method | Gradient Uses | Updates per Epoch | Memory | Convergence |
|---|---|---:|---|---|
| Batch Gradient Descent | all examples | {{< katex >}} 1 {{< /katex >}} | high | smooth but slow |
| Stochastic Gradient Descent | one example | {{< katex >}} N {{< /katex >}} | low | noisy but fast |
| Mini-Batch Gradient Descent | small batch | {{< katex >}} N/B {{< /katex >}} | medium | balanced |

For a dataset with {{< katex >}} N = 10000 {{< /katex >}} samples and batch size {{< katex >}} B = 32 {{< /katex >}}:

| Method | Updates per Epoch |
|---|---:|
| Batch GD | {{< katex >}} 1 {{< /katex >}} |
| SGD | {{< katex >}} 10000 {{< /katex >}} |
| Mini-batch GD | {{< katex >}} 10000 / 32 \approx 313 {{< /katex >}} |

{{% hint success %}}
Mini-batch gradient descent is the common practical choice for deep learning.
It balances GPU efficiency, convergence stability, and update frequency.
{{% /hint %}}

## Mini-Batch Gradient Descent Algorithm ☆

```mermaid
flowchart TD
    A[Initialise parameters] --> B[Shuffle training data]
    B --> C[Choose mini-batch]
    C --> D[Compute mini-batch loss]
    D --> E[Compute mini-batch gradient]
    E --> F[Update parameters]
    F --> G{Stopping criterion met?}
    G -- No --> B
    G -- Yes --> H[Return trained parameters]

    style A fill:#E1F5FE
    style B fill:#C8E6C9
    style C fill:#FFF9C4
    style D fill:#EDE7F6
    style E fill:#E1F5FE
    style F fill:#C8E6C9
    style G fill:#FFF9C4
    style H fill:#C8E6C9
```

For a mini-batch {{< katex >}} \mathcal{B} {{< /katex >}} of size {{< katex >}} B {{< /katex >}}, the mini-batch loss is:

{{% colour "blue" %}}
{{< katex display=true >}}
\mathcal{L}(\theta) = \frac{1}{B}\sum_{i \in \mathcal{B}} \mathcal{L}_i(\theta)
{{< /katex >}}
{{% /colour %}}

The gradient is:

{{% colour "blue" %}}
{{< katex display=true >}}
g = \nabla_\theta \mathcal{L}(\theta) = \frac{1}{B}\sum_{i \in \mathcal{B}} \nabla \mathcal{L}_i(\theta)
{{< /katex >}}
{{% /colour %}}

The parameter update is:

{{% colour "blue" %}}
{{< katex display=true >}}
\theta \leftarrow \theta - \eta g
{{< /katex >}}
{{% /colour %}}

## Toy Problem for Optimisation Examples ☆

A useful toy loss function is:

{{% colour "blue" %}}
{{< katex display=true >}}
\mathcal{L}(w_1,w_2) = w_1^2 + 4w_2^2
{{< /katex >}}
{{% /colour %}}

The gradient is:

{{% colour "blue" %}}
{{< katex display=true >}}
\nabla \mathcal{L} = [2w_1, 8w_2]
{{< /katex >}}
{{% /colour %}}

This creates an elongated bowl.
The {{< katex >}} w_2 {{< /katex >}} direction is four times steeper than the {{< katex >}} w_1 {{< /katex >}} direction.
This causes ordinary gradient descent to move too aggressively in the steep direction and more slowly in the flatter direction.

## Mini-Batch GD Numerical Example ☆

Problem setup:

{{% colour "blue" %}}
{{< katex display=true >}}
\mathcal{L}(w_1,w_2) = w_1^2 + 4w_2^2, \qquad (w_1,w_2) = (4,2), \qquad \eta = 0.1
{{< /katex >}}
{{% /colour %}}

| Iteration | Loss | Gradient {{< katex >}} [\nabla_{w_1}, \nabla_{w_2}] {{< /katex >}} | New {{< katex >}} w_1 {{< /katex >}} | New {{< katex >}} w_2 {{< /katex >}} |
|---:|---:|---:|---:|---:|
| 0 | 32.00 | {{< katex >}} [8,16] {{< /katex >}} | {{< katex >}} 3.20 {{< /katex >}} | {{< katex >}} 0.40 {{< /katex >}} |
| 1 | 10.88 | {{< katex >}} [6.4,3.2] {{< /katex >}} | {{< katex >}} 2.56 {{< /katex >}} | {{< katex >}} 0.08 {{< /katex >}} |
| 2 | 6.58 | {{< katex >}} [5.1,0.64] {{< /katex >}} | {{< katex >}} 2.05 {{< /katex >}} | {{< katex >}} 0.016 {{< /katex >}} |
| 3 | 4.21 | {{< katex >}} [4.1,0.13] {{< /katex >}} | {{< katex >}} 1.64 {{< /katex >}} | {{< katex >}} 0.003 {{< /katex >}} |

Observation:

The loss decreases:

{{% colour "blue" %}}
{{< katex display=true >}}
32 \rightarrow 10.88 \rightarrow 6.58 \rightarrow 4.21
{{< /katex >}}
{{% /colour %}}

However, {{< katex >}} w_2 {{< /katex >}} moves much more aggressively than {{< katex >}} w_1 {{< /katex >}} because its gradient is steeper.
This shows why adaptive methods and momentum can help.

## Learning Rate Effects ☆

The learning rate controls step size.

| Learning Rate | Behaviour |
|---|---|
| Too large | oscillation, unstable updates, possible divergence |
| Too small | very slow progress, may take too long to train |
| Just right | smooth and fast convergence |

```mermaid
flowchart LR
    A[Too Large] --> B[Oscillation or Divergence]
    C[Too Small] --> D[Very Slow Training]
    E[Good Value] --> F[Smooth Descent]

    style A fill:#FFCDD2
    style B fill:#FFCDD2
    style C fill:#FFF9C4
    style D fill:#FFF9C4
    style E fill:#C8E6C9
    style F fill:#C8E6C9
```

{{% hint warning %}}
A high learning rate can make the loss jump around.
A very low learning rate may look stable, but training may be too slow to be useful.
{{% /hint %}}

## Why Adaptive Techniques Are Needed ☆

A fixed learning rate applies the same step size to every parameter.
This can be inefficient because:

- different parameters may need different step sizes;
- some directions in the loss landscape are steeper than others;
- sparse features may need larger updates;
- dense features may need smaller, more careful updates;
- elongated contours can cause zigzag movement.

## Exam Notes ☆

Remember these points:

- Gradient descent moves opposite to the gradient.
- The learning rate controls the update size.
- Mini-batch GD is commonly used in deep learning.
- Batch GD is smooth but computationally heavy.
- SGD is noisy but gives many updates.
- Mini-batch GD is a practical compromise.
- A learning rate that is too large can cause divergence.
- A learning rate that is too small can make training very slow.
- Steep directions can cause unbalanced updates.

---
{{< home-link "Home" >}} | {{< section-index >}}
