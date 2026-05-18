---
title: 'Optimisation of Deep models'
draft: false
tags: ["AI", "Deep Learning", "DNN", "Optimisation", "Optimizers"]
categories: ["AI", "Deep Learning"]
weight: 1000
menu: main
---

# Optimisation of Deep models

Optimizers are algorithms that update neural network parameters to reduce the loss function.

Deep networks usually have millions or billions of parameters, so there is usually no closed-form solution.

Instead, training uses iterative optimisation.

{{% hint info %}}
**Key takeaway:**  
An optimiser decides how the model moves through the loss landscape towards lower loss.
{{% /hint %}}

---

- Goal of Optimization 
- Optimization Challenges in Deep Learning 
- Gradient Descent 
- Stochastic Gradient Descent 
- Minibatch Stochastic Gradient Descent 
- Momentum 
- Adagrad and Algorithm 
- RMSProp and  Algorithm 
- Adadelta and  Algorithm 
- Adam and Algorithm 
- Code Implementation and comparison of algorithms  (webinar) 

---

{{< mermaid >}}
flowchart TD
    A["Optimisers in DNN"] --> B["Gradient Descent Variants"]
    A --> C["Momentum-based Optimiser"]
    A --> D["Adaptive Methods"]
    A --> E["Learning Rate Schedules"]

    D --> D1["Parameter-specific learning rates"]

    E --> E1["Learning rate changes during training"]

    style A fill:#E1F5FE,stroke:#4A90E2,stroke-width:2px
    style B fill:#EDE7F6,stroke:#7E57C2
    style C fill:#C8E6C9,stroke:#43A047
    style D fill:#FFF9C4,stroke:#FBC02D
    style E fill:#F8BBD0,stroke:#D81B60
{{< /mermaid >}}	

---	

## Goal of Optimisation ☆

The goal is to find parameters {{< katex >}} \theta {{< /katex >}} that minimise the loss.

{{% colour "blue" %}}
{{< katex display=true >}}
\theta^* = \arg\min_{\theta} \mathcal{L}(\theta)
{{< /katex >}}
{{% /colour %}}

Here:

| Symbol | Meaning |
|---|---|
| {{< katex >}} \theta {{< /katex >}} | all model parameters |
| {{< katex >}} \mathcal{L}(\theta) {{< /katex >}} | loss function for the current parameters |
| {{< katex >}} \theta^* {{< /katex >}} | best parameters found by optimisation |

Deep neural networks may contain millions or billions of parameters.
Because of this, it is usually not possible to compute the best parameters directly.
Instead, we use iterative algorithms such as gradient descent, mini-batch gradient descent, momentum, RMSProp, Adam, and learning rate schedules.

Without optimisation, the network keeps random weights, produces random predictions, and does not learn useful patterns.

Without optimisation:

- weights remain random
- predictions remain random
- no learning happens

---

## Gradient Descent Core Idea

Gradient descent moves parameters in the opposite direction of the gradient.

{{% colour "blue" %}}
{{< katex display=true >}}
\theta_{t+1}=\theta_t-\eta \nabla_{\theta}\mathcal{L}(\theta_t)
{{< /katex >}}
{{% /colour %}}

Where:

- {{< katex >}} \eta {{< /katex >}} is the learning rate
- {{< katex >}} \nabla_{\theta}\mathcal{L} {{< /katex >}} is the gradient of loss

---

## Gradient Descent Variants ☆

| Method | Data used per update | Updates per epoch | Memory | Behaviour |
|---|---|---:|---|---|
| Batch GD | full dataset | 1 | high | smooth but slow |
| SGD | one example | many | low | noisy but fast |
| Mini-batch GD | batch of examples | medium | medium | balanced and practical |

---

## Mini-Batch Gradient Descent ☆

Mini-batch gradient descent is the practical default for deep learning.

For mini-batch {{< katex >}} B {{< /katex >}}:

{{% colour "blue" %}}
{{< katex display=true >}}
\mathcal{L}_B(\theta)=\frac{1}{B}\sum_{i \in B}\ell_i(\theta)
{{< /katex >}}
{{% /colour %}}

