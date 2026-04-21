---
title: "ML Pipeline"
date: 2026-04-21
draft: false
weight: 9999
tags: ["machine-learning", "preprocessing", "models", "pipeline"]
categories: ["ai", "machine-learning"]
---

# Machine Learning Pipeline: Preprocessing & Models

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

**Why they occur**
- Sensor errors
- Data collection issues

**Methods**

- Numerical → Median (robust to outliers)
- Categorical → Mode

---

# 3. Outlier Handling

**IQR Method**

{{% colour "green" %}}
\[
IQR = Q_3 - Q_1
\]
{{% /colour %}}

{{% colour "green" %}}
\[
\text{Lower} = Q_1 - 1.5 \times IQR
\]
{{% /colour %}}

{{% colour "green" %}}
\[
\text{Upper} = Q_3 + 1.5 \times IQR
\]
{{% /colour %}}

These formulas are used for **IQR-based outlier detection**:

- \( Q_1 \): First quartile (25th percentile)  
- \( Q_3 \): Third quartile (75th percentile)  
- \( IQR \): Spread of the middle 50% of the data  

Any value:
- below **Lower bound** → potential outlier  
- above **Upper bound** → potential outlier  

This method is robust because it is not affected by extreme values.

✔ Keeps all data  
✔ Simple  

---

**Isolation Forest**

- Detects anomalies using tree structures
- Works on multiple features

✔ Detects complex outliers  
✖ Removes rows  

---

# 4. Encoding

**One-Hot Encoding**
- Converts categories to binary columns
- No ordering assumption

**Ordinal Encoding**
- Converts categories to integers
- Can introduce false relationships

---

# 5. Feature Scaling

- **StandardScaler**: Centres data around mean (0) with unit variance. Sensitive to outliers.  
- **MinMaxScaler**: Scales data to a fixed range (usually 0–1). Preserves shape but sensitive to extreme values.  
- **RobustScaler**: Uses median and IQR, making it resistant to outliers — ideal for skewed distributions.

**StandardScaler**
{{% colour "green" %}}
\[
z = \frac{x - \mu}{\sigma}
\]
{{% /colour %}}

**MinMaxScaler**
{{% colour "green" %}}
\[
x' = \frac{x - \min(x)}{\max(x) - \min(x)}
\]
{{% /colour %}}

**RobustScaler**
{{% colour "green" %}}
\[
x' = \frac{x - \text{median}(x)}{IQR}
\]
{{% /colour %}}

✔ Best for noisy data  

---

# 6. Feature Engineering

**Examples**

- Discomfort Index = Temperature × Humidity  
- Pressure × Visibility  
- Seasonal encoding using sin/cos  

---

# 7. Feature Selection

**Mutual Information**
- Detects non-linear relationships

**Random Forest Importance**
- Model-based ranking

---

# 8. Model Development

---

# 8.1 k-Nearest Neighbours (k-NN)

**Idea**
- Predict based on closest data points

**Key concept**
Distance:

{{% colour "green" %}}
\[
d = \sqrt{\sum_{i=1}^{n} (x_i - y_i)^2}
\]
{{% /colour %}}

This represents the **Euclidean distance**, used in k-NN to measure similarity between two data points:

- \( x_i \), \( y_i \): feature values of two samples  
- \( n \): number of features  

The smaller the distance, the more similar the two points are.

**Pros**
- Simple
- No training phase

**Cons**
- Sensitive to scaling
- Slow on large datasets

---

# 8.2 Support Vector Machine (SVM)

**Idea**
- Find best boundary (hyperplane)

**Linear SVM**

{{% colour "green" %}}
\[
w \cdot x + b = 0
\]
{{% /colour %}}

This equation represents the **decision boundary (hyperplane)** in a linear Support Vector Machine (SVM), where:

- \( w \) = weight vector  
- \( x \) = input feature vector  
- \( b \) = bias term  

The model classifies points based on which side of the hyperplane they lie.

**Pros**
- Works well in high dimensions

**Cons**
- Sensitive to parameters
- Requires scaling

---

# 8.3 Decision Tree

**Idea**
- Split data based on conditions

**Example rule**

Temperature > 25 → Hot

**Pros**
- Easy to interpret

**Cons**
- Overfitting

---

# 8.4 Naive Bayes

**Idea**
Based on probability:

{{% colour "green" %}}
\[
P(A \mid B) = \frac{P(B \mid A)\,P(A)}{P(B)}
\]
{{% /colour %}}

This is **Bayes’ Theorem**, used in Naïve Bayes classification:

- \( P(A \mid B) \): Posterior probability (what we want to compute)  
- \( P(B \mid A) \): Likelihood  
- \( P(A) \): Prior probability  
- \( P(B) \): Evidence  

In machine learning, it helps compute the probability of a class given observed features.

**Assumption**
Features are independent

**Pros**
- Fast
- Works well for simple problems

**Cons**
- Independence assumption unrealistic

---

# 8.5 Random Forest

**Idea**
- Ensemble of decision trees

**Pros**
- High accuracy
- Handles non-linearity

**Cons**
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
