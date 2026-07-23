---
title: "Momentum Methods"
draft: false
tags: ["AI", "Deep Learning", "DNN", "Momentum", "SGD with Momentum"]
categories: ["AI", "Deep Learning"]
weight: 1020
menu: main
---

# Optimisation: Momentum Methods

Momentum improves gradient descent by adding a memory of previous update directions.
Instead of using only the current gradient, the optimiser accumulates velocity across iterations.

{{% hint info %}}
**Key takeaway:**  
Momentum helps the optimiser move faster in consistent directions and reduces zigzag movement in directions where gradients oscillate.
{{% /hint %}}

---

{{< mermaid >}}
flowchart TD
    A["Momentum-based Optimiser"] --> B["SGD with Momentum"]

    B --> B1["Adds velocity term"]
    B --> B2["Accumulates past gradients"]
    B --> B3["Reduces zig-zag movement"]
    B --> B4["Speeds up movement in useful direction"]
    B --> B5["Helps through shallow regions"]

    B1 --> C1["Current update depends on previous update"]
    B2 --> C2["Builds inertia"]
    B3 --> C3["Smoother path to minimum"]

    style A fill:#C8E6C9,stroke:#43A047,stroke-width:2px
    style B fill:#E1F5FE,stroke:#4A90E2
    style B1 fill:#EDE7F6,stroke:#7E57C2
    style B2 fill:#FFF9C4,stroke:#FBC02D
    style B3 fill:#F8BBD0,stroke:#D81B60
    style B4 fill:#EDE7F6,stroke:#7E57C2
    style B5 fill:#FFF9C4,stroke:#FBC02D
{{< /mermaid >}}

---	
	
## Physical Intuition ☆

Momentum is often explained using the analogy of a ball rolling down a hill.

- The ball gains speed when it keeps moving in the same direction.
- Small bumps do not stop it immediately.
- Oscillations are dampened in directions where movement keeps changing.
- The optimiser can sometimes move through shallow local minima more effectively.

```mermaid
flowchart LR
    A[Current Gradient] --> B[Velocity Accumulation]
    C[Previous Velocity] --> B
    B --> D[Smoother Update]
    D --> E[Faster Progress Towards Minimum]

    style A fill:#E1F5FE
    style B fill:#C8E6C9
    style C fill:#FFF9C4
    style D fill:#EDE7F6
    style E fill:#C8E6C9
```

## Momentum Formula ☆

The velocity update is:

{{% colour "blue" %}}
{{< katex display=true >}}
v_t = \beta v_{t-1} + \nabla \mathcal{L}(\theta_t)
{{< /katex >}}
{{% /colour %}}

The parameter update is:

{{% colour "blue" %}}
{{< katex display=true >}}
\theta_{t+1} = \theta_t - \eta v_t
{{< /katex >}}
{{% /colour %}}

Where:

| Symbol | Meaning |
|---|---|
| {{< katex >}} v_t {{< /katex >}} | velocity at iteration {{< katex >}} t {{< /katex >}} |
| {{< katex >}} \beta {{< /katex >}} | momentum coefficient |
| {{< katex >}} \eta {{< /katex >}} | learning rate |
| {{< katex >}} \nabla \mathcal{L}(\theta_t) {{< /katex >}} | current gradient |

A common value is:

{{% colour "blue" %}}
{{< katex display=true >}}
\beta = 0.9
{{< /katex >}}
{{% /colour %}}

This means that around {{< katex >}} 90\% {{< /katex >}} of the previous velocity is retained.

## Why Momentum Helps ☆

Vanilla SGD can zigzag in narrow valleys because the gradient direction changes sharply from one step to the next.
Momentum smooths these updates.

| Problem in Vanilla SGD | How Momentum Helps |
|---|---|
| Zigzag movement | smooths oscillations |
| Slow progress in consistent direction | accumulates speed |
| Shallow local minima | may roll through small bumps |
| No memory of previous updates | stores velocity vector |

{{% hint success %}}
Momentum is especially useful when gradients point consistently in one useful direction but oscillate in another direction.
It accelerates the useful direction and dampens the noisy direction.
{{% /hint %}}

## SGD with Momentum Algorithm ☆

```mermaid
flowchart TD
    A[Initialise velocity v = 0] --> B[Choose mini-batch]
    B --> C[Compute gradient g]
    C --> D[Update velocity: v = beta v + g]
    D --> E[Update parameters: theta = theta - eta v]
    E --> F{Stopping criterion met?}
    F -- No --> B
    F -- Yes --> G[Return parameters]

    style A fill:#E1F5FE
    style B fill:#C8E6C9
    style C fill:#FFF9C4
    style D fill:#EDE7F6
    style E fill:#E1F5FE
    style F fill:#FFF9C4
    style G fill:#C8E6C9
```