{{% colour "blue" %}}
{{< katex display=true >}}
g = \nabla_{\theta}\mathcal{L}_B(\theta)=\frac{1}{B}\sum_{i \in B}\nabla_{\theta}\ell_i(\theta)
{{< /katex >}}
{{% /colour %}}

{{% colour "blue" %}}
{{< katex display=true >}}
\theta \leftarrow \theta - \eta g
{{< /katex >}}
{{% /colour %}}

Common batch sizes:

- 32
- 64
- 128
- 256

---

## Learning Rate Effects ☆

| Learning rate | Behaviour | Result |
|---|---|---|
| Too large | jumps around, oscillates | may diverge |
| Too small | tiny updates | very slow learning |
| Just right | stable descent | smooth convergence |

```mermaid
flowchart LR
    A["Learning Rate"] --> B["Too Large"]
    A --> C["Too Small"]
    A --> D["Good Value"]

    B --> B1["Oscillation or divergence"]
    C --> C1["Slow convergence"]
    D --> D1["Stable loss reduction"]

    style A fill:#E1F5FE,stroke:#4A90E2
    style B fill:#FFF9C4,stroke:#FBC02D
    style C fill:#EDE7F6,stroke:#7E57C2
    style D fill:#C8E6C9,stroke:#43A047
```

---

## Loss Functions Review ☆

The loss function measures how wrong the model prediction is.
Different tasks use different loss functions.

| Loss Function | Formula | Gradient Form | Common Use |
|---|---|---|---|
| Mean Squared Error | {{< katex >}} \frac{1}{2}(\hat{y} - y)^2 {{< /katex >}} | {{< katex >}} \hat{y} - y {{< /katex >}} | Regression |
| Mean Absolute Error | {{< katex >}} \lvert \hat{y} - y \rvert {{< /katex >}} | {{< katex >}} \operatorname{sign}(\hat{y} - y) {{< /katex >}} | Regression with outliers |
| Binary Cross-Entropy | {{< katex >}} -[y\log(\hat{y}) + (1-y)\log(1-\hat{y})] {{< /katex >}} | similar to prediction minus target | Binary classification |
| Categorical Cross-Entropy | {{< katex >}} -\sum_k y_k \log(\hat{y}_k) {{< /katex >}} | similar to prediction minus target | Multi-class classification |

{{% hint success %}}
Many useful gradients have the intuitive form:

**prediction minus target**

This is one reason why backpropagation can efficiently compute updates across many layers.
{{% /hint %}}

---

## Optimisation Challenges

Deep learning loss surfaces are difficult.

Important challenges:

| Challenge | Meaning |
|---|---|
| Local minima | point lower than nearby points but not globally best |
| Saddle point | gradient is zero, but not a minimum |
| Plateau | flat region with very small gradient |
| Non-convex landscape | many valleys, ridges, and irregular paths |
| Exploding gradient | gradient becomes extremely large |
| Vanishing gradient | gradient becomes extremely small |


Training deep networks is difficult because the loss surface can be complicated.
The optimiser may face several problems.

### Saddle Points

A saddle point is a point where the gradient is close to zero, but the point is not a true minimum.
In some directions, the loss may increase.
In other directions, the loss may decrease.

{{% colour "blue" %}}
{{< katex display=true >}}
\nabla \mathcal{L}(\theta) \approx 0
{{< /katex >}}
{{% /colour %}}

This can make the optimiser slow down even though a better solution exists nearby.

### Plateaus

A plateau is a flat region of the loss surface.
Gradients become very small, so parameter updates become tiny.
Learning then becomes very slow.

### Non-Convex Landscapes

Deep learning loss surfaces are usually non-convex.
This means there can be many local minima and saddle points.
There is usually no guarantee that training will find the global minimum.

