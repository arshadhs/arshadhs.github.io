---
title: "ML Pipeline: Preprocessing & Models"
date: 2026-04-21
draft: false
weight: 9999
tags: ["machine-learning", "preprocessing", "models", "pipeline"]
categories: ["ai", "machine-learning"]
---

# Machine Learning Pipeline

This page explains both **data preprocessing** and **model development concepts** in a clear, structured way to support understanding.

{{% hint info %}}
A complete ML pipeline includes preprocessing, feature engineering, feature selection, and model training.
{{% /hint %}}

---

# 1. Data Preprocessing Overview

Raw data is often:
- Noisy
- Incomplete
- Inconsistent

Preprocessing ensures data is suitable for machine learning.

---

# 2. Missing Values

## Why they occur
- Sensor errors
- Data collection issues

## Methods

- Numerical → Median (robust to outliers)
- Categorical → Mode

---

# 3. Outlier Handling

## IQR Method

\[
IQR = Q3 - Q1
\]
\[
Lower = Q1 - 1.5 \times IQR
\]
\[
Upper = Q3 + 1.5 \times IQR
\]

✔ Keeps all data  
✔ Simple  

---

## Isolation Forest

- Detects anomalies using tree structures
- Works on multiple features

✔ Detects complex outliers  
✖ Removes rows  

---

# 4. Encoding

## One-Hot Encoding
- Converts categories to binary columns
- No ordering assumption

## Ordinal Encoding
- Converts categories to integers
- Can introduce false relationships

---

# 5. Feature Scaling

## StandardScaler
\[
z = \frac{x - \mu}{\sigma}
\]

## MinMaxScaler
\[
x' = \frac{x - min}{max - min}
\]

## RobustScaler
\[
x' = \frac{x - median}{IQR}
\]

✔ Best for noisy data  

---

# 6. Feature Engineering

## Examples

- Discomfort Index = Temperature × Humidity  
- Pressure × Visibility  
- Seasonal encoding using sin/cos  

---

# 7. Feature Selection

## Mutual Information
- Detects non-linear relationships

## Random Forest Importance
- Model-based ranking

---

# 8. Model Development

---

# 8.1 k-Nearest Neighbours (k-NN)

## Idea
- Predict based on closest data points

## Key concept
Distance:

\[
d = \sqrt{\sum (x_i - y_i)^2}
\]

## Pros
- Simple
- No training phase

## Cons
- Sensitive to scaling
- Slow on large datasets

---

# 8.2 Support Vector Machine (SVM)

## Idea
- Find best boundary (hyperplane)

## Linear SVM
\[
w \cdot x + b = 0
\]

## Pros
- Works well in high dimensions

## Cons
- Sensitive to parameters
- Requires scaling

---

# 8.3 Decision Tree

## Idea
- Split data based on conditions

## Example rule
Temperature > 25 → Hot

## Pros
- Easy to interpret

## Cons
- Overfitting

---

# 8.4 Naive Bayes

## Idea
Based on probability:

\[
P(A|B) = \frac{P(B|A)P(A)}{P(B)}
\]

## Assumption
Features are independent

## Pros
- Fast
- Works well for simple problems

## Cons
- Independence assumption unrealistic

---

# 8.5 Random Forest

## Idea
- Ensemble of decision trees

## Pros
- High accuracy
- Handles non-linearity

## Cons
- Less interpretable

---

# 9. Model Comparison

| Model | Strength | Weakness |
|------|--------|----------|
| k-NN | Simple | Slow, sensitive to scaling |
| SVM | Powerful | Needs tuning |
| Decision Tree | Interpretable | Overfits |
| Naive Bayes | Fast | Simplistic |
| Random Forest | Robust | Less interpretable |

---

# 10. Full Pipeline

Raw Data  
↓  
Preprocessing  
↓  
Feature Engineering  
↓  
Feature Selection  
↓  
Model Training  
↓  
Evaluation  

---

# Key Takeaways

{{% hint info %}}
- Preprocessing is critical for model success  
- Outliers and scaling significantly impact performance  
- Different models have different strengths  
- Ensemble models often perform best  
{{% /hint %}}

---

{{< home-link "Home" >}} | {{< section-index >}}

---
