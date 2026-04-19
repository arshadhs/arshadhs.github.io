---
title: 'Neural Networks'
draft: false
tags: ["AI", "ML", "Neural Networks"]
categories: ["AI", "ML"]
weight: 150
menu: main
---
# Neural Networks

- A **network of artificial neurons** inspired by how neurons function in the **human brain**.  
- At its core - a **mathematical model** designed to process and learn from data.  
- Neural networks form the **foundation of [Deep Learning](/docs/ai/deep-learning/)** (involves training large and complex networks on vast amounts of data).  

---
{{< mermaid >}}
flowchart LR
 subgraph subGraph0["Input Layer"]
        I1(("Input 1"))
        I2(("Input 2"))
        I3(("Input 3"))
  end
 subgraph subGraph1["Hidden Layer"]
        H1(("Hidden 1"))
        H2(("Hidden 2"))
        H3(("Hidden 3"))
  end
 subgraph subGraph2["Output Layer"]
        O(("Output"))
  end
    I1 --> H1 & H2 & H3
    I2 --> H1 & H2 & H3
    I3 --> H1 & H2 & H3
    H1 --> O
    H2 --> O
    H3 --> O

    style I1 fill:#C8E6C9
    style I2 fill:#C8E6C9
    style I3 fill:#C8E6C9
    style H1 stroke:#2962FF,fill:#BBDEFB
    style H2 fill:#BBDEFB
    style H3 fill:#BBDEFB
    style O fill:#FFCDD2
    style subGraph0 stroke:none,fill:transparent
    style subGraph1 stroke:none,fill:transparent
    style subGraph2 stroke:none,fill:transparent
{{< /mermaid >}}

---

### Structure of a Neural Network

A typical neural network has **three main layers**:  

1. **Input Layer**  
   - Receives the data (features) and passes it to the hidden layer.  
   - Each neuron in this layer represents one input variable.  

2. **Hidden Layer(s)**  
   - The **core computational part** where the actual learning happens.  
   - Each input is **multiplied by a weight**, a **bias** is added, and the result is passed through an **activation function**.  
   - The activation function decides whether a neuron should be activated (i.e. contributes to the final output).  
   - There can be one or multiple hidden layers depending on network complexity.  

3. **Output Layer**  
   - Produces the final prediction or classification result.  
   - The output can be a **single value** (for regression) or **multiple probabilities** (for classification).  

---

### Training

- During training, the network **adjusts weights and biases** 
- Uses algorithms like **backpropagation** to minimise prediction error.
- Goal to make the predicted output as close as possible to the **actual (labelled)** output.  

---

{{< home-link "Home" >}} | {{< section-index >}}

