---
title: 'Machine Learning'
featured_image: '/images/zebrato.jpeg'
date: 2024-08-06T23:29:52+01:00
draft: false
tags: ["AI", "ML"]
categories: ["AI", "ML"]
weight: 20
menu: main
---

# Machine Learning

---

{{<mermaid>}}
stateDiagram-v2
	classDef ahsState font-style:italic,font-weight:bold,fill:lightblue
    State1: Machine Learning
	State1 --> SL:::ahsState
	SL: Supervised Learning

	SL --> 	Classification
	Classification --> NB
	NB: Naive Bayes
	NB --> NN
	NN: Nearest Neighbour
	NN --> SVM
	SL --> 	Regression
	Regression --> LR
	LR: Linear Regression
	LR --> NNetwork
	NNetwork: Neural Network
	NNetwork --> DT
	DT: Decision Tree
	State1 --> USL:::ahsState
	USL: Unsupervised Learning
	USL --> Clustering
	Clustering --> KM
	KM: K-Means
	KM --> GM
	GM : Gaussian Matrix
	note right of GM
		Neural Networks
	end note
	GM --> HM
	HM : Hidden Markov
	State1 --> Reinforcement:::ahsState
	Reinforcement --> DM
	DM: Decision Making

{{</mermaid>}}

## Supervised Learning
- Trained using **labelled data**.  
- Each example in the training set includes the **correct output**.  
- The algorithm learns to **generalise** and make predictions on unseen data.  
- Generally more **accurate** than unsupervised methods.  
- Requires **human intervention** for labelling and setup.  
- Widely used due to its **accuracy and efficiency**.  

---

### Classification
- Output is **discrete** (e.g. Yes/No, Spam/Not Spam).  
- Linear classifiers support **vector machines** (e.g. Support Vector Machine).  
- Used for **categorising data** into predefined classes.  

#### **Common Types of Classification**
- **Binary Classification** – Two possible outcomes (e.g. Spam / Not Spam).  
- **Multi-Class Classification** – More than two outcomes (e.g. Classifying animals as Cat, Dog, Bird).  
- **Multi-Label Classification** – Each instance can belong to multiple categories (e.g. A movie tagged as Action and Comedy).  

---

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
- Common techniques include **Clustering**, **Association**, and **Dimensionality Reduction**.  

---

### Clustering
- Groups **similar data points** together based on shared features.  
- Commonly used for **market segmentation**, **image compression**, and **anomaly detection**.  

#### **Common Types of Clustering**
- **K-Means Clustering** – Divides data into *K* groups based on similarity.  
- **Hierarchical Clustering** – Builds a hierarchy (tree) of clusters.  
- **DBSCAN (Density-Based Spatial Clustering)** – Groups points close in density; identifies noise/outliers.  

---

### Association
- Identifies **relationships or correlations** between variables in a dataset.  
- Commonly used in **market basket analysis** (e.g. "Customers who bought X also bought Y").  

#### **Common Techniques**
- **Apriori Algorithm** – Finds frequent itemsets and generates association rules.  
- **Eclat Algorithm** – Similar to Apriori but uses set intersections for faster computation.  

---

### Dimensionality Reduction
- Reduces the **number of input variables** to simplify data.  
- Helps remove noise and redundancy.  
- Commonly used in **data pre-processing** and **visualisation**.  

#### **Common Techniques**
- **Principal Component Analysis (PCA)** – Projects data onto fewer dimensions while keeping most variance.  
- **Linear Discriminant Analysis (LDA)** – Focuses on class separation.  
- **t-SNE (t-Distributed Stochastic Neighbour Embedding)** – Used for visualising high-dimensional data.  
- **Autoencoders** – Neural networks that compress and reconstruct data.  

## Reinforcement
- Uses rewards and punishment model to train. 
- Agent learns a policy/strategy that maximises its rewards over time.

---

{{< home-link "Home" >}} | {{< section-index >}}
