---
title: 'Convolutional Neural Networks'
date: 2026-04-19
draft: false
weight: 600
tags:
  - Deep Learning
  - CNN
  - Convolutional Neural Networks
  - Computer Vision
  - Feature Maps
  - Receptive Field
  - Padding
  - Pooling
categories:
  - ai
  - deep-learning
---

# Convolutional Neural Networks (CNN)

Convolutional Neural Networks (CNNs) are specialised neural networks designed for data with spatial structure, especially images. They became the standard model for computer vision because they preserve spatial locality, reuse the same pattern detector across the image, and build representations hierarchically. In practical terms, a CNN starts by learning simple features such as edges and corners, then combines them into textures, shapes, object parts, and finally full semantic categories.

{{% hint info %}}
**Key takeaway:**  
A CNN is powerful because it does **not** treat an image as just a long vector. Instead, it uses **local connectivity**, **parameter sharing**, and **hierarchical feature learning** through the pattern:

convolution → activation → pooling / downsampling → deeper feature extraction → classifier
{{% /hint %}}

{{% hint warning %}}
**Exam focus:**  
For this topic, make sure you can explain clearly:
- why CNNs are better than fully connected networks for images,
- what filters and feature maps are,
- how padding, stride, and receptive field work,
- how multi-channel convolution works,
- why pooling and `1 × 1` convolution matter,
- and how a standard CNN performs classification from input image to final softmax output.
{{% /hint %}}

## Convolutional Neural Networks

A CNN is not just a “neural network for images”. Its real strength is that it reduces parameters, preserves spatial structure, and learns features hierarchically through **convolution → activation → pooling → deeper feature extraction → classification**. For exams, make sure you can explain **why CNNs are better than fully connected networks for images**, how **padding/stride/receptive field** work, and how architectures evolved from **LeNet to ResNet**.

- image data,
- invariance, locality, and channels,
- convolution operation,
- learning a kernel,
- feature map and receptive field,
- padding and stride,
- `1 × 1` convolution,
- pooling,
- multiple channels,
- and putting the ideas together in a LeNet-style CNN.

Emphasising the ideas, especially the intuition of sliding a filter, producing a feature map, handling borders with padding, controlling movement with stride, and understanding why receptive field grows with depth.

## Why CNNs are needed

A plain multilayer perceptron can process image pixels, but it ignores the fact that nearby pixels are related and that the same pattern can appear in different parts of the image. This makes it a poor design choice for image understanding.

### Problems with image data

CNNs are motivated by several difficulties in image classification:

- **High dimensionality**  
  Even a modest image can contain thousands or millions of pixel values.

- **Local structure matters**  
  Edges, textures, and corners are small local patterns, not global full-image patterns.

- **Position should not matter too much**  
  A dog is still a dog whether it appears at the left, centre, or right of the image.

- **Fully connected networks are parameter-heavy**  
  Connecting every pixel to every neuron leads to very large weight matrices.

### Core reasons CNNs work well

1. **Locality**  
   A neuron only needs to see a small neighbourhood to detect an edge or texture.

2. **Translation equivariance**  
   If the input shifts, the feature map shifts correspondingly.

3. **Approximate translation invariance**  
   Pooling and deeper feature aggregation make the model less sensitive to small shifts.

4. **Parameter sharing**  
   The same filter is reused across many image locations.

5. **Hierarchical feature learning**  
   Early layers learn simple patterns, middle layers learn combinations, deeper layers learn semantic structures.

{{< mermaid >}}
flowchart LR
    A[Input image] --> B[Local filters]
    B --> C[Edges and corners]
    C --> D[Textures and shapes]
    D --> E[Object parts]
    E --> F[Class decision]

    style A fill:#E1F5FE
    style B fill:#C8E6C9
    style C fill:#FFF9C4
    style D fill:#EDE7F6
    style E fill:#C8E6C9
    style F fill:#FFE0B2
{{< /mermaid >}}

## Images as tensors

A digital image is represented numerically.

### Grayscale image

A grayscale image is a 2D grid of pixel intensities:

- height × width

### Colour image

A colour image is a 3D tensor:

- height × width × channels

For RGB images, the channels are:

- Red
- Green
- Blue

So an image of size `32 × 32 × 3` has:

- 32 pixels in height,
- 32 pixels in width,
- 3 channels.

This channel view is important because convolutional layers create many new channels called **feature maps**.

## CNN pipeline

A classical CNN usually follows a pipeline like this:

Input → Convolution → Activation → Pooling or strided convolution → More convolution blocks → Flatten or global average pooling → Dense layer or classifier head → Softmax or sigmoid output

So a CNN does two broad jobs:

- **feature extraction** through convolutional layers,
- **decision making** through a classifier head.

## Convolution: the central operation

A convolutional layer applies a small learnable filter, also called a **kernel**, over the input.

