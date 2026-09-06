---
title: "NN and Neural Language Modelling"
draft: false
tags: ["Natural Language Processing", "NLP", "Neural Networks", "Feed-Forward Networks", "Neural Language Models", "Word Embeddings"]
categories: ["AI", "ML"]
weight: 400
menu: main
---

# Neural Networks and Neural Language Modelling

Neural networks learn useful representations and nonlinear relationships directly from data. In language modelling, they replace discrete N-gram identities with learned word embeddings and use these representations to predict the next word.

## Learning Objectives

- Explain the computation performed by a neural unit.
- Describe why hidden layers and nonlinear activations are needed.
- Explain how feed-forward networks support NLP classification.
- Trace the flow through a feed-forward neural language model.
- Compare N-gram and neural language models.

## Big Picture

{{< mermaid >}}
flowchart TD
    A["Context Words"] --> B["One-hot Inputs"]
    B --> C["Embedding Lookup"]
    C --> D["Combined Context"]
    D --> E["Hidden Layer"]
    E --> F["Softmax"]
    F --> G["Next-word Probabilities"]

    style A fill:#E1F5FE
    style B fill:#C8E6C9
    style C fill:#FFF9C4
    style D fill:#EDE7F6
    style E fill:#E1F5FE
    style F fill:#C8E6C9
    style G fill:#FFF9C4
{{< /mermaid >}}

## 1. Neural Network Units ☆

A neural unit receives input values, multiplies them by learned weights, adds a bias, and applies an activation function.

{{% colour "red" %}}
{{< katex display=true >}}
z=\mathbf{w}^{\mathsf T}\mathbf{x}+b
{{< /katex >}}
{{% /colour %}}

{{% colour "red" %}}
{{< katex display=true >}}
a=f(z)
{{< /katex >}}
{{% /colour %}}

The weights control the influence of each input. The bias shifts the decision boundary, and the activation function determines the unit's output.

### Sigmoid

{{% colour "red" %}}
{{< katex display=true >}}
\sigma(z)=\frac{1}{1+e^{-z}}
{{< /katex >}}
{{% /colour %}}

Sigmoid maps a real-valued score to the interval from 0 to 1, making it suitable for a binary output.

### Tanh and ReLU

The hyperbolic tangent produces values between -1 and 1. ReLU keeps positive inputs and maps negative inputs to zero.

{{% colour "red" %}}
{{< katex display=true >}}
\operatorname{ReLU}(z)=\max(0,z)
{{< /katex >}}
{{% /colour %}}

## 2. Why Nonlinearity Matters

A single linear unit cannot represent every decision boundary. The XOR problem is the classic example: no single straight line can separate its positive and negative cases.

A multilayer network combines several learned boundaries. Nonlinear activation functions allow these combinations to represent patterns that a purely linear model cannot.

{{% hint info %}}
Stacking linear transformations without nonlinear activations still produces only another linear transformation. The activation function gives a multilayer network its extra expressive power.
{{% /hint %}}

## 3. Feed-Forward Neural Networks ☆

A feed-forward neural network, also called a multilayer perceptron, sends information from input to output without feedback loops.

```text
Input layer → Hidden layer or layers → Output layer
```

For a network with one hidden layer:

{{% colour "red" %}}
{{< katex display=true >}}
\mathbf{h}=g(\mathbf{W}\mathbf{x}+\mathbf{b})
{{< /katex >}}
{{% /colour %}}

{{% colour "red" %}}
{{< katex display=true >}}
\hat{\mathbf{y}}=\operatorname{softmax}(\mathbf{U}\mathbf{h}+\mathbf{c})
{{< /katex >}}
{{% /colour %}}

The hidden representation {{< katex >}} \mathbf{h} {{< /katex >}} captures combinations of input features. The output layer converts it into predictions.

## 4. Softmax for Multiple Classes ☆

Sigmoid is suited to a binary decision. Softmax generalises probability prediction to multiple mutually exclusive classes.

{{% colour "red" %}}
{{< katex display=true >}}
\operatorname{softmax}(\mathbf{z})_i=\frac{e^{z_i}}{\sum_{j=1}^{K}e^{z_j}}
{{< /katex >}}
{{% /colour %}}

The outputs are non-negative and sum to 1. In a language model, each output position represents a vocabulary word.

## 5. Feed-Forward Networks for NLP

### Text Classification

A text classifier can predict a class such as positive or negative sentiment.

A single-layer network behaves like logistic regression and learns a linear boundary. Adding a hidden layer allows nonlinear interactions between features.

The deeper benefit is **representation learning**: instead of relying only on manually designed features, the network learns useful features from the data. Word embeddings are an important example.

### Variable-length Text

Basic feed-forward networks expect a fixed-size input, but sentences and documents vary in length. Simple approaches include:

- padding or truncating text to a fixed length
- taking the mean of all word embeddings
- taking the element-wise maximum across word embeddings

