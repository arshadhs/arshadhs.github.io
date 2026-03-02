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

## References

- [Categorical Embedding](https://medium.com/data-science/categorical-embedding-and-transfer-learning-dd3c4af6345d)
- [One Hot Encoding](https://www.geeksforgeeks.org/machine-learning/ml-one-hot-encoding/)
- [Label Encoding](https://www.geeksforgeeks.org/machine-learning/ml-label-encoding-of-datasets-in-python/)

---

{{< home-link "Home" >}} | {{< section-index >}}


