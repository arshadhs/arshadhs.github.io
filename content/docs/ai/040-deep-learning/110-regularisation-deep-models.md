---
title: 'Regularisation for Deep models'
draft: false
tags: ["AI", "Deep Learning", "DNN", "Regularisation", "Dropout", "Batch Normalisation"]
categories: ["AI", "Deep Learning"]
weight: 1100
menu: main
---

# Regularisation for Deep models

Regularisation means adding constraints or techniques that prevent a model from becoming too complex and memorising the training data.

The goal is not only low training error.

The goal is good performance on unseen data.

{{% hint info %}}
**Key takeaway:**  
Regularisation helps the model generalise by controlling complexity, stabilising training, and reducing overfitting.
{{% /hint %}}

- Generalization for regression  
- Training Error and Generalization Error 
- Underfitting or Overfitting 
- Model Selection 
- Weight Decay and Norms 
- Generalization in Classification 
- Environment and Distribution Shift 
- Generalization in Deep Learning 
- Dropout 
- Batch Normalization  
- Layer Normalization  

---

## Underfitting, Good Fit, and Overfitting ☆

| Case | Model behaviour | Training error | Test error |
|---|---|---|---|
| Underfitting | too simple | high | high |
| Good fit | captures useful pattern | low | low |
| Overfitting | memorises training noise | very low | high |

```mermaid
flowchart LR
    A["Model Complexity"] --> B["Too Simple: Underfitting"]
    A --> C["Just Right: Good Fit"]
    A --> D["Too Complex: Overfitting"]

    style A fill:#E1F5FE,stroke:#4A90E2
    style B fill:#FFF9C4,stroke:#FBC02D
    style C fill:#C8E6C9,stroke:#43A047
    style D fill:#EDE7F6,stroke:#7E57C2
```

---

## Training Error and Generalisation Error ☆

Training error measures performance on data used for learning.

Generalisation error measures expected performance on unseen data.

A model can have excellent training performance and poor test performance.

That is overfitting.

---

## Regularisation Taxonomy

```mermaid
flowchart TD
    A["Regularisation"] --> B["Explicit Regularisation"]
    A --> C["Implicit Regularisation"]

    B --> B1["Weight penalties: L1 and L2"]
    B --> B2["Structural methods: Dropout"]

    C --> C1["Training procedures: Early stopping"]
    C --> C2["Normalisation: Batch Norm and Layer Norm"]
    C --> C3["Data augmentation"]

    style A fill:#E1F5FE,stroke:#4A90E2
    style B fill:#C8E6C9,stroke:#43A047
    style C fill:#FFF9C4,stroke:#FBC02D
    style B1 fill:#EDE7F6,stroke:#7E57C2
    style B2 fill:#EDE7F6,stroke:#7E57C2
    style C1 fill:#C8E6C9,stroke:#43A047
    style C2 fill:#C8E6C9,stroke:#43A047
    style C3 fill:#C8E6C9,stroke:#43A047
```

---

## L2 Regularisation / Weight Decay ☆

L2 regularisation penalises large weights.

{{% colour "blue" %}}
{{< katex display=true >}}
J_{regularised}(\theta)=J(\theta)+\lambda \|\theta\|_2^2
{{< /katex >}}
{{% /colour %}}

This encourages smoother models with smaller weights.

Weight decay is closely related to L2 regularisation in gradient-based optimisation.

---

## L1 Regularisation

L1 regularisation encourages sparse weights.

{{% colour "blue" %}}
{{< katex display=true >}}
J_{regularised}(\theta)=J(\theta)+\lambda \|\theta\|_1
{{< /katex >}}
{{% /colour %}}

It can push some weights towards zero.

---

## Dropout ☆

Dropout randomly disables some neurons during training.

This prevents the model from relying too heavily on any single neuron or feature.

{{% hint warning %}}
Dropout does not usually remove neurons because they are bad.

It randomly drops units during training to make the network more robust.
{{% /hint %}}

---

## Dropout Intuition

```mermaid
flowchart LR
    A["Full Network"] --> B["Randomly Drop Some Units During Training"]
    B --> C["Different Sub-Network Each Batch"]
    C --> D["More Robust Features"]
    D --> E["Better Generalisation"]

    style A fill:#E1F5FE,stroke:#4A90E2
    style B fill:#FFF9C4,stroke:#FBC02D
    style C fill:#EDE7F6,stroke:#7E57C2
    style D fill:#C8E6C9,stroke:#43A047
    style E fill:#C8E6C9,stroke:#43A047
```

---

## Dropout Formula

During training, a binary mask is sampled.

{{% colour "blue" %}}
{{< katex display=true >}}
\tilde{h}=m \odot h
{{< /katex >}}
{{% /colour %}}

Where:

- {{< katex >}} h {{< /katex >}} is the activation
- {{< katex >}} m {{< /katex >}} is a random mask
- {{< katex >}} \odot {{< /katex >}} means element-wise multiplication

---

## Batch Normalisation ☆

Batch normalisation normalises activations using mini-batch statistics.

It helps stabilise training and often allows faster convergence.

{{% colour "blue" %}}
{{< katex display=true >}}
\mu_B = \frac{1}{m}\sum_{i=1}^{m}x_i
{{< /katex >}}
{{% /colour %}}

