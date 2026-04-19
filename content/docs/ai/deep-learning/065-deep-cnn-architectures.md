---
title: "Deep CNN Architectures"
date: 2026-04-19
draft: false
weight: 650
tags:
  - Deep Learning
  - CNN
  - Deep CNN Architectures
  - LeNet
  - AlexNet
  - VGG
  - NiN
  - GoogLeNet
  - ResNet
  - Transfer Learning
categories:
  - ai
  - deep-learning
---

# Deep CNN Architectures

Once the basic ideas of convolution, pooling, channels, and classifier heads are understood, the next step is to study how successful CNN architectures are designed in practice. The history of deep CNNs is not just a list of famous models. It is a progression of design ideas: smaller filters, more depth, better optimisation, bottlenecks, multi-scale processing, residual connections, and transfer learning.

{{% hint info %}}
**Key takeaway:**  
Deep CNN architectures evolved by solving specific problems one by one: **LeNet** established the template, **AlexNet** proved deep learning could dominate large-scale vision, **VGG** simplified the design, **NiN** introduced powerful `1 × 1` ideas, **GoogLeNet** made multi-scale processing efficient, and **ResNet** solved the optimisation problem of very deep networks.
{{% /hint %}}

{{% hint warning %}}
**Exam focus:**  
For this topic, you should be able to explain:
- the evolution from LeNet to ResNet,
- the main innovation of each architecture,
- why VGG used stacked `3 × 3` convolutions,
- why Inception used parallel multi-scale branches,
- why ResNet used skip connections,
- and how transfer learning uses these pre-trained architectures in practical tasks.
{{% /hint %}}

## Where this fits in the course

In the course handout, the **Deep Convolutional Neural Networks** part includes:

- AlexNet,
- VGGNet,
- Network in Network,
- GoogLeNet / Inception,
- Residual Networks (ResNet).

The following session then continues into **transfer learning** and fine-tuning. The lecture transcripts also reinforce this progression by revisiting VGG, ResNet, Inception-style models, and the practical idea of taking a pre-trained model and adapting it to a new task.

## Why deeper CNN architectures were needed

A simple CNN such as `Conv → Pool → Conv → Pool → Dense` works well on small tasks, but larger real-world vision problems need more expressive models.

As researchers scaled CNNs, they encountered several challenges:

- need for better feature hierarchies,
- increased computational cost,
- risk of overfitting,
- optimisation difficulty in very deep networks,
- need to capture patterns at multiple scales,
- need to reuse strong learned features on smaller datasets.

This is why deep CNN architecture design became such an important field.

## The evolution of CNN architectures

{{< mermaid >}}
flowchart LR
    A[LeNet] --> B[AlexNet]
    B --> C[VGG]
    C --> D[NiN]
    D --> E[GoogLeNet or Inception]
    E --> F[ResNet]
    F --> G[Transfer learning on new tasks]
{{< /mermaid >}}

## Design patterns in successful CNNs

Before looking at each model individually, it is useful to understand the repeated design ideas that appear across strong architectures.

### 1. Spatial reduction with channel expansion

As the network goes deeper:

- height and width usually decrease,
- the number of channels usually increases.

This keeps computation manageable while allowing richer semantic features.

### 2. Repetitive building blocks

Strong architectures are usually built from repeated modules rather than arbitrary layer-by-layer changes.

Examples:

- VGG block,
- Inception block,
- residual block.

This improves modularity, scalability, and design clarity.

### 3. Skip or residual connections

Residual connections help gradient flow and make very deep networks trainable.

{{% colour "green" %}}
$$
H(x)=F(x)+x
$$
{{% /colour %}}

If the identity mapping is already good, the block only needs to learn the residual correction \(F(x)\).

### 4. Bottleneck design

Bottlenecks reduce channels before expensive convolutions and expand them afterwards.

Typical pattern:

- `1 × 1` reduce,
- `3 × 3` process,
- `1 × 1` expand.

This saves large amounts of computation.

### 5. Multi-scale feature extraction

Objects can appear at different sizes, so architectures such as Inception process the same input through different branch sizes in parallel.

