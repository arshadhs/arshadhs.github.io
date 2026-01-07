---
title: 'Artificial Intelligence'
featured_image: '/images/zebrato.jpeg'
date: 2024-07-04T10:55:52+01:00
draft: false
tags: ["AI"]
categories: ["AI"]
weight: 10
menu: main
bookCollapseSection: true
---
# My AI Notes

Learning how machines learn! My working notes as I learn AI.

---

{{< mermaid >}}
flowchart LR
    AI[Artificial Intelligence]
    ML[Machine Learning]
    DL[Deep Learning]
    FM[Foundation Models]
    LLM[LLM Models]

    AI --> ML
    ML --> DL
    DL --> FM
    FM --> LLM

    style AI fill:#E1F5FE
    style ML fill:#C8E6C9
    style DL fill:#90CAF9
    style FM fill:#64B5F6
    style LLM fill:#FFCCBC
{{< /mermaid >}}

---

- Mathematical Foundations for Machine Learning
- Introduction to Statistical Methods
- Deep Neural Networks
- Machine Learning

---
{{< section_tree >}}
---

{{< section_menu >}}

---

- Machine Learning → The broad field where systems learn patterns from data to make predictions or decisions.
- Neural Networks → A subset of machine learning that uses interconnected artificial neurons to model complex relationships.
- Deep Learning → A subset of neural networks that uses many hidden layers to learn high-level features from large datasets.
- Foundation Models → Large deep learning models trained on massive datasets and reused across many tasks using transfer learning.
- LLMs (Large Language Models) → A specialised type of foundation model focused on understanding and generating human language.

---

{{< mermaid >}}
flowchart TD

    %% Core hierarchy
    AI[Artificial Intelligence]
    ML[Machine Learning]
    NN[Neural Networks]
    DL[Deep Learning]
    FM[Foundation Models]
    LLM[LLM Models]

    %% Relationships
    AI --> ML
    ML --> NN
    NN --> DL
    DL --> FM
    FM --> LLM

    %% ML examples
    ML --> LR[Linear Regression]
    ML --> DT[Decision Trees]

    %% NN examples
    NN --> MLP[MLP]
    NN --> CNN[CNN]

    %% DL examples
    DL --> CNN_DL[CNN Deep]
    DL --> RNN[RNN]

    %% Foundation Model examples
    FM --> BERT[BERT]
    FM --> CLIP[CLIP]

    %% LLM examples
    LLM --> GPT[GPT]
    LLM --> LLAMA[LLaMA]

    %% Modalities
    LLM --> TEXT[Text]
    LLM --> IMAGE[Images]
    LLM --> AUDIO[Audio]
    LLM --> VIDEO[Video]

    %% Styling
    style AI fill:#E1F5FE
    style ML fill:#C8E6C9
    style NN fill:#BBDEFB
    style DL fill:#90CAF9
    style FM fill:#64B5F6
    style LLM fill:#FFCCBC

    style LR fill:#DCEDC8
    style DT fill:#DCEDC8

    style MLP fill:#E3F2FD
    style CNN fill:#E3F2FD

    style CNN_DL fill:#BBDEFB
    style RNN fill:#BBDEFB

    style BERT fill:#B3E5FC
    style CLIP fill:#B3E5FC

    style GPT fill:#FFE0B2
    style LLAMA fill:#FFE0B2

    style TEXT fill:#FFF9C4
    style IMAGE fill:#FFF9C4
    style AUDIO fill:#FFF9C4
    style VIDEO fill:#FFF9C4
{{< /mermaid >}}

---

![AI, ML, DL, and Data Science Diagram](/images/ai/ai_ml_dl_ds_diagram.png)

---
{{< home-link "Home" >}}