These methods create a fixed-size representation, though they may lose word-order information.

## 6. Neural Language Models ☆

A neural language model predicts the next word from the preceding context.

Like an N-gram model, a basic feed-forward neural language model uses a fixed-size sliding window. The important difference is its representation:

- an N-gram model treats words and contexts as discrete identities
- a neural model represents words with learned continuous vectors

Suppose the context contains three words. Each word is first represented as a one-hot vector and then mapped through an embedding matrix. The three embeddings are concatenated, passed through a hidden layer, and finally through softmax.

{{% colour "red" %}}
{{< katex display=true >}}
\mathbf{e}=[\mathbf{E}\mathbf{x}_{t-3};\mathbf{E}\mathbf{x}_{t-2};\mathbf{E}\mathbf{x}_{t-1}]
{{< /katex >}}
{{% /colour %}}

{{% colour "red" %}}
{{< katex display=true >}}
\mathbf{h}=g(\mathbf{W}\mathbf{e}+\mathbf{b})
{{< /katex >}}
{{% /colour %}}

{{% colour "red" %}}
{{< katex display=true >}}
P(w_t\mid w_{t-3},w_{t-2},w_{t-1})=\operatorname{softmax}(\mathbf{U}\mathbf{h}+\mathbf{c})
{{< /katex >}}
{{% /colour %}}

## 7. Why Neural Models Generalise Better

Consider:

```text
Training: I have to make sure that the cat gets fed.
Test:     I forgot to make sure that the dog gets ____.
```

An N-gram model may have little or no evidence for the new sequence. A neural model can exploit the similarity between the learned vectors for `cat` and `dog` and predict `fed`.

{{% hint success %}}
Dense embeddings allow evidence learned for one word or context to help with a semantically similar word or context.
{{% /hint %}}

## 8. Training the Network ☆

Training repeatedly performs:

1. A forward pass to compute a prediction.
2. A loss calculation to measure prediction error.
3. Backpropagation to calculate gradients.
4. A parameter update to reduce future error.

For a one-hot target in multiclass prediction, cross-entropy loss is:

{{% colour "red" %}}
{{< katex display=true >}}
L=-\sum_{i=1}^{K}y_i\log(\hat{y}_i)
{{< /katex >}}
{{% /colour %}}

Parameters are updated in the direction that reduces the loss:

{{% colour "red" %}}
{{< katex display=true >}}
\theta\leftarrow\theta-\eta\nabla_{\theta}L
{{< /katex >}}
{{% /colour %}}

Here {{< katex >}} \eta {{< /katex >}} is the learning rate.

### Embedding Choices

Two approaches are possible:

| Approach | What happens |
|---|---|
| Fixed embeddings | Start with pretrained vectors and keep them unchanged |
| Jointly trained embeddings | Initialise embeddings and update them with the rest of the model |

Joint training is often useful when enough task-specific data is available.

## 9. N-gram Models and Neural Language Models

| Property | N-gram model | Feed-forward neural model |
|---|---|---|
| Representation | Discrete counts | Distributed embeddings |
| Context | Fixed | Fixed sliding window |
| Generalisation | Limited for unseen combinations | Better across similar words |
| Computation | Simpler | More expensive |
| Interpretability | Higher | Lower |

The feed-forward neural model still has a fixed context window, so it does not fully solve long-distance dependency problems. More advanced architectures address this limitation.

## Common Mistakes

{{% hint warning %}}
- More layers do not help if every layer remains purely linear.
- Softmax produces a probability distribution across classes; it is not the same as selecting the largest score.
- A feed-forward neural language model still uses limited context.
- Word embeddings may be fixed or learned jointly; they are not automatically pretrained.
{{% /hint %}}

## Practice Questions

1. What roles do the weights, bias and activation function play in a neural unit?
2. Why can a hidden layer solve patterns that a single linear unit cannot?
3. Why is softmax used at the output of a neural language model?
4. How do embeddings help a neural model generalise from `cat` to `dog`?
5. Compare fixed embeddings with jointly trained embeddings.

## Key Takeaways

{{% hint success %}}
- A neural unit applies a weighted sum, bias and nonlinear activation.
- Hidden layers learn nonlinear feature interactions and useful representations.
- A feed-forward neural language model maps context words to embeddings and predicts the next word with softmax.
- Learned embeddings support generalisation across similar words and contexts.
- Training uses a forward pass, loss, backpropagation and parameter updates.
{{% /hint %}}

## Checklist

- [ ] I can describe a neural unit mathematically and intuitively.
- [ ] I can explain why nonlinear activation functions matter.
- [ ] I can trace data through a feed-forward neural language model.
- [ ] I can explain how the model is trained.
- [ ] I can compare N-gram and neural language models.

---
{{< home-link "Home" >}} | {{< section-index >}}
