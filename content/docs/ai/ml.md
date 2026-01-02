---
title: 'Machine Learning'
featured_image: '/images/zebrato.jpeg'
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
    classDef category font-style:italic,font-weight:bold,fill:#f3f4f6,stroke:#374151

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
## Supervised Learning
- Trained using **labelled data**.  
- Each example in the training set includes the **correct output**.  
- The algorithm learns to **generalise** and make predictions on unseen data.  
- Generally more **accurate** than unsupervised methods.  
- Requires **human intervention** for labelling and setup.  
- Widely used due to its **accuracy and efficiency**.
- Produces **highly accurate results** when trained on good-quality labelled data.  

### Classification
- Output is **discrete** (e.g. Yes/No, Spam/Not Spam).  
- Linear classifiers support **vector machines** (e.g. Support Vector Machine).  
- Used for **categorising data** into predefined classes.  

#### **Common Types of Classification**
- **Binary Classification** – Two possible outcomes (e.g. Spam / Not Spam).  
- **Multi-Class Classification** – More than two outcomes (e.g. Classifying animals as Cat, Dog, Bird).  
- **Multi-Label Classification** – Each instance can belong to multiple categories (e.g. A movie tagged as Action and Comedy).  

### Regression
- Output is a **continuous value** (e.g. price, temperature, probability).  
- Predicts **numeric outcomes** rather than categories.  

#### **Common Types of Regression**
- **Linear Regression** – Models a straight-line relationship between input and output.  
- **Multiple Linear Regression** – Uses multiple features to predict one output.  
- **Polynomial Regression** – Models curved relationships.  
- **Logistic Regression** – Despite the name, used for classification (predicting probabilities).  

---

## Unsupervised Learning
- Works on **unlabelled raw data**.
- The algorithm **discovers hidden patterns** without prior knowledge of outcomes.  
- Requires **no human intervention** during training.  
- Does not make direct predictions — it **groups or organises data** instead.
- Carries a **higher risk** because there’s no ground truth to verify results.  
- Common techniques include **Clustering**, **Association**, and **Dimensionality Reduction**.  

### Clustering
- Groups **similar data points** together based on shared features.  
- Commonly used for **market segmentation**, **image compression**, and **anomaly detection**.  

#### **Common Types of Clustering**
- **K-Means Clustering** – Divides data into *K* groups based on similarity.  
- **Hierarchical Clustering** – Builds a hierarchy (tree) of clusters.  
- **DBSCAN (Density-Based Spatial Clustering)** – Groups points close in density; identifies noise/outliers.  

### Association
- Identifies **relationships or correlations** between variables in a dataset.  
- Commonly used in **market basket analysis** (e.g. "Customers who bought X also bought Y").  

#### **Common Techniques**
- **Apriori Algorithm** – Finds frequent itemsets and generates association rules.  
- **Eclat Algorithm** – Similar to Apriori but uses set intersections for faster computation.  

### Dimensionality Reduction
- Reduces the **number of input variables** to simplify data.  
- Helps remove noise and redundancy.  
- Commonly used in **data pre-processing** and **visualisation**.  

#### **Common Techniques**
- **Principal Component Analysis (PCA)** – Projects data onto fewer dimensions while keeping most variance.  
- **Linear Discriminant Analysis (LDA)** – Focuses on class separation.  
- **t-SNE (t-Distributed Stochastic Neighbour Embedding)** – Used for visualising high-dimensional data.  
- **Autoencoders** – Neural networks that compress and reconstruct data.  

---

## Semi-Supervised Learning
- A combination of **labelled** and **unlabelled data**.  
- Useful when labelling large datasets is **expensive or time-consuming**.  
- Works well with **high-volume datasets** (e.g. millions of images).  
- Only a **small fraction of data** is labelled (e.g. a few thousand).  
- The algorithm learns from both labelled examples and structure in unlabelled data.  
- **Ideal for medical imaging** where labelled data is limited.  
- For example, a **radiologist** can label a small set of medical scans,  
  and the model uses that to learn from thousands of unlabelled scans.  
- Helps improve **accuracy and generalisation** with minimal manual effort.

---

## Reinforcement
- Uses rewards and punishment model to train. 
- Agent learns a policy/strategy that maximises its rewards over time.

---

{{< home-link "Home" >}} | {{< section-index >}}

