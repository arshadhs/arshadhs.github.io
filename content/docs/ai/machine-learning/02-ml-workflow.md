---
title: 'Machine learning Workflow'
draft: false
tags: ["AI", "ML"]
categories: ["AI", "ML"]
weight: 420
---
# Machine learning Workflow

Data is the foundation of any machine learning system.
Quality of data matters more than model complexity.

### Role of Data 

Data determines:

- What patterns the model can learn
- How well it generalises
- Whether bias or noise is introduced

Bad data → bad model (even with perfect algorithms).

### Data Preprocessing, wrangling 

Raw data is never ready for training.

Common preprocessing steps:

- Handling missing values
- Removing duplicates
- Normalisation / standardisation
- Encoding categorical variables
- Feature selection

Convert raw data → usable features.

### Data skewness removal (sampling) 

Real-world data is often imbalanced.

Example:
- 95% normal transactions
- 5% fraud cases

If left untreated, the model becomes biased.

Common techniques:
- Oversampling minority class
- Undersampling majority class
- Stratified sampling

Goal:
Ensure the model learns fairly from all classes.

### Model Training 

This is where learning happens.

- Choose an algorithm
- Initialise parameters
- Optimise a loss function
- Iterate until performance improves

Training is iterative, not one-time.

### Model Testing and performance metrics

Training accuracy alone is misleading.

Models must be evaluated on unseen data.

Common metrics:
- Accuracy
- Precision
- Recall
- F1-score
- ROC-AUC

This tells us:
Does the model generalise beyond training data?

---

{{< home-link "Home" >}} | {{< section-index >}}

