---
title: 'Recurrent Neural Networks'
date: 2026-04-19
draft: false

weight: 700
tags:
  - Deep Learning
  - RNN
  - Recurrent Neural Networks
  - Sequence Modelling
  - BPTT
  - Encoder Decoder
  - Teacher Forcing
  - Time Series
categories:
  - ai
  - deep-learning
---

# Recurrent Neural Networks  

Recurrent Neural Networks (RNNs) are neural networks designed for **sequential data**, where the order of inputs matters and the model must use information from earlier time steps to interpret later ones. Unlike a feedforward network, an RNN does not process each input in isolation. It carries a **hidden state** from one time step to the next, so the network can build a running summary of what it has seen so far.

{{% hint info %}}
**Key takeaway:**  
A vanilla RNN solves the main limitation of feedforward networks on sequences by introducing a **recurrent hidden state**. The same parameters are reused at every time step, allowing the model to process variable-length sequences and learn temporal patterns through **Backpropagation Through Time (BPTT)**.
{{% /hint %}}

{{% hint warning %}}
**Exam focus:**  
Make sure you can explain:
- why sequence models are needed,
- why feedforward networks fail on variable-length sequential data,
- autoregressive models vs Markov assumptions vs hidden-state models,
- the vanilla RNN equations,
- parameter sharing across time,
- BPTT and why gradients flow backward through time,
- vanishing gradients,
- encoder–decoder intuition,
- teacher forcing,
- and when RNNs are suitable for NLP and time-series tasks.
{{% /hint %}}

## Where this fits in the course

In the course handout, the **Recurrent Neural Networks** session covers:

- raw text into sequence data,
- recurrent connection,
- recurrent neural networks with hidden states,
- backpropagation through time,
- RNNs for NLP tasks,
- encoder–decoder architecture,
- teacher forcing,
- loss function with masking,
- training and prediction.

That is the structure followed in this page. The lecture transcript also repeatedly explains the topic in the same flow: start from sequence modelling, discuss why a fixed-context feedforward model is limited, introduce hidden state as a compressed summary of the past, and then move to BPTT, sequence prediction, and encoder–decoder style training.

## Why sequence models are needed

Many important data sources are sequential:

- text,
- speech,
- sensor streams,
- financial time series,
- weather sequences,
- video frame sequences,
- biological sequences.

In these problems, **order matters**. The meaning of the current element depends on earlier elements.

Examples:

- In language, the next word depends on previous words.
- In time series, tomorrow's value depends on earlier observations.
- In speech, the current sound depends on neighbouring sounds and longer context.

### Why feedforward networks fail on sequences

A standard feedforward network is a poor fit for sequences because:

1. **it assumes fixed-size input**,
2. **it has no memory of previous inputs**,
3. **it does not naturally share the same transition rule across time**,
4. **it cannot easily generalise to arbitrary sequence lengths**.

The transcripts strongly emphasise this point: for a sentence such as *“I grew up in France, so I speak fluent …”*, the network needs information from much earlier in the sequence to predict the missing word correctly.

{{< mermaid >}}
flowchart TD
    A["Sequential input\nwords, audio, sensor values"]:::p1 --> B["Order matters"]:::p2
    B --> C["Need memory of past"]:::p3
    C --> D["Feedforward network is limited"]:::p4
    D --> E["Use recurrent hidden state"]:::p5

    classDef p1 fill:#FDE2E4,stroke:#D8A6AE,color:#333;
    classDef p2 fill:#E2ECE9,stroke:#A8C8BE,color:#333;
    classDef p3 fill:#FFF1E6,stroke:#E6C1A1,color:#333;
    classDef p4 fill:#E8E8F8,stroke:#B8B8DD,color:#333;
    classDef p5 fill:#E3F2FD,stroke:#A6C8E0,color:#333;
{{< /mermaid >}}

## From raw sequence to prediction

For sequence modelling, we often represent the sequence as:

{{% colour "green" %}}
{{< katex display=true >}}
(x_1, x_2, x_3, \, \dots, x_T)
{{< /katex >}}
{{% /colour %}}

At each time step, the model receives one element and updates its internal memory.

Typical output styles:

- **many-to-one**: one output after reading the full sequence,
- **many-to-many**: one output for each input step,
- **sequence-to-sequence**: one input sequence mapped to another output sequence.

Examples:

- sentiment analysis: many-to-one,
- part-of-speech tagging: many-to-many,
- translation: sequence-to-sequence,
- next-step forecasting: many-to-one or many-to-many.

