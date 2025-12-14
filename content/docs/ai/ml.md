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

---

{{< mermaid >}}
stateDiagram-v2
    classDef ahsState font-style:italic,font-weight:bold,fill:lightblue

    ML: Machine Learning

    ML --> SL:::ahsState
    SL: Supervised Learning

    SL --> Classification
    Classification --> NB
    NB: Naive Bayes
    NB --> KNN
    KNN: k-Nearest Neighbours
    KNN --> SVM
    SVM: Support Vector Machine

    SL --> Regression
    Regression --> LR
    LR: Linear Regression
    LR --> NN
    NN: Neural Network
    NN --> DT
    DT: Decision Tree

    ML --> USL:::ahsState
    USL: Unsupervised Learning

    USL --> Clustering
    Clustering --> KM
    KM: K-Means
    KM --> GMM
    GMM: Gaussian Mixture Model
    GMM --> HMM
    HMM: Hidden Markov Model

    ML --> RL:::ahsState
    RL: Reinforcement Learning
    RL --> DM
    DM: Decision Making
{{< /mermaid >}}


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