### 6. Replacing heavy dense layers

Architectures such as NiN and many later models reduce dependence on large fully connected layers by using:

- `1 × 1` convolution,
- global average pooling,
- more efficient classification heads.

## LeNet-5

LeNet-5 is one of the earliest successful CNNs and is historically important because it established the standard convolutional recipe.

### Why LeNet matters

- one of the first convincing CNN successes,
- designed for document and digit recognition,
- established the broad pattern of convolution, subsampling / pooling, and classification.

### Main lesson from LeNet

LeNet showed that local filtering followed by progressively more abstract feature extraction could outperform simpler hand-crafted approaches on image tasks.

### Conceptual structure

{{< mermaid >}}
flowchart LR
    A[Input digits] --> B[Conv]
    B --> C[Pooling]
    C --> D[Conv]
    D --> E[Pooling]
    E --> F[Dense classifier]
    F --> G[Digit class]
{{< /mermaid >}}

LeNet is small by modern standards, but it provided the template that later deep CNNs extended dramatically.

## AlexNet

AlexNet was the architecture that won ImageNet 2012 and triggered the modern deep learning revolution in computer vision.

### Why AlexNet was a breakthrough

It showed that a deep CNN trained on a large dataset with GPU computation could dramatically outperform older vision systems.

### Key innovations

- **ReLU activation** instead of sigmoid or tanh,
- **dropout** for regularisation,
- **data augmentation**,
- **GPU acceleration**,
- deeper and larger model capacity than earlier CNNs.

### Architectural summary

- 8 learned layers,
- 5 convolutional layers,
- 3 fully connected layers,
- roughly 60 million parameters,
- input size around `224 × 224 × 3`.

### Why AlexNet mattered academically and practically

AlexNet proved that scale matters:

- more data,
- more computation,
- deeper models,
- and better training choices.

It also made ReLU and dropout mainstream in CNN design.

## VGG

VGG made CNN design cleaner and more systematic.

### Core idea

Instead of using larger filters directly, VGG stacked many small `3 × 3` convolutions.

### Why stacked `3 × 3` convolutions are powerful

Two `3 × 3` convolutions approximate the receptive field of one `5 × 5` convolution, while offering:

- fewer parameters,
- more non-linearities,
- a simpler repeated design.

### VGG characteristics

- very uniform architecture,
- repeated blocks of `3 × 3` convolutions,
- `2 × 2` max-pooling for downsampling,
- commonly seen in VGG-16 and VGG-19.

### Strengths

- easy to understand,
- clean design,
- useful for learning architecture patterns.

### Limitations

- computationally heavy,
- very large parameter count,
- especially expensive because of large dense layers.

### Why VGG is still important

Even though later models became more efficient, VGG is excellent for understanding the logic of deep CNN block design.

## Network in Network (NiN)

NiN introduced the idea that local convolution should not be only a linear filter. Instead, a small micro-network can be used at each location.

### Main contributions

- **MLPconv** layers,
- heavy use of **`1 × 1` convolution**,
- **global average pooling** instead of large dense layers.

### Why NiN mattered

NiN helped popularise `1 × 1` convolution as a practical and powerful idea. This influenced many later models including Inception and ResNet bottlenecks.

### Conceptual significance

NiN showed that we can increase representational power without simply making every layer wider or every dense layer larger.

## GoogLeNet / Inception

GoogLeNet introduced the **Inception module**, which processes the same input at multiple scales in parallel and concatenates the outputs.

### The problem Inception tried to solve

Different objects and patterns can be meaningful at different spatial scales. A single fixed filter size may not be ideal everywhere.

### Inception idea

Use parallel branches such as:

- `1 × 1`,
- `3 × 3`,
- `5 × 5`,
- pooling branch,

then concatenate the outputs.

{{< mermaid >}}
flowchart TD
    A[Input] --> B1[1x1 conv]
    A --> B2[1x1 reduce then 3x3 conv]
    A --> B3[1x1 reduce then 5x5 conv]
    A --> B4[Pooling then 1x1 conv]
    B1 --> C[Concatenate]
    B2 --> C
    B3 --> C
    B4 --> C
{{< /mermaid >}}