## Autoregressive modelling

Before introducing RNNs, it is useful to understand **autoregressive thinking**.

If a sequence is

{{% colour "green" %}}
{{< katex display=true >}}
(x_1, x_2, \dots, x_T)
{{< /katex >}}
{{% /colour %}}

then the full joint probability can be decomposed as:

{{% colour "green" %}}
{{< katex display=true >}}
P(x_1, x_2, \dots, x_T) = \prod_{t=1}^{T} P(x_t \mid x_1, x_2, \dots, x_{t-1})
{{< /katex >}}
{{% /colour %}}

This says that each new item depends on the full history.

### The practical difficulty

The history grows as the sequence grows. That becomes difficult to model directly.

## Markov assumption

The transcript explains the next simplification very clearly: instead of conditioning on the full past, **limit the context window**.

### First-order Markov assumption

{{% colour "green" %}}
{{< katex display=true >}}
P(x_t \mid x_1, \dots, x_{t-1}) \approx P(x_t \mid x_{t-1})
{{< /katex >}}
{{% /colour %}}

### k-th order Markov assumption

{{% colour "green" %}}
{{< katex display=true >}}
P(x_t \mid x_1, \dots, x_{t-1}) \approx P(x_t \mid x_{t-k}, \dots, x_{t-1})
{{< /katex >}}
{{% /colour %}}

### Strength and weakness

**Advantage:**
- fixed context size,
- easier computation,
- simpler modelling.

**Limitation:**
- older information is discarded,
- long-range dependencies are lost,
- the chosen window may be too small.

This is exactly why hidden-state sequence models are introduced.

## Hidden-state or latent-variable view

Instead of keeping the entire history explicitly, the model keeps a **compressed summary** called the hidden state.

{{% colour "green" %}}
{{< katex display=true >}}
h_t = f(x_t, h_{t-1})
{{< /katex >}}
{{% /colour %}}

The transcript repeatedly explains this idea as **“summarise the past with the hidden state”**.

The hidden state is a latent variable that stores useful contextual information from the earlier sequence.

{{% colour "green" %}}
{{< katex display=true >}}
P(x_t \mid x_1, \dots, x_{t-1}) \approx P(x_t \mid h_t)
{{< /katex >}}
{{% /colour %}}

This is the central idea behind RNNs.

## Vanilla RNN architecture

A vanilla RNN processes the sequence one step at a time. At each time step:

1. read the current input `x_t`,
2. combine it with previous hidden state `h_{t-1}`,
3. compute new hidden state `h_t`,
4. optionally produce output `o_t`.

{{< mermaid >}}
flowchart LR
    X1["x₁"]:::p1 --> H1["h₁"]:::p2
    X2["x₂"]:::p1 --> H2["h₂"]:::p2
    X3["x₃"]:::p1 --> H3["h₃"]:::p2
    H1 --> H2 --> H3
    H1 --> O1["o₁"]:::p3
    H2 --> O2["o₂"]:::p3
    H3 --> O3["o₃"]:::p3

    classDef p1 fill:#FFF1E6,stroke:#E6C1A1,color:#333;
    classDef p2 fill:#E2ECE9,stroke:#A8C8BE,color:#333;
    classDef p3 fill:#E3F2FD,stroke:#A6C8E0,color:#333;
{{< /mermaid >}}

### Unrolled view across time

An RNN is easier to understand when unrolled over time. Even though it looks like multiple copies, the same weights are reused at every time step.

## Mathematical formulation of a vanilla RNN

A common form is:

{{% colour "green" %}}
{{< katex display=true >}}
h_t = \phi(W_{xh}x_t + W_{hh}h_{t-1} + b_h)
{{< /katex >}}
{{% /colour %}}

{{% colour "green" %}}
{{< katex display=true >}}
o_t = W_{ho}h_t + b_o
{{< /katex >}}
{{% /colour %}}

If the task is classification over classes, we may convert scores to probabilities using softmax:

{{% colour "green" %}}
{{< katex display=true >}}
\hat{y}_t = \text{softmax}(o_t)
{{< /katex >}}
{{% /colour %}}

Where:

- `x_t` is the input at time `t`,
- `h_t` is the hidden state,
- `W_{xh}` maps input to hidden,
- `W_{hh}` is the recurrent transition matrix,
- `W_{ho}` maps hidden state to output,
- `\phi` is typically `tanh` in vanilla RNNs.

## Why parameter sharing matters

One of the most important RNN ideas is **parameter sharing across time**.

