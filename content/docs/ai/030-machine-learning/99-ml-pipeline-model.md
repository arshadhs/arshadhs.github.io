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
{{< katex display=true >}}
IQR = Q_3 - Q_1
{{< /katex >}}
{{% /colour %}}

{{% colour "green" %}}
{{< katex display=true >}}
\text{Lower} = Q_1 - 1.5 \times IQR
{{< /katex >}}
{{% /colour %}}

{{% colour "green" %}}
{{< katex display=true >}}
\text{Upper} = Q_3 + 1.5 \times IQR
{{< /katex >}}
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
{{< katex display=true >}}
z = \frac{x - \mu}{\sigma}
{{< /katex >}}
{{% /colour %}}

**MinMaxScaler**

{{% colour "green" %}}
{{< katex display=true >}}
x' = \frac{x - \min(x)}{\max(x) - \min(x)}
{{< /katex >}}
{{% /colour %}}

**RobustScaler**

{{% colour "green" %}}
{{< katex display=true >}}
x' = \frac{x - \text{median}(x)}{IQR}
{{< /katex >}}
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

k-NN is a **non-parametric, instance-based learning algorithm**.  
It does not learn explicit parameters but instead stores the training data.

### Key Idea
Prediction is made based on the **majority class of the nearest neighbours**.

### Distance Metric (Euclidean)

{{% colour "blue" %}}
{{< katex display=true >}}
d = \sqrt{\sum_{i=1}^{n}(x_i - y_i)^2}
{{< /katex >}}
{{% /colour %}}

- Measures similarity between data points  
- Smaller distance → more similar  

### Prediction Rule

{{% colour "blue" %}}
{{< katex display=true >}}
\hat{y} = \arg\max_{c} \sum_{i \in N_k(x)} \mathbf{1}(y_i = c)
{{< /katex >}}
{{% /colour %}}

Where:
- \( N_k(x) \): k nearest neighbours  
- \( \mathbf{1} \): indicator function  

### Key Characteristics
- No training phase  
- Sensitive to scaling → requires normalisation  
- Computationally expensive at prediction time  

---

# 8.2 Support Vector Machine (SVM)

SVM finds the **optimal hyperplane** that maximises the margin between classes.

### Decision Boundary

{{% colour "blue" %}}
{{< katex display=true >}}
w \cdot x + b = 0
{{< /katex >}}
{{% /colour %}}

### Margin Constraint

{{% colour "blue" %}}
{{< katex display=true >}}
y_i(w \cdot x_i + b) \geq 1
{{< /katex >}}
{{% /colour %}}

### Optimisation Objective

{{% colour "blue" %}}
{{< katex display=true >}}
\min_{w,b} \frac{1}{2} \|w\|^2
{{< /katex >}}
{{% /colour %}}

### With Hinge Loss

{{% colour "blue" %}}
{{< katex display=true >}}
L = \max(0, 1 - y_i(w \cdot x_i + b))
{{< /katex >}}
{{% /colour %}}

### Key Characteristics
- Effective in high-dimensional spaces  
- Sensitive to feature scaling  
- Can use kernels for non-linear boundaries  

---

# 8.3. Decision Tree

A Decision Tree splits data recursively based on feature thresholds.

### Gini Impurity

{{% colour "blue" %}}
{{< katex display=true >}}
Gini = 1 - \sum_{i=1}^{C} p_i^2
{{< /katex >}}
{{% /colour %}}

### Split Quality

{{% colour "blue" %}}
{{< katex display=true >}}
G_{split} = \frac{N_L}{N}Gini_L + \frac{N_R}{N}Gini_R
{{< /katex >}}
{{% /colour %}}

### Information Gain

{{% colour "blue" %}}
{{< katex display=true >}}
Gain = Gini_{parent} - G_{split}
{{< /katex >}}
{{% /colour %}}

### Key Characteristics
- Highly interpretable  
- Captures non-linear relationships  
- Prone to overfitting  

---

# 8.4. Naïve Bayes (Gaussian)

Naïve Bayes is a **probabilistic classifier** based on **Bayes’ Theorem**.

### Bayes Theorem

{{% colour "blue" %}}
{{< katex display=true >}}
P(A \mid B) = \frac{P(B \mid A) P(A)}{P(B)}
{{< /katex >}}
{{% /colour %}}

### Naïve Assumption
Features are **conditionally independent given the class**.

### Gaussian Likelihood

{{% colour "blue" %}}
{{< katex display=true >}}
P(x_i \mid y=c) =
\frac{1}{\sqrt{2\pi\sigma_c^2}}
\exp\left(-\frac{(x_i - \mu_c)^2}{2\sigma_c^2}\right)
{{< /katex >}}
{{% /colour %}}

### Final Prediction

{{% colour "blue" %}}
{{< katex display=true >}}
\hat{y} = \arg\max_c \left( \log P(c) + \sum_{i=1}^{n} \log P(x_i \mid c) \right)
{{< /katex >}}
{{% /colour %}}

### Key Characteristics
- Fast and efficient  
- Works well with high-dimensional data  
- Assumption rarely holds → still performs well  

---

# 8.5. Random Forest (Ensemble Model)

Random Forest is an **ensemble of decision trees** using:
- Bootstrap sampling  
- Random feature selection  
- Majority voting  

### Bootstrap Sampling
Each tree is trained on a random sample of data with replacement.

### Random Feature Selection
At each split:

{{% colour "blue" %}}
{{< katex display=true >}}
m = \sqrt{d}
{{< /katex >}}
{{% /colour %}}

Where:
- \( d \): number of features  
- \( m \): features considered per split  

### Final Prediction (Voting)

{{% colour "blue" %}}
{{< katex display=true >}}
\hat{y} = \operatorname{mode}(y_1, y_2, ..., y_T)
{{< /katex >}}
{{% /colour %}}

### Key Characteristics
- Reduces overfitting  
- Handles non-linear interactions  
- Robust to noise  

---

# 9. Model Comparison Summary

| Model | Strengths | Weaknesses |
|------|----------|-----------|
| k-NN | Simple, intuitive | Slow at prediction, sensitive to scaling |
| Naïve Bayes | Fast, probabilistic | Strong independence assumption |
| Decision Tree | Interpretable, flexible | Overfitting |
| SVM | Powerful, margin-based | Sensitive to tuning |
| Random Forest | Robust, accurate | Less interpretable |

---

## Key Insight

{{% hint info %}}
Different models capture different structures:

- k-NN → local similarity  
- Naïve Bayes → probabilistic independence  
- Decision Tree → rule-based splits  
- SVM → margin optimisation  
- Random Forest → ensemble robustness  

Combining them gives a comprehensive understanding of the dataset.
{{% /hint %}}

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
