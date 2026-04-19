---
title: 'Deep Recurrent Neural Networks'
date: 2026-04-19
draft: false
weight: 750
tags:
  - Deep Learning
  - Deep RNN
  - LSTM
  - GRU
  - Bidirectional RNN
  - Stacked RNN
  - Sequence Modelling
categories:
  - ai
  - deep-learning
---

# Deep Recurrent Neural Networks  

Vanilla RNNs introduce the hidden-state idea, but they struggle on longer and more complex sequences because gradients can vanish across time. Deep recurrent models extend the RNN idea in two important ways:

1. **make the recurrent architecture richer**, for example by stacking multiple recurrent layers or using information from both directions,
2. **use gates and memory cells** to control what should be remembered, forgotten, updated, and exposed.

This is why practical recurrent modelling usually moves from a simple RNN to **stacked RNNs, bidirectional RNNs, GRUs, or LSTMs**.

{{% hint info %}}
**Key takeaway:**  
Deep RNNs improve vanilla RNNs by adding either **depth**, **two-directional context**, or **gating mechanisms**. In practice, the most important deep recurrent architectures are **stacked RNNs**, **bidirectional RNNs**, **GRUs**, and **LSTMs**.
{{% /hint %}}

{{% hint warning %}}
**Exam focus:**  
Be ready to explain:
- why deep recurrent models are needed after vanilla RNN,
- how bidirectional RNNs differ from causal left-to-right models,
- how stacked RNNs create hierarchical sequence features,
- how GRU gates work,
- how LSTM gates and cell state work,
- why LSTM and GRU help with vanishing gradients,
- and when to prefer GRU vs LSTM.
{{% /hint %}}

## Where this fits in the course

In the course handout, the **Deep Recurrent Neural Networks** session covers:

- Long Short-Term Memory (LSTM),
- gated memory cell,
- Gated Recurrent Units (GRU),
- reset gate and update gate,
- candidate hidden state,
- stacked RNN architectures,
- bidirectional recurrent neural networks.

This page follows that structure closely. The lecture transcripts reinforce these same points and also emphasise useful practical observations such as:

- deep RNNs often outperform a wider single-layer RNN,
- a practical depth is often around **2 to 4 layers**,
- bidirectional RNNs should not be used for real-time online causal prediction,
- gated models are introduced mainly to address long-range dependency problems.

## Why we need deep recurrent architectures

A vanilla RNN has two major weaknesses:

1. **limited representational capacity**,
2. **difficulty learning long-range dependencies**.

The slides and transcript both motivate the next step clearly:

- use **depth** to learn hierarchical temporal features,
- use **gates** to improve information flow,
- use **bidirectional context** when the full sequence is available.

{{< mermaid >}}
flowchart TD
    A["Vanilla RNN"]:::p1 --> B["Long-range dependency problem"]:::p2
    A --> C["Limited capacity"]:::p2
    B --> D["Use gates"]:::p3
    C --> E["Use stacked layers"]:::p4
    C --> F["Use both directions if future context is available"]:::p5
    D --> G["GRU / LSTM"]:::p6
    E --> H["Deep RNN"]:::p6
    F --> I["Bidirectional RNN"]:::p6

    classDef p1 fill:#E3F2FD,stroke:#A6C8E0,color:#333;
    classDef p2 fill:#FDE2E4,stroke:#D8A6AE,color:#333;
    classDef p3 fill:#E2ECE9,stroke:#A8C8BE,color:#333;
    classDef p4 fill:#FFF1E6,stroke:#E6C1A1,color:#333;
    classDef p5 fill:#E8E8F8,stroke:#B8B8DD,color:#333;
    classDef p6 fill:#F1F7ED,stroke:#B8D7A3,color:#333;
{{< /mermaid >}}

## Bidirectional RNNs

A standard RNN reads a sequence from left to right, so the state at time `t` only contains **past** information.

That is perfect for causal problems such as:

- online forecasting,
- real-time speech processing,
- next-token prediction.

But many tasks benefit from **both past and future context**.

Examples given in the slides:

- sentiment analysis,
- named entity recognition,
- speech recognition,
- encoder-side sequence understanding.

### Core idea

Run one RNN forward and another backward.

- forward RNN reads from `1` to `T`,
- backward RNN reads from `T` to `1`,
- the two hidden states are combined.