The same matrices are reused at every time step:

- `W_{xh}`
- `W_{hh}`
- `W_{ho}`
- biases

This has several benefits:

- the number of parameters does **not** grow with sequence length,
- the model can process variable-length inputs,
- a temporal pattern learned at one position can be reused elsewhere.

### Parameter count

If:

- input dimension = `d`,
- hidden dimension = `h`,
- output dimension = `V`,

then the total number of trainable parameters is:

{{% colour "green" %}}
{{< katex display=true >}}
hd + h^2 + h + Vh + V
{{< /katex >}}
{{% /colour %}}

or equivalently:

{{% colour "green" %}}
{{< katex display=true >}}
h(d + h + V + 1) + V
{{< /katex >}}
{{% /colour %}}

## Activation functions in vanilla RNNs

The slides highlight two common choices:

### tanh

{{% colour "green" %}}
{{< katex display=true >}}
\phi(z) = \tanh(z)
{{< /katex >}}
{{% /colour %}}

Why it is common:

- output is bounded in `[-1, 1]`,
- zero-centred,
- stable for many simple RNN formulations.

### ReLU

{{% colour "green" %}}
{{< katex display=true >}}
\phi(z) = \max(0, z)
{{< /katex >}}
{{% /colour %}}

Possible benefit:
- can reduce vanishing gradient in some settings.

Possible drawback:
- may create exploding activations or instability.

## Training objective for sequences

For sequence classification or prediction, the total loss is usually the sum or mean over time steps.

If the model predicts a target `y_t` at each time step, then a common total loss is:

{{% colour "green" %}}
{{< katex display=true >}}
\mathcal{L} = \sum_{t=1}^{T} \ell(\hat{y}_t, y_t)
{{< /katex >}}
{{% /colour %}}

For example:

- cross-entropy for token prediction,
- binary cross-entropy for binary sequence outputs,
- mean squared error for regression and forecasting.

## Backpropagation Through Time (BPTT)

The slides describe BPTT as the method used to train RNNs because the hidden state at a time step depends on all earlier steps.

The transcript gives a very direct summary:

- compute outputs for all time steps,
- compute gradients from `t = T` back to `t = 1`,
- gradients flow backward through time,
- contributions from all time steps are accumulated because parameters are shared.

{{< mermaid >}}
flowchart RL
    L3["Loss at t = T"]:::p4 --> H3["h_T"]:::p2
    L2["Loss at t = T-1"]:::p4 --> H2["h_{T-1}"]:::p2
    L1["Loss at t = 1"]:::p4 --> H1["h_1"]:::p2
    H3 --> H2 --> H1
    X3["x_T"]:::p1 --> H3
    X2["x_{T-1}"]:::p1 --> H2
    X1["x_1"]:::p1 --> H1

    classDef p1 fill:#FFF1E6,stroke:#E6C1A1,color:#333;
    classDef p2 fill:#E2ECE9,stroke:#A8C8BE,color:#333;
    classDef p4 fill:#FDE2E4,stroke:#D8A6AE,color:#333;
{{< /mermaid >}}

### Why BPTT is needed

Because:

- `h_t` depends on `h_{t-1}`,
- `h_{t-1}` depends on `h_{t-2}`,
- and so on,

so the loss at the end of the sequence depends on parameters used much earlier.

### Key idea

Unroll the RNN into a time-expanded computational graph, then apply ordinary backpropagation over that graph.

## Vanishing gradients in vanilla RNNs

This is one of the biggest limitations of simple RNNs.

When the gradient is repeatedly multiplied across many time steps, it can shrink rapidly toward zero.

### Consequences

- early time steps receive very weak learning signals,
- the model struggles with long-term dependencies,
- long-range context is often forgotten.

This is why vanilla RNNs are usually poor at very long sequences.

{{% hint warning %}}
**Important intuition:**  
A vanilla RNN can, in theory, represent long-term dependencies, but in practice the optimisation becomes difficult because the gradient signal fades as it travels backward through many time steps.
{{% /hint %}}

## Encoder–decoder idea

For tasks where the input and output are both sequences, an encoder–decoder architecture is often used.

### Encoder

The encoder reads the full input sequence and converts it into a context representation.

### Decoder

The decoder uses that context to generate the output sequence step by step.

