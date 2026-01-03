---
title: 'Unsupervised Learning'
featured_image: '/images/zebrato.jpeg'
date: 2026-01-03T10:29:52+01:00
draft: false
tags: ["AI", "ML"]
categories: ["AI", "ML"]
weight: 402
---
# Unsupervised Learning
- Works on **unlabelled raw data**.
- The algorithm **discovers hidden patterns** without prior knowledge of outcomes.  
- Requires **no human intervention** during training.  
- Does not make direct predictions — it **groups or organises data** instead.
- Carries a **higher risk** because there’s no ground truth to verify results.  
- Common techniques include **Clustering**, **Association**, and **Dimensionality Reduction**.  

---

## Clustering
- Groups **similar data points** together based on shared features.  
- Commonly used for **market segmentation**, **image compression**, and **anomaly detection**.  

### Common Types of Clustering
- **K-Means Clustering** – Divides data into *K* groups based on similarity.  
- **Hierarchical Clustering** – Builds a hierarchy (tree) of clusters.  
- **DBSCAN (Density-Based Spatial Clustering)** – Groups points close in density; identifies noise/outliers.  

---

## Association
- Identifies **relationships or correlations** between variables in a dataset.  
- Commonly used in **market basket analysis** (e.g. "Customers who bought X also bought Y").  

### Common Techniques
- **Apriori Algorithm** – Finds frequent itemsets and generates association rules.  
- **Eclat Algorithm** – Similar to Apriori but uses set intersections for faster computation.  
---

## Dimensionality Reduction
- Reduces the **number of input variables** to simplify data.  
- Helps remove noise and redundancy.  
- Commonly used in **data pre-processing** and **visualisation**.  

### Common Techniques
- **Principal Component Analysis (PCA)** – Projects data onto fewer dimensions while keeping most variance.  
- **Linear Discriminant Analysis (LDA)** – Focuses on class separation.  
- **t-SNE (t-Distributed Stochastic Neighbour Embedding)** – Used for visualising high-dimensional data.  
- **Autoencoders** – Neural networks that compress and reconstruct data.  

---

{{< home-link "Home" >}} | {{< section-index >}}