At each spatial position, the layer:

- selects a local patch,
- multiplies it elementwise with the kernel,
- sums the products,
- and usually adds a bias.

That produces one value in the output feature map.

Intuition: A **kernel** is a small pattern detector. One kernel may respond strongly to vertical edges, another to horizontal edges, another to texture. During training, the network learns useful filters automatically by backpropagation and gradient descent.

### Single-channel convolution

For a grayscale image and a `K_h × K_w` kernel:

{{% colour "green" %}}
{{< katex display=true >}}
Y(i,j)=\sum_{m=0}^{K_h-1}\sum_{n=0}^{K_w-1} X(i+m,j+n)\,W(m,n)+b
{{< /katex >}}
{{% /colour %}}

Where:
- {{< katex >}}X{{< /katex >}} is the input,
- {{< katex >}}W{{< /katex >}} is the kernel,
- {{< katex >}}b{{< /katex >}} is the bias,
- {{< katex >}}Y{{< /katex >}} is the output feature map.

---

### Multi-channel convolution

If the input is:

{{< katex >}}H \times W \times C_{in}{{< /katex >}}

then each filter spans all input channels.

One filter size is:

{{< katex >}}K_h \times K_w \times C_{in}{{< /katex >}}

One filter produces one output channel.

If we use:

{{< katex >}}C_{out}{{< /katex >}}

filters, we obtain:

{{< katex >}}C_{out}{{< /katex >}}

output channels.

{{% colour "green" %}}
{{< katex display=true >}}
\text{Output channels} = \text{Number of filters}
{{< /katex >}}
{{% /colour %}}

{{% colour "green" %}}
{{< katex display=true >}}
\text{Parameters in a conv layer}
=
(K_h \times K_w \times C_{in} + 1)\times C_{out}
{{< /katex >}}
{{% /colour %}}

**Why this formula matters**

This is a key exam formula. Notice that the number of learnable parameters depends on:

- kernel height and width,
- number of input channels,
- number of output channels,
- and whether bias is included.

It does **not** depend on the input height or width. That is one major reason CNNs are far more parameter-efficient than fully connected networks.

## Feature maps

The output produced by a filter across the full image is called a **feature map**.

If a layer has 64 filters, it produces 64 feature maps. Together they form an output volume:

- height × width × channels

Each feature map can be interpreted as:

> “Where in the image does this learned pattern appear strongly?”

## Receptive field

The **receptive field** of a neuron is the region of the input that can affect that neuron’s activation.

This is a very important concept because it explains why deeper layers can capture more global information.

**Intuition of receptive field growth**

- early layers see only small local regions,
- deeper layers indirectly depend on larger parts of the image,
- so deeper layers capture more context and more meaningful structure.

**Why receptive field matters**

- **Small receptive field**  
  good for local detail, but poor for large objects or global context.

- **Large receptive field**  
  better for understanding object structure, relationships, and scene context.

**Ways to increase receptive field**

- stack more convolutional layers,
- use larger kernels,
- use pooling,
- use strided convolution,
- use dilated convolution.

{{< mermaid >}}
flowchart TD
    A[Small receptive field] --> B[Edges and tiny local patterns]
    B --> C[Stack more layers]
    C --> D[Larger effective receptive field]
    D --> E[Shapes object parts and context]

    style A fill:#E1F5FE
    style B fill:#C8E6C9
    style C fill:#FFF9C4
    style D fill:#EDE7F6
    style E fill:#FFE0B2
{{< /mermaid >}}

## Padding

Without padding, convolution shrinks the spatial size. In deep networks, repeated shrinking can make feature maps too small too quickly and can also reduce the importance of border pixels.

### Types of padding

- **Valid padding**  
  no padding; output becomes smaller.

- **Same padding**  
  padding is added so that the output size is preserved when stride is 1.

**Output size formula**

{{% colour "green" %}}
{{< katex display=true >}}
H_{out}
=
\left\lfloor
\frac{H+2P-K}{S}
\right\rfloor
+1
{{< /katex >}}
{{% /colour %}}

{{% colour "green" %}}
{{< katex display=true >}}
W_{out}
=
\left\lfloor
\frac{W+2P-K}{S}
\right\rfloor
+1
{{< /katex >}}
{{% /colour %}}

Where:
- {{< katex >}}H,W{{< /katex >}} are input dimensions,
- {{< katex >}}K{{< /katex >}} is kernel size,
- {{< katex >}}P{{< /katex >}} is padding,
- {{< katex >}}S{{< /katex >}} is stride.

---

For odd kernels such as `3 × 3` with stride 1, same padding is commonly:

{{% colour "green" %}}
{{< katex display=true >}}
P=\frac{K-1}{2}
{{< /katex >}}
{{% /colour %}}

**Example**

