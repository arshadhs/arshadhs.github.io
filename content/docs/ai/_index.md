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
- Statistical Methods
- Machine Learning
- Deep Neural Networks

---

{{< section_tree >}}

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
AI["Artificial<br/>Intelligence"] --> ML["Machine<br/>Learning"]
ML --> NN["Neural<br/>Networks"]
NN --> DL["Deep<br/>Learning"]
DL --> FM["Foundation<br/>Models"]
FM --> LLM["LLM<br/>Models"]

%% ML examples
ML --> LR["Linear<br/>Regression"]
ML --> DT["Decision<br/>Trees"]

%% NN examples
NN --> MLP["MLP"]
NN --> CNN["CNN"]

%% DL examples
DL --> CNN_DL["CNN<br/>(deep)"]
DL --> RNN["RNN"]

%% Foundation Model examples
FM --> BERT["BERT"]
FM --> CLIP["CLIP"]

%% LLM examples
LLM --> GPT["GPT"]
LLM --> LLAMA["LLaMA"]

%% Modalities
LLM --> TEXT["Text"]
LLM --> IMAGE["Images"]
LLM --> AUDIO["Audio"]
LLM --> VIDEO["Video"]

%% Styling (your pastel palette)
style AI fill:#90CAF9,stroke:#1E88E5,color:#000
style ML fill:#90CAF9,stroke:#1E88E5,color:#000
style NN fill:#90CAF9,stroke:#1E88E5,color:#000

style DL fill:#CE93D8,stroke:#8E24AA,color:#000
style FM fill:#CE93D8,stroke:#8E24AA,color:#000

style LLM fill:#C8E6C9,stroke:#2E7D32,color:#000
style LR fill:#C8E6C9,stroke:#2E7D32,color:#000
style DT fill:#C8E6C9,stroke:#2E7D32,color:#000
style MLP fill:#C8E6C9,stroke:#2E7D32,color:#000
style CNN fill:#C8E6C9,stroke:#2E7D32,color:#000
style CNN_DL fill:#C8E6C9,stroke:#2E7D32,color:#000
style RNN fill:#C8E6C9,stroke:#2E7D32,color:#000
style BERT fill:#C8E6C9,stroke:#2E7D32,color:#000
style CLIP fill:#C8E6C9,stroke:#2E7D32,color:#000
style GPT fill:#C8E6C9,stroke:#2E7D32,color:#000
style LLAMA fill:#C8E6C9,stroke:#2E7D32,color:#000
style TEXT fill:#C8E6C9,stroke:#2E7D32,color:#000
style IMAGE fill:#C8E6C9,stroke:#2E7D32,color:#000
style AUDIO fill:#C8E6C9,stroke:#2E7D32,color:#000
style VIDEO fill:#C8E6C9,stroke:#2E7D32,color:#000

{{< /mermaid >}}

---

![AI, ML, DL, and Data Science Diagram](/images/ai/ai_ml_dl_ds_diagram.png)

---
{{< home-link "Home" >}}