{{< mermaid >}}
flowchart LR
    X1["x₁"]:::p1 --> F1["forward h₁"]:::p2 --> F2["forward h₂"]:::p2 --> F3["forward h₃"]:::p2
    X3["x₃"]:::p1 --> B3["backward h₃"]:::p3 --> B2["backward h₂"]:::p3 --> B1["backward h₁"]:::p3
    F1 --> C1["combine"]:::p4
    B1 --> C1
    F2 --> C2["combine"]:::p4
    B2 --> C2
    F3 --> C3["combine"]:::p4
    B3 --> C3

    classDef p1 fill:#FFF1E6,stroke:#E6C1A1,color:#333;
    classDef p2 fill:#E2ECE9,stroke:#A8C8BE,color:#333;
    classDef p3 fill:#E3F2FD,stroke:#A6C8E0,color:#333;
    classDef p4 fill:#FDE2E4,stroke:#D8A6AE,color:#333;
{{< /mermaid >}}

### Mathematical idea

If the forward and backward states are:

{{% colour "green" %}}
{{< katex display=true >}}
\overrightarrow{h_t} = \phi(W_x^{(f)}x_t + W_h^{(f)}\overrightarrow{h_{t-1}} + b^{(f)})
{{< /katex >}}
{{% /colour %}}

{{% colour "green" %}}
{{< katex display=true >}}
\overleftarrow{h_t} = \phi(W_x^{(b)}x_t + W_h^{(b)}\overleftarrow{h_{t+1}} + b^{(b)})
{{< /katex >}}
{{% /colour %}}

then the combined representation can be concatenation or another combination:

{{% colour "green" %}}
{{< katex display=true >}}
h_t = [\overrightarrow{h_t}; \overleftarrow{h_t}]
{{< /katex >}}
{{% /colour %}}

### When to use bidirectional RNNs

Use them when:

- the full sequence is already available,
- future context improves interpretation,
- the problem is not causal.

Do **not** use them when:

- predictions must be made online,
- future information is unavailable,
- causality matters.

The transcript states this very directly: **bidirectional RNNs are not preferred for real-time or online prediction**.

## Stacked or deep RNNs

A deep RNN places multiple recurrent layers on top of each other.

### Why stack layers?

The slides explain this as hierarchical feature learning in time:

- lower layers capture simpler or shorter-range patterns,
- higher layers capture more abstract or longer-range structure.

Examples mentioned in the slides:

- text: characters → words → phrases → sentences,
- speech: phonemes → syllables → words,
- time series: short-term → medium-term → long-term trends.

{{< mermaid >}}
flowchart TD
    X1["x₁"]:::p1 --> H11["layer 1\nh₁¹"]:::p2 --> H21["layer 2\nh₁²"]:::p3
    X2["x₂"]:::p1 --> H12["layer 1\nh₂¹"]:::p2 --> H22["layer 2\nh₂²"]:::p3
    X3["x₃"]:::p1 --> H13["layer 1\nh₃¹"]:::p2 --> H23["layer 2\nh₃²"]:::p3
    H11 --> H12 --> H13
    H21 --> H22 --> H23

    classDef p1 fill:#FFF1E6,stroke:#E6C1A1,color:#333;
    classDef p2 fill:#E2ECE9,stroke:#A8C8BE,color:#333;
    classDef p3 fill:#E3F2FD,stroke:#A6C8E0,color:#333;
{{< /mermaid >}}

### General formulation for a stacked RNN

For layer `\ell` and time `t`:

{{% colour "green" %}}
{{< katex display=true >}}
h_t^{(\ell)} = \phi\left(W_{x}^{(\ell)} h_t^{(\ell-1)} + W_{h}^{(\ell)} h_{t-1}^{(\ell)} + b^{(\ell)}\right)
{{< /katex >}}
{{% /colour %}}

with the convention:

{{% colour "green" %}}
{{< katex display=true >}}
h_t^{(0)} = x_t
{{< /katex >}}
{{% /colour %}}

### Gradient-flow issue in deep RNNs

A stacked RNN suffers from a **compound gradient problem**:

- gradients must flow across **time**,
- and also across **depth**.

That makes optimisation harder than in a single-layer vanilla RNN.

### Practical lesson from the transcript

The instructor explicitly notes that:

- deep RNNs often outperform wider single-layer RNNs,
- but in practice a depth of around **2 to 4 layers** is common,
- beyond that, diminishing returns and optimisation difficulties become more severe.

## Gated Recurrent Unit (GRU)

GRU is a gated recurrent architecture designed to improve memory control and gradient flow.

### Motivation

The slides summarise the main vanilla RNN problems:

- vanishing gradients,
- all information mixed into one hidden state,
- no explicit mechanism to decide what to keep or forget.

### Core GRU idea

Use **gates** to control information flow.

In the transcript, this is explained intuitively using sigma values:

- if the gate output is near `0`, information is blocked,
- if it is near `1`, information passes through,
- values in between partially allow information through.

### GRU components

- **reset gate** `r_t`
- **update gate** `z_t`
- **candidate hidden state** `\tilde{h}_t`
- **final hidden state** `h_t`