Input size `32 × 32`, kernel `3 × 3`, stride `1`, padding `1`:

{{% colour "green" %}}
{{< katex display=true >}}
H_{out}
=
W_{out}
=
\left\lfloor
\frac{32+2(1)-3}{1}
\right\rfloor
+1
=
32
{{< /katex >}}
{{% /colour %}}

So the output spatial size is preserved.

## Stride

**Stride** tells us how far the filter moves each time.

- **Stride 1**  
  move one step at a time, produce more overlap, retain more spatial detail.

- **Stride 2**  
  jump two steps at a time, reduce spatial size, lower computation.

### Effect of increasing stride

Increasing stride:

- reduces spatial resolution,
- reduces computation,
- may discard fine detail.

So stride acts as both:

- a feature extraction setting,
- and a downsampling setting.

## Strided convolution vs pooling

Both can reduce spatial dimensions, but they behave differently.

### Pooling

- fixed operation,
- no learnable parameters,
- summarises a neighbourhood.

### Strided convolution

- learnable operation,
- extracts features and downsamples at the same time.

Modern architectures often replace some pooling layers with strided convolution.

## Dilated convolution

A **dilated convolution** inserts gaps between kernel elements so the receptive field grows without increasing the number of parameters.

It is especially useful when we want:

- a larger receptive field,
- preserved spatial resolution,
- dense prediction tasks such as segmentation.

{{% colour "green" %}}
{{< katex display=true >}}
K_{\text{eff}}
=
K + (K-1)(r-1)
{{< /katex >}}
{{% /colour %}}

Where:
- {{< katex >}}K{{< /katex >}} is kernel size,
- {{< katex >}}r{{< /katex >}} is dilation rate.

For a `3 × 3` kernel with dilation rate `2`:

{{% colour "green" %}}
{{< katex display=true >}}
K_{\text{eff}}
=
3 + (3-1)(2-1)
=
5
{{< /katex >}}
{{% /colour %}}

So a `3 × 3` kernel acts like a `5 × 5` receptive field while keeping the same number of weights.

### Dilated vs strided convolution

- **Strided convolution** reduces spatial size and is often used in classification pipelines.
- **Dilated convolution** keeps resolution higher while increasing context and is often useful in dense prediction problems.

## Multiple channels

Real images are multi-channel, and deeper CNNs usually increase channels while decreasing spatial size.

A common progression is:

- high resolution, few channels in early layers,
- lower resolution, many channels in deeper layers.

This follows a useful design principle:

> spatial size decreases while semantic depth increases.

### Why channels increase

- early layers capture simple local patterns,
- deeper layers need many channels to represent richer semantic features,
- increasing channels preserves representational capacity even when height and width shrink.

## Pooling layers

Pooling reduces the spatial dimensions of feature maps.

### Why pooling is useful

- reduces computation,
- makes features less sensitive to small shifts,
- helps regularisation,
- indirectly increases effective receptive field.

### Max pooling

Max pooling takes the maximum value in each local window.

It keeps the strongest local activation and is the most common classical pooling method.

### Average pooling

Average pooling takes the mean value in each local region.

It performs smoother downsampling and is often used later in a network.

### Global average pooling

Global average pooling averages each channel over the full spatial map and produces one value per channel. It can replace dense layers and greatly reduce parameters.

### Exams

Pooling layers have **zero learnable parameters**.

{{% colour "green" %}}
{{< katex display=true >}}
\text{Parameters in pooling layers} = 0
{{< /katex >}}
{{% /colour %}}

**`1 × 1` convolution**

A `1 × 1` convolution does not combine neighbouring spatial pixels. Instead, it mixes information **across channels** at each spatial location.

**Why `1 × 1` convolution is useful**

It can:

- change the number of channels,
- perform a learned linear combination across channels,
- reduce computation before expensive `3 × 3` or `5 × 5` convolutions,
- act as a bottleneck layer,
- add non-linearity when followed by an activation.

This idea became central in:

- Network in Network,
- Inception / GoogLeNet,
- ResNet bottleneck blocks,
- many efficient modern CNNs.

## ReLU activation

After convolution, the output usually passes through a non-linear activation. The most common choice is **ReLU**.

{{% colour "green" %}}
{{< katex display=true >}}
\text{ReLU}(x)=\max(0,x)
{{< /katex >}}
{{% /colour %}}

### Why ReLU is popular

- simple and computationally efficient,
- adds non-linearity,
- helps reduce vanishing gradient issues compared with sigmoid and tanh,
- often creates sparse activations.

### Limitation

If a unit keeps receiving negative values, it may remain inactive. This is the **dying ReLU** problem.

## The classifier head: flatten, dense layer, softmax

In a classical CNN, after feature extraction, the representation is fed to a classifier.

### Fully connected layer

A dense layer combines the extracted features and maps them to class scores.

If the input vector has size \(n\) and the output has size \(m\):