For each mini-batch {{< katex >}} \mathcal{B} {{< /katex >}}:

{{% colour "blue" %}}
{{< katex display=true >}}
g = \frac{1}{B}\sum_{i \in \mathcal{B}} \nabla \mathcal{L}_i(\theta)
{{< /katex >}}
{{% /colour %}}

Then:

{{% colour "blue" %}}
{{< katex display=true >}}
v \leftarrow \beta v + g
{{< /katex >}}
{{% /colour %}}

{{% colour "blue" %}}
{{< katex display=true >}}
\theta \leftarrow \theta - \eta v
{{< /katex >}}
{{% /colour %}}

## Momentum Numerical Example ☆

Use the same toy problem:

{{% colour "blue" %}}
{{< katex display=true >}}
\mathcal{L}(w_1,w_2) = w_1^2 + 4w_2^2
{{< /katex >}}
{{% /colour %}}

Start at:

{{% colour "blue" %}}
{{< katex display=true >}}
(w_1,w_2) = (4,2), \qquad \eta = 0.05, \qquad \beta = 0.9
{{< /katex >}}
{{% /colour %}}

The learning rate is reduced compared with vanilla gradient descent because momentum amplifies the step direction.

| Iteration | Loss | Gradient {{< katex >}} [\nabla_{w_1}, \nabla_{w_2}] {{< /katex >}} | Velocity {{< katex >}} [v_1,v_2] {{< /katex >}} | {{< katex >}} w_1 {{< /katex >}} | {{< katex >}} w_2 {{< /katex >}} |
|---:|---:|---:|---:|---:|---:|
| 0 | 32.00 | {{< katex >}} [8,16] {{< /katex >}} | {{< katex >}} [8,16] {{< /katex >}} | {{< katex >}} 3.60 {{< /katex >}} | {{< katex >}} 1.20 {{< /katex >}} |
| 1 | 18.72 | {{< katex >}} [7.2,9.6] {{< /katex >}} | {{< katex >}} [14.4,24] {{< /katex >}} | {{< katex >}} 2.88 {{< /katex >}} | {{< katex >}} 0.00 {{< /katex >}} |
| 2 | 8.29 | {{< katex >}} [5.76,0] {{< /katex >}} | {{< katex >}} [18.7,21.6] {{< /katex >}} | {{< katex >}} 1.94 {{< /katex >}} | {{< katex >}} -1.08 {{< /katex >}} |
| 3 | 8.43 | {{< katex >}} [3.88,-8.64] {{< /katex >}} | {{< katex >}} [20.7,10.8] {{< /katex >}} | {{< katex >}} 0.91 {{< /katex >}} | {{< katex >}} -1.62 {{< /katex >}} |

The values show that the method can still oscillate, especially in the steep {{< katex >}} w_2 {{< /katex >}} direction.
However, momentum changes the behaviour of training by accumulating movement across steps.

{{% hint warning %}}
Momentum often needs a smaller learning rate than vanilla gradient descent because it amplifies steps using accumulated velocity.
{{% /hint %}}

## Momentum vs Vanilla SGD ☆

| Feature | Vanilla SGD | SGD with Momentum |
|---|---|---|
| Uses current gradient | yes | yes |
| Uses previous update direction | no | yes |
| Extra memory required | no | velocity vector |
| Handles zigzag movement | weaker | better |
| Typical use | simple baseline | general deep learning and production systems |

## Practical Guidelines ☆

Use momentum when:

- training with SGD is too noisy;
- the loss decreases slowly;
- updates zigzag in narrow valleys;
- the model is a deep neural network;
- you want a stronger baseline than vanilla SGD.

A typical setting is:

{{% colour "blue" %}}
{{< katex display=true >}}
\beta = 0.9
{{< /katex >}}
{{% /colour %}}

The learning rate should still be tuned carefully.
Momentum does not remove the need for learning rate selection.

## Exam Notes ☆

Remember these points:

- Momentum adds memory to SGD.
- The velocity stores accumulated gradients.
- The gradient is added to velocity, not directly to parameters.
- A common value is {{< katex >}} \beta = 0.9 {{< /katex >}}.
- Momentum can speed up convergence.
- Momentum can smooth oscillations in ravines.
- Momentum may require a smaller learning rate.
- Additional memory is needed to store a velocity vector of the same size as the parameters.

---
{{< home-link "Home" >}} | {{< section-index >}}