```mermaid
flowchart LR
    A[High Loss] --> B[Local Minimum]
    B --> C[Saddle Point]
    C --> D[Plateau]
    D --> E[Lower Loss Region]
    E --> F[Global Minimum]

    style A fill:#E1F5FE
    style B fill:#FFF9C4
    style C fill:#EDE7F6
    style D fill:#C8E6C9
    style E fill:#E1F5FE
    style F fill:#C8E6C9
```

## How to Identify Optimisation Problems ☆

During training, optimisation issues can be detected by looking at the loss curve, validation error, and gradient norms.

Common signs include:

- The loss curve flattens but validation error is still high.
- Gradient norms approach zero.
- Training progress stagnates for many epochs.
- Loss jumps around instead of decreasing smoothly.
- Training loss decreases, but validation loss becomes worse.

{{% hint warning %}}
A flat loss curve does not always mean the model has learned well.
It may also mean that the optimiser is stuck on a plateau, near a saddle point, or using an unsuitable learning rate.
{{% /hint %}}

---

## Momentum ☆

Momentum adds velocity to gradient descent.

It remembers previous update directions and smooths zig-zag movement.

{{% colour "blue" %}}
{{< katex display=true >}}
v_t = \beta v_{t-1} + g_t
{{< /katex >}}
{{% /colour %}}

{{% colour "blue" %}}
{{< katex display=true >}}
\theta_{t+1}=\theta_t-\eta v_t
{{< /katex >}}
{{% /colour %}}

Where:

- {{< katex >}} v_t {{< /katex >}} is velocity
- {{< katex >}} \beta {{< /katex >}} is momentum coefficient, often around 0.9
- {{< katex >}} g_t {{< /katex >}} is current gradient

---

## Momentum Intuition

```mermaid
flowchart TD
    A["SGD Path"] --> B["Noisy zig-zag"]
    C["Momentum Path"] --> D["Smoother movement"]
    D --> E["Faster progress in useful direction"]
    D --> F["Less oscillation across ravines"]

    style A fill:#FFF9C4,stroke:#FBC02D
    style B fill:#EDE7F6,stroke:#7E57C2
    style C fill:#E1F5FE,stroke:#4A90E2
    style D fill:#C8E6C9,stroke:#43A047
    style E fill:#C8E6C9,stroke:#43A047
    style F fill:#C8E6C9,stroke:#43A047
```

---

## Adaptive Optimisers ☆

Adaptive methods change the effective learning rate for each parameter.

They are useful when:

- gradients vary strongly across dimensions
- some parameters are sparse
- some directions are steep
- fixed learning rate causes zig-zag movement

---

## Adagrad

Adagrad accumulates squared gradients and gives smaller learning rates to frequently updated parameters.

{{% colour "blue" %}}
{{< katex display=true >}}
r_t = r_{t-1} + g_t \odot g_t
{{< /katex >}}
{{% /colour %}}

{{% colour "blue" %}}
{{< katex display=true >}}
\theta_{t+1}=\theta_t-\frac{\eta}{\sqrt{r_t}+\epsilon}\odot g_t
{{< /katex >}}
{{% /colour %}}

Good for sparse features, but the learning rate may shrink too much over time.

---

## RMSProp

RMSProp uses an exponentially weighted average of squared gradients.

{{% colour "blue" %}}
{{< katex display=true >}}
r_t = \rho r_{t-1} + (1-\rho)g_t \odot g_t
{{< /katex >}}
{{% /colour %}}

{{% colour "blue" %}}
{{< katex display=true >}}
\theta_{t+1}=\theta_t-\frac{\eta}{\sqrt{r_t}+\epsilon}\odot g_t
{{< /katex >}}
{{% /colour %}}

It avoids Adagrad's continuously shrinking learning rate problem.

---

## Adam ☆

Adam combines momentum and RMSProp-style adaptive scaling.

It tracks:

- first moment: average gradient
- second moment: average squared gradient

{{% colour "blue" %}}
{{< katex display=true >}}
m_t = \beta_1 m_{t-1} + (1-\beta_1)g_t
{{< /katex >}}
{{% /colour %}}