### GRU equations

#### Reset gate

{{% colour "green" %}}
{{< katex display=true >}}
r_t = \sigma(W_r x_t + U_r h_{t-1} + b_r)
{{< /katex >}}
{{% /colour %}}

This decides how much past information should influence the candidate state.

#### Update gate

{{% colour "green" %}}
{{< katex display=true >}}
z_t = \sigma(W_z x_t + U_z h_{t-1} + b_z)
{{< /katex >}}
{{% /colour %}}

This decides how much of the old hidden state to keep versus how much of the new candidate to use.

#### Candidate state

{{% colour "green" %}}
{{< katex display=true >}}
\tilde{h}_t = \tanh(W_h x_t + U_h(r_t \odot h_{t-1}) + b_h)
{{< /katex >}}
{{% /colour %}}

#### Final update

{{% colour "green" %}}
{{< katex display=true >}}
h_t = (1 - z_t) \odot h_{t-1} + z_t \odot \tilde{h}_t
{{< /katex >}}
{{% /colour %}}

### Intuition

- if `z_t` is small, keep old memory,
- if `z_t` is large, replace with new information,
- if `r_t` is small, the candidate ignores much of the old state.

{{< mermaid >}}
flowchart TD
    X["x_t"]:::p1 --> Z["update gate z_t"]:::p2
    Hprev["h_{t-1}"]:::p3 --> Z
    X --> R["reset gate r_t"]:::p2
    Hprev --> R
    X --> Cand["candidate state h~_t"]:::p4
    R --> Cand
    Hprev --> Cand
    Hprev --> Mix["mix old and new"]:::p5
    Cand --> Mix
    Z --> Mix
    Mix --> Hnew["h_t"]:::p6

    classDef p1 fill:#FFF1E6,stroke:#E6C1A1,color:#333;
    classDef p2 fill:#E2ECE9,stroke:#A8C8BE,color:#333;
    classDef p3 fill:#E3F2FD,stroke:#A6C8E0,color:#333;
    classDef p4 fill:#FDE2E4,stroke:#D8A6AE,color:#333;
    classDef p5 fill:#E8E8F8,stroke:#B8B8DD,color:#333;
    classDef p6 fill:#F1F7ED,stroke:#B8D7A3,color:#333;
{{< /mermaid >}}

### Why GRU helps

- improves gradient flow,
- allows selective memory retention,
- simpler than LSTM,
- often trains faster than LSTM,
- works well for many medium-length sequence tasks.

### Parameter counting for GRU

Because there are three affine components, GRU has more parameters than a vanilla RNN but fewer than an LSTM.

If input size is `d` and hidden size is `h`, the parameter count is:

{{% colour "green" %}}
{{< katex display=true >}}
3(hd + h^2 + h)
{{< /katex >}}
{{% /colour %}}

not counting any output projection layer.

## Long Short-Term Memory (LSTM)

LSTM is the most famous gated recurrent architecture.

### Motivation

The slides explain the key LSTM innovation very clearly:

- separate **long-term memory** from **working output state**,
- give finer control over memory flow,
- create a more stable path for gradients.

### Two states in LSTM

- **cell state** `c_t`: long-term memory,
- **hidden state** `h_t`: short-term working state or exposed output.

This is one of the most important conceptual differences between GRU and LSTM.

### Main gates in LSTM

- **forget gate** `f_t`
- **input gate** `i_t`
- **candidate cell state** `\tilde{c}_t`
- **output gate** `o_t`

### LSTM equations

#### Forget gate

{{% colour "green" %}}
{{< katex display=true >}}
f_t = \sigma(W_f x_t + U_f h_{t-1} + b_f)
{{< /katex >}}
{{% /colour %}}

This controls how much of the previous cell state to keep.

#### Input gate

{{% colour "green" %}}
{{< katex display=true >}}
i_t = \sigma(W_i x_t + U_i h_{t-1} + b_i)
{{< /katex >}}
{{% /colour %}}

#### Candidate cell content

{{% colour "green" %}}
{{< katex display=true >}}
\tilde{c}_t = \tanh(W_c x_t + U_c h_{t-1} + b_c)
{{< /katex >}}
{{% /colour %}}

#### Cell state update

{{% colour "green" %}}
{{< katex display=true >}}
c_t = f_t \odot c_{t-1} + i_t \odot \tilde{c}_t
{{< /katex >}}
{{% /colour %}}

#### Output gate

{{% colour "green" %}}
{{< katex display=true >}}
o_t = \sigma(W_o x_t + U_o h_{t-1} + b_o)
{{< /katex >}}
{{% /colour %}}

#### Hidden state

{{% colour "green" %}}
{{< katex display=true >}}
h_t = o_t \odot \tanh(c_t)
{{< /katex >}}
{{% /colour %}}