{{% colour "green" %}}
{{< katex display=true >}}
y=f(Wx+b)
{{< /katex >}}
{{% /colour %}}

{{% colour "green" %}}
{{< katex display=true >}}
\text{Parameters in FC layer}
=
n\times m + m
{{< /katex >}}
{{% /colour %}}

This is why dense layers are usually much more parameter-heavy than convolutional layers.

### Softmax output

For multi-class classification, the final layer often uses **softmax**.

{{% colour "green" %}}
{{< katex display=true >}}
\text{softmax}(z_i)
=
\frac{e^{z_i}}
{\sum_j e^{z_j}}
{{< /katex >}}
{{% /colour %}}

It converts raw class scores into a probability distribution whose entries sum to 1.

## A typical CNN architecture

A standard pattern is:

`Input → Conv → ReLU → Pool → Conv → ReLU → Pool → Dense / GAP → Softmax`

### Layer-by-layer interpretation

- **Early layers**  
  edges, corners, local gradients.

- **Middle layers**  
  textures, motifs, repeated patterns.

- **Deeper layers**  
  object parts and class-specific structure.

{{< mermaid >}}
flowchart LR
    A[Input image] --> B[Conv plus ReLU]
    B --> C[Pooling]
    C --> D[Conv plus ReLU]
    D --> E[Pooling]
    E --> F[Dense or GAP]
    F --> G[Softmax output]

    style A fill:#E1F5FE
    style B fill:#C8E6C9
    style C fill:#FFF9C4
    style D fill:#EDE7F6
    style E fill:#C8E6C9
    style F fill:#FFF9C4
    style G fill:#FFE0B2
{{< /mermaid >}}

## CNN vs MLP for images

This comparison is often asked directly.

| Aspect | MLP | CNN |
|---|---|---|
| Uses spatial structure | No | Yes |
| Parameter sharing | No | Yes |
| Local connectivity | No | Yes |
| Parameter efficiency for images | Poor | Strong |
| Translation handling | Weak | Much better |
| Hierarchical feature learning | Limited | Natural and strong |

### Bottom line

A multilayer perceptron can classify images, but it ignores the geometry that makes images meaningful. CNNs are designed specifically to exploit that structure.

## Examples

### Example 1: output size after convolution

Input:

- `32 × 32`,
- kernel `5 × 5`,
- padding `0`,
- stride `1`.

{{% colour "green" %}}
{{< katex display=true >}}
H_{out}
=
W_{out}
=
\left\lfloor
\frac{32+2(0)-5}{1}
\right\rfloor
+1
=
28
{{< /katex >}}
{{% /colour %}}

So the output is `28 × 28`.

### Example 2: convolutional parameter count

Suppose:

- kernel `3 × 3`,
- input channels = 3,
- output channels = 64,
- bias included.

{{% colour "green" %}}
{{< katex display=true >}}
(3\times 3\times 3 + 1)\times 64
=
(27+1)\times 64
=
1792
{{< /katex >}}
{{% /colour %}}

So the layer has **1792 learnable parameters**.

### Example 3: why pooling has no parameters

A `2 × 2` max-pooling layer simply takes the maximum in each local window. Since it does not learn weights or biases:

{{% colour "green" %}}
{{< katex display=true >}}
0
{{< /katex >}}
{{% /colour %}}

## Exams Summary

You should be able to explain clearly:

1. Why images need CNNs instead of plain fully connected networks  
2. What a kernel or filter is  
3. What a feature map is  
4. How padding changes output size  
5. How stride changes output size and computation  
6. What receptive field means  
7. Difference between pooling and strided convolution  
8. Role of multi-channel convolution  
9. Why `1 × 1` convolution is useful  
10. Why ReLU is commonly used  
11. Why softmax is used at the end of multi-class CNNs  
12. How a standard CNN turns an input image into a class prediction  

## Quick viva questions

- Why is parameter sharing so important in CNNs?
- Why does increasing stride reduce spatial resolution?
- How is dilated convolution different from strided convolution?
- Why do pooling layers not have learnable parameters?
- Why can CNNs handle images better than MLPs?
- Why is `1 × 1` convolution powerful even though it has no spatial neighbourhood?

## References

- Lecture transcripts:
  - convolution
  - pooling
  - padding
  - stride
  - receptive field
  - channels
  - ReLU
  - softmax
  - AlexNet
  - VGG
  - ResNet
  - transfer learning
- Zhang, A., Lipton, Z. C., Li, M., & Smola, A. J. *Dive into Deep Learning*
- Goodfellow, Bengio, and Courville. *Deep Learning*
- [CNN in ML](https://www.geeksforgeeks.org/deep-learning/convolutional-neural-network-cnn-in-machine-learning/)

---

{{< home-link "Home" >}} | {{< section-index >}}

---