{{< mermaid >}}
flowchart LR
    A["Input sequence"]:::p1 --> B["Encoder RNN"]:::p2
    B --> C["Context / final state"]:::p3
    C --> D["Decoder RNN"]:::p2
    D --> E["Output sequence"]:::p4

    classDef p1 fill:#E3F2FD,stroke:#A6C8E0,color:#333;
    classDef p2 fill:#E2ECE9,stroke:#A8C8BE,color:#333;
    classDef p3 fill:#FFF1E6,stroke:#E6C1A1,color:#333;
    classDef p4 fill:#FDE2E4,stroke:#D8A6AE,color:#333;
{{< /mermaid >}}

This is useful for:

- machine translation,
- summarisation,
- text generation,
- some forecasting setups.

## Teacher forcing

The transcript explicitly explains teacher forcing as a **training-time strategy** where the decoder is fed the **ground-truth previous output** instead of its own previous prediction.

### Why it helps

- training becomes more stable,
- the decoder learns the next-step mapping more directly,
- error accumulation during training is reduced.

### But note the train–test gap

At inference time, the model usually does **not** have the ground-truth next token. It must feed back its own generated output.

That difference between training and inference is important in sequence generation problems.

## Loss masking

In variable-length sequence tasks, many mini-batches include padded tokens. We do not want those padded positions to influence the loss.

So a mask is used to ensure the loss is computed only on valid sequence positions.

Conceptually:

{{% colour "green" %}}
{{< katex display=true >}}
\mathcal{L}_{\text{masked}} = \sum_{t=1}^{T} m_t\,\ell(\hat{y}_t, y_t)
{{< /katex >}}
{{% /colour %}}

where:

- `m_t = 1` for a valid position,
- `m_t = 0` for a padded position.

## RNNs for NLP tasks

Vanilla RNNs were historically used for:

- next-word prediction,
- sequence tagging,
- language modelling,
- text generation,
- encoder–decoder translation.

They are conceptually important because they introduce the idea of stateful sequence processing, even though modern transformer models now dominate many NLP tasks.

## RNNs for time-series forecasting

The slides also connect RNNs to forecasting.

Typical tasks include:

- next-step temperature prediction,
- stock price modelling,
- demand forecasting,
- sensor forecasting.

### Single-step forecasting

Predict only the next value:

{{% colour "green" %}}
{{< katex display=true >}}
\hat{x}_{t+1} = f(x_1, x_2, \dots, x_t)
{{< /katex >}}
{{% /colour %}}

### Multi-step forecasting

Predict several future values:

{{% colour "green" %}}
{{< katex display=true >}}
[\hat{x}_{t+1}, \hat{x}_{t+2}, \dots, \hat{x}_{t+k}] = f(x_1, x_2, \dots, x_t)
{{< /katex >}}
{{% /colour %}}

### Univariate vs multivariate

- **univariate**: one variable over time,
- **multivariate**: multiple correlated variables over time.

The slides note that multivariate forecasting can capture relationships such as humidity influencing temperature prediction.

## Advantages of vanilla RNNs

- handles variable-length sequences,
- shares parameters across time,
- introduces temporal memory,
- conceptually simple,
- foundation for GRU, LSTM, and sequence-to-sequence models.

## Limitations of vanilla RNNs

- vanishing gradients,
- difficulty with long-term dependencies,
- sequential computation reduces parallelism,
- training can be unstable on long sequences,
- weaker than gated recurrent models on harder problems.

## Exam-style comparison: feedforward vs autoregressive vs RNN

| Model | Main idea | Strength | Limitation |
|---|---|---|---|
| Feedforward network | Fixed-size independent input | Simple | No memory, fixed input length |
| Markov / autoregressive | Limited recent context | Efficient | Loses long-range context |
| Vanilla RNN | Hidden state summarises past | Flexible sequence model | Vanishing gradients |

## What comes next

Vanilla RNNs are the starting point, but they are not the end of the story. The next page extends this topic into **Deep Recurrent Neural Networks**, where we study:

- stacked / deep RNNs,
- bidirectional RNNs,
- GRU,
- LSTM,
- gated memory,
- and why these models work better on long-range dependencies.

## References

- Course handout: Deep Neural Networks, Session 9 topic structure.
- DNN Module 7 slides on sequence modelling, RNNs, BPTT, encoder–decoder, teacher forcing, and time-series forecasting.
- Lecture transcripts explaining Markov context windows, hidden-state summary of the past, BPTT gradient flow, and teacher forcing with ground-truth tokens.

- **Dive into deep learning. Cambridge University Press.**. (T1 – Ch 9)
- R4 – Ch 9
- R3 – Ch 2.2.10

---
{{< home-link "Home" >}} | {{< section-index >}}
---