{{% colour "blue" %}}
{{< katex display=true >}}
\sigma_B^2 = \frac{1}{m}\sum_{i=1}^{m}(x_i-\mu_B)^2
{{< /katex >}}
{{% /colour %}}

{{% colour "blue" %}}
{{< katex display=true >}}
\hat{x}_i = \frac{x_i-\mu_B}{\sqrt{\sigma_B^2+\epsilon}}
{{< /katex >}}
{{% /colour %}}

{{% colour "blue" %}}
{{< katex display=true >}}
y_i=\gamma \hat{x}_i+\beta
{{< /katex >}}
{{% /colour %}}

---

## Layer Normalisation ☆

Layer normalisation normalises across features within each example.

It is commonly used in transformers.

Batch normalisation depends on batch statistics.

Layer normalisation is more suitable for sequence models where batch statistics may be less stable.

---

## Early Stopping

Early stopping monitors validation performance and stops training when validation error no longer improves.

It prevents the model from continuing to memorise the training set.

```mermaid
flowchart LR
    A["Start Training"] --> B["Training Loss Falls"]
    B --> C["Validation Loss Improves"]
    C --> D{"Validation Loss Stops Improving?"}
    D -- "No" --> B
    D -- "Yes" --> E["Stop Training"]

    style A fill:#E1F5FE,stroke:#4A90E2
    style B fill:#C8E6C9,stroke:#43A047
    style C fill:#C8E6C9,stroke:#43A047
    style D fill:#FFF9C4,stroke:#FBC02D
    style E fill:#EDE7F6,stroke:#7E57C2
```

---

## Data Augmentation

Data augmentation creates modified versions of training examples.

In computer vision, this may include:

- rotation
- cropping
- flipping
- brightness changes
- zooming

It helps the model learn robust patterns instead of memorising exact images.

---

## Vanishing Gradient Problem ☆

Vanishing gradient means gradients become extremely small in earlier layers.

This makes early layers learn very slowly.

{{% colour "blue" %}}
{{< katex display=true >}}
\frac{\partial L}{\partial W^{(1)}} = \frac{\partial L}{\partial h^{(L)}} \prod_{l=2}^{L} \frac{\partial h^{(l)}}{\partial h^{(l-1)}} \frac{\partial h^{(1)}}{\partial W^{(1)}}
{{< /katex >}}
{{% /colour %}}

If many factors in the product are less than one, the gradient becomes very small.

Common in:

- deep networks with sigmoid or tanh
- long RNN sequences
- poor weight initialisation

---

## Exploding Gradient Problem ☆

Exploding gradient means gradients become extremely large.

Symptoms:

- loss becomes NaN or infinity
- huge weight updates
- unstable training curves

Solutions:

- gradient clipping
- better initialisation
- normalisation
- residual connections

---

## Weight Initialisation ☆

Good weight initialisation helps avoid vanishing or exploding gradients.

Bad initialisation can:

- break learning
- make neurons compute identical functions
- slow convergence
- cause saturation

---

## Xavier / Glorot Initialisation

Xavier initialisation keeps activation variance and gradient variance more stable across layers.

It is commonly used for sigmoid or tanh-style activations.

{{% colour "blue" %}}
{{< katex display=true >}}
W \sim U\left(-\sqrt{\frac{6}{n_{in}+n_{out}}}, \sqrt{\frac{6}{n_{in}+n_{out}}}\right)
{{< /katex >}}
{{% /colour %}}

---

## He Initialisation

He initialisation is commonly used with ReLU and Leaky ReLU.

{{% colour "blue" %}}
{{< katex display=true >}}
W \sim N\left(0, \frac{2}{n_{in}}\right)
{{< /katex >}}
{{% /colour %}}

---

## When to Apply Regularisation ☆

| Situation | Useful technique |
|---|---|
| Small dataset | data augmentation, L2, dropout |
| Deep network | batch norm, residual connections, dropout |
| Transformer | layer norm, dropout, weight decay |
| RNN / sequence model | recurrent dropout, gradient clipping |
| Overfitting | dropout, early stopping, L2 |
| Training instability | normalisation, better initialisation, gradient clipping |

---

## Common Mistakes ☆

- saying regularisation always improves training accuracy
- confusing training error and test error
- saying dropout permanently deletes neurons
- forgetting dropout behaves differently during training and inference
- using batch norm and layer norm interchangeably without context
- ignoring weight initialisation in deep networks

---

## Revision / Summary

{{% hint success %}}
Regularisation is about finding the right balance:

**enough model capacity to learn patterns, but enough constraint to avoid memorisation.**
{{% /hint %}}

| Technique | Main purpose |
|---|---|
| L1 | sparsity |
| L2 / weight decay | smaller weights |
| Dropout | reduce co-adaptation |
| Batch norm | stabilise mini-batch activations |
| Layer norm | stabilise per-example feature activations |
| Early stopping | stop before overfitting worsens |
| Data augmentation | increase effective data variety |
| Gradient clipping | prevent exploding gradients |
| Xavier / He initialisation | support stable gradient flow |

---
  
## Reference
- **Dive into deep learning. Cambridge University Press.**. ([T1 – Ch 3.6, 3.7, T1 - Ch 4.6, 4.7, T1 - Ch 5.5, 5.6, T1 - Ch 8.5, T1 - Ch 11.7](https://d2l.ai/chapter_introduction/index.html)

---
{{< home-link "Home" >}} | {{< section-index >}}