### Why `1 × 1` reduction was crucial

A naive parallel architecture would be computationally expensive. Inception used `1 × 1` convolutions before the more expensive `3 × 3` and `5 × 5` branches to reduce channel count.

### Inception advantages

- captures multi-scale information,
- increases model capacity,
- improves efficiency using bottlenecks,
- allows depth and width without massive computational explosion.

### Historical importance

GoogLeNet won ImageNet 2014 and demonstrated that careful architecture design can outperform brute-force scaling.

## ResNet

As networks became deeper, optimisation became harder. Surprisingly, very deep plain networks sometimes had **higher training error** than shallower ones. This showed that the problem was not simply overfitting; it was optimisation difficulty.

ResNet solved this using **residual learning**.

### Residual idea

Instead of learning \(H(x)\) directly, the block learns a residual correction \(F(x)\), then adds the identity shortcut:

{{% colour "green" %}}
$$
H(x)=F(x)+x
$$
{{% /colour %}}

### Why this helps

- easier gradient flow during backpropagation,
- easier learning of identity mappings,
- enables very deep networks such as ResNet-50, ResNet-101, and ResNet-152.

### Basic residual block

A standard residual block often contains:

- convolution,
- batch normalisation,
- ReLU,
- convolution,
- batch normalisation,
- identity shortcut,
- final ReLU.

{{< mermaid >}}
flowchart LR
    A[x] --> B[Conv]
    B --> C[BN]
    C --> D[ReLU]
    D --> E[Conv]
    E --> F[BN]
    A --> G[Identity shortcut]
    F --> H[Add]
    G --> H
    H --> I[ReLU]
{{< /mermaid >}}

### Bottleneck residual block

Deeper ResNet variants usually use:

- `1 × 1` reduce,
- `3 × 3` process,
- `1 × 1` expand.

This saves substantial computation while preserving depth.

### Why ResNet changed the field

Residual connections became a standard deep learning idea far beyond CNNs. They now appear in many modern architectures including transformers.

## Comparing major deep CNN architectures

| Architecture | Main idea | Strength | Limitation |
|---|---|---|---|
| LeNet | Early conv-pool-classifier pattern | Historical foundation | Small and simple |
| AlexNet | ReLU, dropout, GPU training | Breakthrough on ImageNet | Large parameter count |
| VGG | Deep stacks of `3 × 3` convolutions | Clean and intuitive design | Computationally heavy |
| NiN | `1 × 1` convolution and GAP | More expressive and efficient | Less common as a production backbone now |
| GoogLeNet | Multi-scale Inception modules | Efficient and deep | More complex module design |
| ResNet | Residual learning with skip connections | Very deep networks train well | More architectural complexity |

## How to choose architecture depth and width

The slides and teaching flow strongly suggest that architecture choice depends on:

- **task complexity**,
- **dataset size**,
- **computational budget**,
- **risk of overfitting**,
- **need for efficient deployment**.

### Practical guidance

- **Small dataset**  
  prefer transfer learning or a moderate architecture.

- **Large dataset**  
  deeper networks become more useful.

- **Limited compute**  
  use efficient designs and avoid huge dense heads.

- **Very deep model**  
  residual connections become important.

### Depth vs width

- **Depth** means more layers and more hierarchical composition.
- **Width** means more channels per layer and more parallel feature capacity.

Strong modern design often balances both rather than maximising only one.

## Transfer learning

Transfer learning is one of the most practical outcomes of deep CNN architecture research.

### Definition

A model trained on a large source dataset is reused as the starting point for a new but related target task.

Typical source task:

- ImageNet classification.

Typical target task:

- your own classification problem with much less labelled data.

### Why transfer learning works

The early and middle layers of CNNs learn reusable visual features such as:

- edges,
- colour contrasts,
- textures,
- local motifs,
- object-part patterns.

These are useful across many related image domains.

### Main transfer learning strategies

#### 1. Feature extraction

- keep the pre-trained backbone frozen,
- train only the new classifier head.

Use this when:

- dataset is small,
- source and target domains are similar.

#### 2. Fine-tuning

