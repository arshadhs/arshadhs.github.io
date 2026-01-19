---
title: "Principal Component Analysis (PCA)"
draft: false
tags: ["Machine Learning", "Mathematics", "Linear Algebra", "Probability", "Statistics", "Calculus"]
categories: ["AI", "ML"]
weight: 1170
menu: main
---
# Principal Component Analysis (PCA)

- dimensionality reduction technique 
- helps us to **reduce the number of features** in a dataset while keeping the most important information.
- changes complex datasets by transforming correlated features into a smaller set of uncorrelated components.
- uses **linear algebra** to transform data into **new features** called principal components.
- finds these by calculating **eigenvectors (directions)** and **eigenvalues (importance)** from the **covariance matrix**.
- PCA **selects the top components with the highest eigenvalues** and **projects the data onto them simplify the dataset**.

{{% hint info %}}
Note: It prioritizes the directions where the data varies the most because more variation = more useful information.
{{% /hint %}}

---

{{% step %}}
1. Standardize the Data
2. Calculate **Covariance Matrix**
3. Find the Principal Components
4. Pick the Top Directions & Transform Data
{{% /step %}}

---

{{< home-link "Home" >}} | {{< section-index >}}