{{< mermaid >}}
flowchart TD
    Hprev["h_{t-1}"]:::p1 --> F["forget gate f_t"]:::p2
    X["x_t"]:::p3 --> F
    Hprev --> I["input gate i_t"]:::p2
    X --> I
    Hprev --> O["output gate o_t"]:::p2
    X --> O
    Hprev --> Cbar["candidate c~_t"]:::p4
    X --> Cbar
    Cprev["c_{t-1}"]:::p5 --> Cell["cell update"]:::p6
    F --> Cell
    I --> Cell
    Cbar --> Cell
    Cell --> Cnew["c_t"]:::p6
    Cnew --> Hnew["h_t"]:::p7
    O --> Hnew

    classDef p1 fill:#E3F2FD,stroke:#A6C8E0,color:#333;
    classDef p2 fill:#E2ECE9,stroke:#A8C8BE,color:#333;
    classDef p3 fill:#FFF1E6,stroke:#E6C1A1,color:#333;
    classDef p4 fill:#FDE2E4,stroke:#D8A6AE,color:#333;
    classDef p5 fill:#E8E8F8,stroke:#B8B8DD,color:#333;
    classDef p6 fill:#F1F7ED,stroke:#B8D7A3,color:#333;
    classDef p7 fill:#F6EACB,stroke:#DCC68A,color:#333;
{{< /mermaid >}}

### Why LSTM helps with vanishing gradients

The core reason is the **cell state path**. The additive update in

{{% colour "green" %}}
{{< katex display=true >}}
c_t = f_t \odot c_{t-1} + i_t \odot \tilde{c}_t
{{< /katex >}}
{{% /colour %}}

provides a more stable route for gradient flow than repeatedly compressing everything through one nonlinear hidden state.

That is why LSTMs are better than vanilla RNNs at very long-range dependencies.

### Parameter counting for LSTM

Because there are four affine gate-related computations, an LSTM has roughly:

{{% colour "green" %}}
{{< katex display=true >}}
4(hd + h^2 + h)
{{< /katex >}}
{{% /colour %}}

parameters before the output layer, so it is typically heavier than a GRU.

## GRU vs LSTM

The slides give a direct practical rule of thumb.

### Prefer GRU when:

- the dataset is smaller,
- the sequence is shorter,
- faster training is important,
- you want fewer parameters.

### Prefer LSTM when:

- sequences are longer,
- you need stronger long-range memory handling,
- more data is available,
- maximum performance is more important than simplicity.

## Summary table

| Architecture | Main idea | Strength | Limitation |
|---|---|---|---|
| Vanilla RNN | Hidden state summarises past | Simple sequence model | Vanishing gradients |
| Bidirectional RNN | Use past and future context | Better full-context understanding | Not causal, not online |
| Stacked RNN | Multiple recurrent layers | Hierarchical sequence features | Harder optimisation |
| GRU | Two-gate controlled memory | Efficient and effective | Less explicit memory separation |
| LSTM | Cell state + three gates | Best long-term memory control | More parameters |

## Practical guidance

### For causal forecasting

Use:
- vanilla RNN only for simple baselines,
- GRU or LSTM for stronger sequence memory,
- avoid bidirectional RNN when future values are unavailable.

### For full-sequence text understanding

Use:
- bidirectional RNN if future context helps,
- stacked GRU/LSTM if deeper representations are useful.

### For deeper recurrent stacks

Remember the transcript guidance:
- start with a shallow model,
- a depth of **2–4 recurrent layers** is often enough,
- deeper stacks need care because gradients must flow through both time and depth.

## Exam-ready takeaways

{{% hint info %}}
- **Bidirectional RNN** = one forward RNN + one backward RNN. Good when full context is available.
- **Deep RNN** = stacked recurrent layers. Good for hierarchical temporal representations.
- **GRU** = reset gate + update gate + candidate state. Lighter than LSTM.
- **LSTM** = forget gate + input gate + output gate + cell state. Best known for handling long dependencies.
- **GRU and LSTM exist mainly to reduce the training difficulty of vanilla RNNs on long sequences.**
{{% /hint %}}

## References
- **Dive into deep learning. Cambridge University Press.**. (T1 – Ch 10)
- R4 – Ch 9

- Course handout: Deep Neural Networks, Session 10 topic structure.
- DNN Module 7 slides on bidirectional RNNs, deep stacked RNNs, GRU, LSTM, and recurrent forecasting applications.
- Lecture transcripts explaining gate intuition, why bidirectional RNNs are not suitable for real-time prediction, and why practical deep RNN depth is often limited to a few layers.

---
{{< home-link "Home" >}} | {{< section-index >}}
---