- initialise from pre-trained weights,
- train the full model with a smaller learning rate.

Use this when:

- dataset is medium or large,
- the new task is related but needs adaptation.

#### 3. Selective fine-tuning

- freeze early layers,
- fine-tune later layers.

Use this when:

- you want a balance between stability and task adaptation.

{{< mermaid >}}
flowchart TD
    A[New task] --> B{Dataset size}
    B -->|Small| C{Similarity to ImageNet}
    C -->|High| D[Feature extraction]
    C -->|Lower| E[Freeze early layers and fine tune later layers]
    B -->|Medium or large| F{Similarity to ImageNet}
    F -->|High| G[Fine tune whole model with low learning rate]
    F -->|Lower| H[Selective or full fine tuning based on validation]
{{< /mermaid >}}

### Practical rule of thumb

- **Small and similar dataset**  
  freeze more layers.

- **Larger or more different dataset**  
  fine-tune more layers.

## Applications of deep CNN architectures

These models are used in many practical tasks:

### Image classification

Assign one label to the entire image.

### Object detection

Locate and classify multiple objects.

### Semantic segmentation

Predict a class label for every pixel.

### Face recognition

Identify or verify identities.

### Medical imaging

Classify scans, detect abnormalities, support diagnosis.

### Beyond images

CNN ideas are also used in:

- audio processing,
- text classification,
- time-series modelling with `1D` convolution,
- graph-inspired convolutional methods.

## Practical implementation guidance

### Preprocessing

- normalise pixel values,
- use proper training, validation, and test splits,
- resize images consistently.

### Data augmentation

For better generalisation, use:

- flipping,
- cropping,
- rotation,
- scaling,
- colour jitter.

### Training strategy

- start with a proven baseline architecture,
- monitor both training and validation metrics,
- use learning-rate scheduling,
- use early stopping where appropriate,
- prefer transfer learning when data is limited.

### Common pitfalls

#### Overfitting

Symptoms:

- very high training accuracy,
- weaker validation performance.

Typical fixes:

- more augmentation,
- dropout,
- weight decay,
- transfer learning,
- more data.

#### Underfitting

Symptoms:

- poor training and validation results.

Typical fixes:

- more depth or width,
- better optimisation,
- longer training,
- lower regularisation.

#### Vanishing or exploding gradients

Typical fixes:

- good initialisation,
- batch normalisation,
- residual connections.

#### Excessive computation

Typical fixes:

- reduce input size where acceptable,
- use bottlenecks,
- reduce channels carefully,
- choose more efficient architectures.

## Exam-ready summary

You should be able to explain:

1. Why deeper CNNs were needed beyond a simple LeNet-style model  
2. The main contribution of LeNet, AlexNet, VGG, NiN, GoogLeNet, and ResNet  
3. Why VGG preferred repeated `3 × 3` filters  
4. Why NiN made `1 × 1` convolution important  
5. Why Inception uses parallel branches  
6. Why bottlenecks reduce computation  
7. Why ResNet uses skip connections  
8. Why residual learning made very deep networks trainable  
9. How transfer learning uses pre-trained CNN backbones  
10. When feature extraction and fine-tuning should be used  

## Quick viva questions

- Why was AlexNet considered a turning point in deep learning?
- Why can two `3 × 3` convolutions be preferable to one `5 × 5` convolution?
- Why is `1 × 1` convolution central to NiN and Inception?
- How do Inception modules capture multi-scale features?
- Why do plain very deep networks become hard to optimise?
- Why does ResNet learn residuals instead of direct mappings?
- Why is transfer learning often better than training from scratch on a small dataset?

## Related pages

- [CNN Fundamentals]({{% relref "060-cnn-fundamentals.md" %}})

## References

- sections on architecture design, popular CNN architectures, and transfer learning
- Lecture transcripts covering VGG, Inception, ResNet, and transfer learning discussions
- Zhang, A., Lipton, Z. C., Li, M., & Smola, A. J. *Dive into Deep Learning*
- Goodfellow, Bengio, and Courville. *Deep Learning*

---

{{< home-link "Home" >}} | {{< section-index >}}

---
