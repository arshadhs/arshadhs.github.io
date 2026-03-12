---
title: 'Machine learning Workflow'
draft: false
tags: ["AI", "ML"]
categories: ["AI", "ML"]
weight: 200
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

---

### Data Preprocessing, wrangling 

Raw data is never ready for training.

**Data Issues**

- Noise
  - For **objects**, noise is an **extraneous object**
  - For **attributes**, noise refers to **modification of original values**
  - Use Log or Z Transfer to convert to mean   
- Outliers
  - Data objects with characteristics that are considerably different than most of the other data objects in the data set
  - Handle: Use **IQR** method
  - Find Lower and Upper Bound and **replace Outlier with Lower or Upper Bound**
- Missing Values
  - Eliminate data objects or variables
  - Handle: Estimate missing values
    - **Mean, Median or Mode**
    - Prefer **Median** if there are missing **outliers**
  - Ignore the missing value during analysis
- Duplicate Data
  - Major issue when merging data from heterogeneous sources
- Inconsistent Codes
  - Find all Unique and transfer all inconsistent to 

**Data Preprocessing techniques**

- Aggregation
  - Combining two or more attributes (or objects) into a single attribute (or object)
  - Combine to create summary, e.g. Sales Data to monthly or quarterly data   
- Cleaning
  - handling outliers, removing duplicates, data inconsistency
- Partitioning
  - split data into subsets for training, validating, testing
- Scaling
  - transform numerical features to range
  - Standadard range (0-1)
  - **Min-Max Scaling** (Normalization)
    - Transforms data into a fixed range, typically [0, 1]
    - Use when the upper/lower bounds are known, such as in image processing or neural networks
  - **Z-Score Normalization** (Standardization)
    - Transforms data to have a mean of 0 and a standard deviation of 1
    - The resulting dataset has a mean () of 0 and a standard deviation () of 1
    - Use when your data follows a normal distribution or contains significant **outliers**, as it is **more robust to extreme values**

additional notes to augment above:
- Handling missing values
- Removing duplicates
- Normalisation / standardisation
- Encoding categorical variables
- Feature selection

---

### Distance / similarity (why scaling matters)

Many algorithms compare examples by “closeness”.
The definition of closeness depends on a distance or similarity measure.

#### Cosine similarity vs Euclidean distance

Cosine similarity compares direction (angle), not magnitude:

{{< colour "blue" >}}
{{< katex display=true >}}
\cos(\theta)=\frac{x\cdot y}{\|x\|\|y\|}
{{< /katex >}}
{{< /colour >}}

When to use cosine:
- text/document vectors (high-dimensional, sparse)
- when vector length should not dominate similarity

Euclidean distance measures straight-line distance:

{{< colour "blue" >}}
{{< katex display=true >}}
d(x,y)=\|x-y\|
{{< /katex >}}
{{< /colour >}}

When to use Euclidean:
- continuous features on comparable scales (often after normalisation)

Key takeaway:
Euclidean distance changes when features are on different scales, so scaling is often required.

---

**Categorical Preprocessing**

- **One Hot Encoding (OHE)**
  - Method for converting categorical variables into a binary format
  - Creates new columns for each category where 1 means the category is present and 0 means it is not
  - can lead to increased dimensionality - Curse of Dimensionality
 - **Label Encoding**
   - Each unique category is assigned an integer between 0 and the number of classes
   - model falsely things one is greater than other (e.g. Blue > Red)
   - commonly used
  - **Embeddings**
    - Convert to numberical vectores [0.2 0.35 0.4]
    - Convert categorical variables (e.g., **zip codes**, user IDs, product types) into low-dimensional, continuous **vector representations**
    - Using Natural Language Processing
    - An embedding matrix is a lookup table for a vector. Each row of an embedding matrix is a vector for a unique category.

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
- **Bootstrapping (sampling with replacement)**
  - Create a new dataset by sampling from the training data **with replacement**
  - Some rows repeat, some are left out
  - Used heavily in **bagging** (and Random Forests) to reduce variance
  - Each bootstrap sample is used to train a separate model, then predictions are averaged/voted

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
Models must be evaluated on unseen data (validation/test).

