---
title: 'Machine Learning'
date: 2024-08-06T23:29:52+01:00
draft: false
tags: ["AI", "ML"]
categories: ["AI", "ML"]
weight: 400
---
# Machine Learning

{{< mermaid >}}
stateDiagram-v2

    %% ===== CLASS DEFINITIONS (Math-based colours) =====
    classDef algebra fill:#cfe8ff,stroke:#1e3a8a,stroke-width:1px
    classDef probability fill:#d1fae5,stroke:#065f46,stroke-width:1px
    classDef geometry fill:#ffedd5,stroke:#9a3412,stroke-width:1px
    classDef logic fill:#ede9fe,stroke:#5b21b6,stroke-width:1px
    classDef category font-style:italic,font-weight:bold,fill:#aaaaaa,stroke:#374151,stroke-width:3px

    %% ===== ROOT =====
    ML: Machine Learning

    %% ===== SUPERVISED =====
    ML --> SL:::category
    SL: Supervised Learning

    SL --> Classification
    Classification --> NB:::probability
    NB: Naive Bayes

    NB --> KNN:::geometry
    KNN: k-Nearest Neighbours

    KNN --> SVM:::algebra
    SVM: Support Vector Machine

    SL --> Regression
    Regression --> LR:::algebra
    LR: Linear Regression

    LR --> NN:::algebra
    NN: Neural Network

    NN --> DT:::logic
    DT: Decision Tree

    %% ===== UNSUPERVISED =====
    ML --> USL:::category
    USL: Unsupervised Learning

    USL --> Clustering
    Clustering --> KM:::geometry
    KM: K-Means

    KM --> GMM:::probability
    GMM: Gaussian Mixture Model

    GMM --> HMM:::probability
    HMM: Hidden Markov Model

    %% ===== REINFORCEMENT =====
    ML --> RL:::category
    RL: Reinforcement Learning

    RL --> DM:::logic
    DM: Decision Making
{{< /mermaid >}}

---

{{% details title="Mathematical Legend" open=false %}}

### Algebra / Linear Algebra (Blue)

Used heavily when models rely on:

- Equations  
- Vectors  
- Hyperplanes  
- Weights  

**Examples:**

- Linear Regression  
- Neural Networks  
- Support Vector Machines (SVM)  

---

### Probability & Statistics (Green)

Used when models are based on:

- Likelihood  
- Distributions  
- Randomness  
- Bayesian thinking  

**Examples:**

- Naive Bayes  
- Gaussian Mixture Models (GMM)  
- Hidden Markov Models (HMM)  

---

### Geometry / Distance (Orange)

Used when models depend on:

- Distance  
- Similarity  
- Clustering in space  

**Examples:**

- k-Nearest Neighbours (k-NN)  
- K-Means  

---

### Logic / Decision / Optimisation (Purple)

Used when models are based on:

- Rules  
- Decisions  
- Reward and punishment  
- Tree structures  

**Examples:**

- Decision Trees  
- Reinforcement Learning  
{{% /details %}}

---

{{< home-link "Home" >}} | {{< section-index >}}