{{% colour "blue" %}}
{{< katex display=true >}}
v_t = \beta_2 v_{t-1} + (1-\beta_2)g_t^2
{{< /katex >}}
{{% /colour %}}

Bias correction:

{{% colour "blue" %}}
{{< katex display=true >}}
\hat{m}_t = \frac{m_t}{1-\beta_1^t}, \quad \hat{v}_t = \frac{v_t}{1-\beta_2^t}
{{< /katex >}}
{{% /colour %}}

Update:

{{% colour "blue" %}}
{{< katex display=true >}}
\theta_{t+1}=\theta_t-\eta\frac{\hat{m}_t}{\sqrt{\hat{v}_t}+\epsilon}
{{< /katex >}}
{{% /colour %}}

Common defaults:

- {{< katex >}} \beta_1 = 0.9 {{< /katex >}}
- {{< katex >}} \beta_2 = 0.999 {{< /katex >}}
- {{< katex >}} \epsilon = 10^{-8} {{< /katex >}}

---

## Adadelta

Adadelta is an adaptive method designed to reduce sensitivity to the initial learning rate.

It uses running averages of squared gradients and squared parameter updates.

For exams, remember it as an extension of adaptive learning-rate methods.

---

## Learning Rate Schedules ☆

Learning rate schedules change the learning rate over training.

Common strategies:

| Schedule | Idea |
|---|---|
| Step decay | reduce learning rate at fixed epochs |
| Exponential decay | gradually reduce by a constant factor |
| Cosine decay | smooth reduction using cosine curve |
| Warm-up | start small, then increase early |
| Reduce on plateau | reduce when validation loss stops improving |

---

## Gradient Clipping

Gradient clipping limits very large gradients.

It is especially useful for RNNs and unstable training.

{{% colour "blue" %}}
{{< katex display=true >}}
g \leftarrow g \cdot \frac{c}{\|g\|} \quad \text{if } \|g\| > c
{{< /katex >}}
{{% /colour %}}

---

## Choosing an Optimiser

| Situation | Good choice |
|---|---|
| Basic baseline | Mini-batch SGD |
| Noisy gradients | Momentum |
| Sparse features | Adagrad or Adam |
| General deep learning default | Adam |
| Need strong final generalisation | SGD with momentum after tuning |
| Exploding gradients | optimiser plus gradient clipping |

---

## Practical Interpretation

Optimisers decide how the model moves through the loss landscape.
A poor optimiser or a poor learning rate can make training slow, unstable, or completely unsuccessful.
A good optimiser can speed up training and make deep networks easier to train.

---

## Common Exam Mistakes ☆

- saying optimiser changes the dataset
- confusing learning rate with momentum
- forgetting mini-batch is the practical default
- assuming Adam always gives best generalisation
- not explaining why too high learning rate diverges
- forgetting that adaptive methods use parameter-wise scaling

---

## Revision / Summary

Remember these points:

- The aim of optimisation is to minimise the loss function.
- Deep networks usually need iterative optimisation because closed-form solutions are not available.
- Gradients tell the optimiser which direction increases the loss most quickly.
- Gradient descent moves in the opposite direction of the gradient.
- Saddle points, plateaus, and non-convexity make optimisation difficult.
- The choice of loss function depends on the task type.
- The choice of optimiser affects convergence speed and stability.

{{% hint success %}}
Remember the optimiser progression:

**Gradient Descent → Mini-batch GD → Momentum → Adaptive Methods → Adam → Learning Rate Scheduling**
{{% /hint %}}

| Optimiser | Main idea |
|---|---|
| Batch GD | full dataset update |
| SGD | one-example update |
| Mini-batch GD | balanced practical update |
| Momentum | accumulate velocity |
| Adagrad | scale by accumulated squared gradients |
| RMSProp | moving average of squared gradients |
| Adam | momentum plus adaptive scaling |
| Adadelta | adaptive method with reduced LR sensitivity |

---
  
## Reference
- **Dive into deep learning. Cambridge University Press.**. (Ch12)

---
{{< home-link "Home" >}} | {{< section-index >}}