This tells us:
Does the model generalise beyond training data?

For **regression**:
common metrics include MSE, MAE, RMSE.

For **classification**:
we usually start with a **confusion matrix**, then derive metrics like:
- Accuracy
- Precision
- Recall
- F1-score
- ROC-AUC
---

#### Confusion matrix (binary classification)

A confusion matrix counts how predictions compare to the true labels.
You must decide which label is the **positive class** (the class you care about most).

|  | Predicted Positive | Predicted Negative |
|---|---:|---:|
| Actual Positive | TP | FN |
| Actual Negative | FP | TN |

- TP (true positive): actual positive, predicted positive
- TN (true negative): actual negative, predicted negative
- FP (false positive): actual negative, predicted positive
- FN (false negative): actual positive, predicted negative

---

#### Accuracy

Accuracy measures overall correctness.

{{< colour "blue" >}}
{{< katex display=true >}}
\mathrm{Accuracy}=\frac{TP+TN}{TP+TN+FP+FN}
{{< /katex >}}
{{< /colour >}}

Error rate:

{{< colour "blue" >}}
{{< katex display=true >}}
\mathrm{Error}=1-\mathrm{Accuracy}
{{< /katex >}}
{{< /colour >}}

When accuracy works well:
- class distribution is roughly balanced

When accuracy is misleading:
- highly skewed/imbalanced data (a trivial model can predict the majority class and still get high accuracy)

---

#### Precision

Precision measures **positive prediction correctness**:
out of everything predicted positive, how many are truly positive?

{{< colour "blue" >}}
{{< katex display=true >}}
\mathrm{Precision}=\frac{TP}{TP+FP}
{{< /katex >}}
{{< /colour >}}

When to focus on precision:
- false positives are costly
- example: spam filtering (avoid marking important email as spam)

---

#### Recall (TPR / Sensitivity)

Recall measures **positive case finding ability**:
out of all actual positives, how many did the model find?

{{< colour "blue" >}}
{{< katex display=true >}}
\mathrm{Recall}=\mathrm{TPR}=\frac{TP}{TP+FN}
{{< /katex >}}
{{< /colour >}}

When to focus on recall:
- false negatives are costly
- example: disease screening (missing a positive case is dangerous)

---

#### F1-score

F1 combines precision and recall into one number (harmonic mean).
Useful when data is imbalanced and you want a single metric.

{{< colour "blue" >}}
{{< katex display=true >}}
F_1=\frac{2\,(\mathrm{Precision})(\mathrm{Recall})}{\mathrm{Precision}+\mathrm{Recall}}
{{< /katex >}}
{{< /colour >}}

Rule of thumb:
if $F_1$ is closer to 1, the model has a better balance between precision and recall.

---

#### ROC curve and ROC-AUC

ROC curve plots:
- x-axis: false positive rate (FPR)
- y-axis: true positive rate (TPR)

{{< colour "blue" >}}
{{< katex display=true >}}
\mathrm{FPR}=\frac{FP}{FP+TN}
\qquad
\mathrm{TPR}=\frac{TP}{TP+FN}
{{< /katex >}}
{{< /colour >}}

ROC is mainly used for:
- threshold selection (not always $0.5$)
- performance assessment
- comparing classifiers

AUC (Area Under the ROC Curve):
- $AUC=1$ → perfect model
- $AUC=0.5$ → random guessing
- higher AUC → better separability between classes

How to construct ROC (exam-style steps):
- take a classifier that outputs a continuous score (e.g., probability)
- sort instances by score (descending)
- apply thresholds at each unique score
- compute (TP, FP, TN, FN) at each threshold
- compute TPR and FPR and plot points

---

## References

- [Categorical Embedding](https://medium.com/data-science/categorical-embedding-and-transfer-learning-dd3c4af6345d)
- [One Hot Encoding](https://www.geeksforgeeks.org/machine-learning/ml-one-hot-encoding/)
- [Label Encoding](https://www.geeksforgeeks.org/machine-learning/ml-label-encoding-of-datasets-in-python/)

---

{{< home-link "Home" >}} | {{< section-index >}}
