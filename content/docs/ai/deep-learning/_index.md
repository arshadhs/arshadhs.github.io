---
title: 'Deep Learning'
draft: false
tags: ['deep learning', 'neural networks', 'ai']
categories: ['artificial intelligence']
weight: 415
bookCollapseSection: true
---
# Deep Learning

- Subset of ML
- focuses on algorithms inspired by the structure and function of the brain called **Artificial Neural Networks**.
- A [neural network](/docs/ai/neural-network/) with multiple hidden layers and multiple nodes in each hidden layer is known as a deep learning system or a deep neural network. 
- Allows systems to **automatically learn hierarchical representations** (features) from raw input, such as images, sound, or text.

---
{{< mermaid >}}
flowchart LR
    %% Input Layer
    subgraph subGraph0["Input Layer"]
        I1(("Input 1"))
        I2(("Input 2"))
        I3(("Input 3"))
    end

    %% Hidden Layers
    subgraph subGraph1["Hidden Layer 1"]
        H1a(("H1-1"))
        H1b(("H1-2"))
        H1c(("H1-3"))
    end

    subgraph subGraph2["Hidden Layer 2"]
        H2a(("H2-1"))
        H2b(("H2-2"))
        H2c(("H2-3"))
    end

    subgraph subGraph3["Hidden Layer 3"]
        H3a(("H3-1"))
        H3b(("H3-2"))
        H3c(("H3-3"))
    end

    %% Output Layer
    subgraph subGraph4["Output Layer"]
        O(("Output"))
    end

    %% Connections: Input to Hidden Layer 1
    I1 --> H1a & H1b & H1c
    I2 --> H1a & H1b & H1c
    I3 --> H1a & H1b & H1c

    %% Connections: Hidden Layer 1 to Hidden Layer 2
    H1a --> H2a & H2b & H2c
    H1b --> H2a & H2b & H2c
    H1c --> H2a & H2b & H2c

    %% Connections: Hidden Layer 2 to Hidden Layer 3
    H2a --> H3a & H3b & H3c
    H2b --> H3a & H3b & H3c
    H2c --> H3a & H3b & H3c

    %% Connections: Hidden Layer 3 to Output
    H3a --> O
    H3b --> O
    H3c --> O

    %% Styling
    style I1 fill:#C8E6C9
    style I2 fill:#C8E6C9
    style I3 fill:#C8E6C9
    style H1a fill:#BBDEFB
    style H1b fill:#BBDEFB
    style H1c fill:#BBDEFB
    style H2a fill:#90CAF9
    style H2b fill:#90CAF9
    style H2c fill:#90CAF9
    style H3a fill:#64B5F6
    style H3b fill:#64B5F6
    style H3c fill:#64B5F6
    style O fill:#FFCDD2
    style subGraph0 stroke:none,fill:transparent
    style subGraph1 stroke:none,fill:transparent
    style subGraph2 stroke:none,fill:transparent
    style subGraph3 stroke:none,fill:transparent
    style subGraph4 stroke:none,fill:transparent
{{</mermaid >}}

---

## Types of Neural Networks
- Standard NN - Small and Standard for a smaller and simpler data (e.g. Real Estate
- CNN - Convolution - used for Images (e.g. Photo Tagging, Object Detection)
- RNN - used for Text (e.g. Speech Recognition, Translation)
- Hybrid NN (e.g. Autonoumous Driving)

--

## Components of DL

- Data
- Learning Algorithm : How to transform data
- Loss Function: Objective function that quantifies how well is model doing?  lower the loss function, the better the model. So loss function will try to quantify how well or badly the model is learning or the model is doing.
- Optimnisation Algorithm: in order to adjust the loss function, Learning Algorithm will try to **optimize our algorithm**. searching for the best possible parameters for minimizing the loss function. Popular optimization algorithms for deep learning are based on an approach called **gradient descent**.
- Model




## Applications

- Computer Vision (e.g., face detection, medical imaging)
- Natural Language Processing (e.g., ChatGPT, translation)
- Self Driving Cars
- Speech Assistants (e.g., Alexa, Siri)

---
{{< home-link "Home" >}} | {{< section-index >}}  

