---
title: 'Neural Networks'
draft: false
tags: ["AI", "ML", "Neural Networks"]
categories: ["AI", "ML"]
weight: 25
menu: main
---

## Neural Networks

- A **network of artificial neurons** inspired by how neurons function in the **human brain**.  
- At its core - a **mathematical model** designed to process and learn from data.  
- Neural networks form the **foundation of [Deep Learning](/docs/ai/deep-learning/)** (involves training large and complex networks on vast amounts of data).  

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

