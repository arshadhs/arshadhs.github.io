---
title: "Ensemble Learning"
draft: false
tags: ["AI", "ML", "Machine Learning", "Ensemble Learning", "Random Forest", "Boosting"]
categories: ["AI", "ML"]
weight: 900
menu: main
---

# Ensemble Learning

Ensemble Learning is a machine learning approach where we combine **multiple models** to produce a stronger final prediction.

Instead of depending on one model, an ensemble uses a group of models and combines their outputs.

The main idea is simple:

> Many weak or moderately good models can work together to produce a better and more stable model.

{{% hint info %}}
**Key takeaway:**  
Ensemble Learning improves prediction by combining several models.

It is especially useful when a single model is unstable, overfits, or does not generalise well.
{{% /hint %}}

- Combining classifiers
- Bagging
- Random Forest
- Boosting
- AdaBoost
- Gradient Boosting
- XGBoost

---

## Why Ensemble Learning Matters

A single model can make mistakes because of:

- noise in the data
- overfitting
- high variance
- weak decision boundaries
- limited training examples
- complex relationships between features and target

Ensemble methods try to reduce these problems by combining several models.

For example, one decision tree may overfit the training data.

But a group of decision trees, if built with randomness and combined properly, can produce a more reliable prediction.

This is the central idea behind **Random Forest**.

---

## Ensemble Learning Map

{{< mermaid >}}
flowchart LR
    A["Ensemble Learning"] --> B["Combining Classifiers"]
    A --> C["Bagging"]
    A --> D["Boosting"]

    C --> E["Random Forest"]

    D --> F["AdaBoost"]
    D --> G["Gradient Boosting"]
    D --> H["XGBoost"]

    style A fill:#E1F5FE,stroke:#5b7db1,color:#222
    style B fill:#C8E6C9,stroke:#5f8f6a,color:#222
    style C fill:#FFF9C4,stroke:#b59b3b,color:#222
    style D fill:#EDE7F6,stroke:#8a6fb3,color:#222
    style E fill:#FFF9C4,stroke:#b59b3b,color:#222
    style F fill:#EDE7F6,stroke:#8a6fb3,color:#222
    style G fill:#EDE7F6,stroke:#8a6fb3,color:#222
    style H fill:#EDE7F6,stroke:#8a6fb3,color:#222
{{< /mermaid >}}

---

## 1. Combining Classifiers ☆

Combining classifiers means using multiple classifiers and combining their predictions.

Each classifier gives an output.

The ensemble then decides the final output using a rule such as:

- majority voting
- weighted voting
- averaging
- weighted averaging

The purpose is to get a final prediction that is more reliable than the prediction from a single model.

---

### Majority Voting

In classification, each model votes for a class.

The class with the maximum number of votes becomes the final prediction.

Example:

| Model | Prediction |
|---|---|
| Model 1 | Yes |
| Model 2 | No |
| Model 3 | Yes |
| Model 4 | Yes |
| Model 5 | No |

Final prediction:

**Yes**, because it receives the majority vote.

---

### Majority Voting Formula

Let there be {{< katex >}} M {{< /katex >}} classifiers.

Each classifier predicts one class.

The ensemble selects the class that receives the highest number of votes.

{{% colour "green" %}}
{{< katex display=true >}}
\hat{y} = \arg\max_{c} \sum_{m=1}^{M} \mathbf{1}(h_m(x)=c)
{{< /katex >}}
{{% /colour %}}

Where:

- {{< katex >}} h_m(x) {{< /katex >}} is the prediction of model {{< katex >}} m {{< /katex >}}
- {{< katex >}} c {{< /katex >}} is a possible class
- {{< katex >}} \mathbf{1}(h_m(x)=c) {{< /katex >}} counts whether model {{< katex >}} m {{< /katex >}} voted for class {{< katex >}} c {{< /katex >}}

---

### Averaging for Regression

For regression, models output numerical values.

The final prediction is usually the average of all predictions.

{{% colour "green" %}}
{{< katex display=true >}}
\hat{y} = \frac{1}{M}\sum_{m=1}^{M} h_m(x)
{{< /katex >}}
{{% /colour %}}

Where:

- {{< katex >}} M {{< /katex >}} is the number of models
- {{< katex >}} h_m(x) {{< /katex >}} is the prediction of model {{< katex >}} m {{< /katex >}}

---

## 2. Bagging ☆

Bagging stands for **Bootstrap Aggregating**.

It is an ensemble method where we train multiple models independently on different random samples of the training data.

The models are trained in parallel.

The final output is obtained by voting or averaging.

---

### Intuition Behind Bagging

Suppose one decision tree overfits.

Instead of relying on one tree, we create many trees.

Each tree sees a slightly different version of the training data.

Because each tree learns a different pattern, their errors may not be identical.

When we combine them, the overall prediction becomes more stable.

---

### Bootstrap Sampling

In bagging, we create multiple training subsets using **sampling with replacement**.

This means:

- some training examples may appear more than once
- some training examples may not appear in a particular subset
- each model gets a slightly different training set

{{% colour "green" %}}
{{< katex display=true >}}
D_m \sim \text{Bootstrap}(D)
{{< /katex >}}
{{% /colour %}}

Where:

- {{< katex >}} D {{< /katex >}} is the original dataset
- {{< katex >}} D_m {{< /katex >}} is the bootstrap sample for model {{< katex >}} m {{< /katex >}}

---

### Bagging Workflow

{{< mermaid >}}
flowchart LR
    A["Original Dataset"] --> B["Bootstrap Sample 1"]
    A --> C["Bootstrap Sample 2"]
    A --> D["Bootstrap Sample 3"]

    B --> E["Model 1"]
    C --> F["Model 2"]
    D --> G["Model 3"]

    E --> H["Combine Predictions"]
    F --> H
    G --> H

    H --> I["Final Output"]

    style A fill:#E1F5FE,stroke:#5b7db1,color:#222
    style B fill:#FFF9C4,stroke:#b59b3b,color:#222
    style C fill:#FFF9C4,stroke:#b59b3b,color:#222
    style D fill:#FFF9C4,stroke:#b59b3b,color:#222
    style E fill:#C8E6C9,stroke:#5f8f6a,color:#222
    style F fill:#C8E6C9,stroke:#5f8f6a,color:#222
    style G fill:#C8E6C9,stroke:#5f8f6a,color:#222
    style H fill:#EDE7F6,stroke:#8a6fb3,color:#222
    style I fill:#E1F5FE,stroke:#5b7db1,color:#222
{{< /mermaid >}}

---

## 3. Random Forest ☆

Random Forest is one of the most important bagging-based ensemble algorithms.

It builds many decision trees and combines their predictions.

The word **forest** means a collection of trees.

The word **random** comes from two types of randomness:

1. Random selection of rows using bootstrap sampling
2. Random selection of features when splitting nodes

---

### Why Random Forest Is Needed

A single decision tree can easily overfit.

It may try to fit every detail in the training data, including noise.

Random Forest reduces this risk by combining many trees.

Each tree is trained on a slightly different dataset and considers different feature subsets.

This makes the trees less correlated.

When less-correlated trees vote together, the final prediction is usually more stable.

---

### Random Forest for Classification

For classification, each tree votes for a class.

The class with the most votes becomes the final prediction.

{{% colour "green" %}}
{{< katex display=true >}}
\hat{y} = \text{mode}\left(h_1(x), h_2(x), \ldots, h_M(x)\right)
{{< /katex >}}
{{% /colour %}}

---

### Random Forest for Regression

For regression, each tree predicts a number.

The final prediction is the average of all tree predictions.

{{% colour "green" %}}
{{< katex display=true >}}
\hat{y} = \frac{1}{M}\sum_{m=1}^{M} h_m(x)
{{< /katex >}}
{{% /colour %}}

---

### Random Forest Process

{{< mermaid >}}
flowchart LR
    A["Training Data"] --> B["Random Rows"]
    A --> C["Random Rows"]
    A --> D["Random Rows"]

    B --> E["Random Feature Subset"]
    C --> F["Random Feature Subset"]
    D --> G["Random Feature Subset"]

    E --> H["Tree 1"]
    F --> I["Tree 2"]
    G --> J["Tree 3"]

    H --> K["Voting or Averaging"]
    I --> K
    J --> K

    K --> L["Random Forest Prediction"]

    style A fill:#E1F5FE,stroke:#5b7db1,color:#222
    style B fill:#FFF9C4,stroke:#b59b3b,color:#222
    style C fill:#FFF9C4,stroke:#b59b3b,color:#222
    style D fill:#FFF9C4,stroke:#b59b3b,color:#222
    style E fill:#C8E6C9,stroke:#5f8f6a,color:#222
    style F fill:#C8E6C9,stroke:#5f8f6a,color:#222
    style G fill:#C8E6C9,stroke:#5f8f6a,color:#222
    style H fill:#EDE7F6,stroke:#8a6fb3,color:#222
    style I fill:#EDE7F6,stroke:#8a6fb3,color:#222
    style J fill:#EDE7F6,stroke:#8a6fb3,color:#222
    style K fill:#E1F5FE,stroke:#5b7db1,color:#222
    style L fill:#C8E6C9,stroke:#5f8f6a,color:#222
{{< /mermaid >}}

---

### Important Random Forest Parameters

| Parameter | Meaning |
|---|---|
| Number of trees | How many decision trees are built |
| Maximum depth | Maximum depth of each tree |
| Minimum samples split | Minimum samples required to split a node |
| Number of features | How many features are considered at each split |
| Bootstrap | Whether sampling with replacement is used |

---

### Advantages of Random Forest

- Reduces overfitting compared with a single decision tree
- Works for classification and regression
- Handles non-linear relationships
- Works well with tabular data
- Gives feature importance
- Usually performs well without heavy tuning

---

### Limitations of Random Forest

- Less interpretable than a single decision tree
- Can be slower than one tree
- May require more memory
- Not ideal when real-time low-latency prediction is required

---

## 4. Boosting ☆

Boosting is another ensemble technique.

Unlike bagging, boosting trains models **sequentially**.

Each new model tries to correct the mistakes made by the previous models.

The main idea is:

> Focus more on the examples that previous models got wrong.

---

### Bagging vs Boosting

| Aspect | Bagging | Boosting |
|---|---|---|
| Training style | Parallel | Sequential |
| Main goal | Reduce variance | Reduce bias and improve weak learners |
| Data handling | Bootstrap samples | Reweights or focuses on errors |
| Base models | Usually strong or unstable models like trees | Usually weak learners |
| Example | Random Forest | AdaBoost, Gradient Boosting, XGBoost |

---

### Boosting Workflow

{{< mermaid >}}
flowchart LR
    A["Training Data"] --> B["Weak Learner 1"]
    B --> C["Find Mistakes"]
    C --> D["Increase Focus on Mistakes"]
    D --> E["Weak Learner 2"]
    E --> F["Find Remaining Mistakes"]
    F --> G["Weak Learner 3"]
    G --> H["Weighted Combination"]
    H --> I["Final Strong Model"]

    style A fill:#E1F5FE,stroke:#5b7db1,color:#222
    style B fill:#C8E6C9,stroke:#5f8f6a,color:#222
    style C fill:#FFF9C4,stroke:#b59b3b,color:#222
    style D fill:#FFF9C4,stroke:#b59b3b,color:#222
    style E fill:#C8E6C9,stroke:#5f8f6a,color:#222
    style F fill:#FFF9C4,stroke:#b59b3b,color:#222
    style G fill:#C8E6C9,stroke:#5f8f6a,color:#222
    style H fill:#EDE7F6,stroke:#8a6fb3,color:#222
    style I fill:#E1F5FE,stroke:#5b7db1,color:#222
{{< /mermaid >}}

---

## 5. AdaBoost ☆

AdaBoost stands for **Adaptive Boosting**.

It is called adaptive because it changes the importance of training examples after each round.

Misclassified examples receive higher weight.

Correctly classified examples receive lower weight.

The next weak learner then focuses more on the difficult examples.

---

### Weak Learner

A weak learner is a model that performs only slightly better than random guessing.

In AdaBoost, the common weak learner is a **decision stump**.

A decision stump is a decision tree with only one split.

---

### AdaBoost Intuition

AdaBoost works like this:

1. Start with equal weight for all training examples.
2. Train a weak learner.
3. Check which examples are misclassified.
4. Increase the weights of misclassified examples.
5. Train the next weak learner on the reweighted data.
6. Combine all weak learners using weighted voting.

---

### Initial Weights

If there are {{< katex >}} N {{< /katex >}} training examples, each example initially receives equal weight.

{{% colour "green" %}}
{{< katex display=true >}}
w_i = \frac{1}{N}
{{< /katex >}}
{{% /colour %}}

---

### Weighted Error

For learner {{< katex >}} t {{< /katex >}}, the weighted error is:

{{% colour "green" %}}
{{< katex display=true >}}
\epsilon_t = \sum_{i=1}^{N} w_i \mathbf{1}\left(y_i \neq h_t(x_i)\right)
{{< /katex >}}
{{% /colour %}}

Where:

- {{< katex >}} w_i {{< /katex >}} is the weight of example {{< katex >}} i {{< /katex >}}
- {{< katex >}} y_i {{< /katex >}} is the actual label
- {{< katex >}} h_t(x_i) {{< /katex >}} is the prediction of weak learner {{< katex >}} t {{< /katex >}}

---

### Learner Weight

A weak learner with lower error gets higher importance in the final model.

{{% colour "green" %}}
{{< katex display=true >}}
\alpha_t = \frac{1}{2}\ln\left(\frac{1-\epsilon_t}{\epsilon_t}\right)
{{< /katex >}}
{{% /colour %}}

Interpretation:

- If error is low, {{< katex >}} \alpha_t {{< /katex >}} is high.
- If error is high, {{< katex >}} \alpha_t {{< /katex >}} is low.
- If the learner is no better than random guessing, it is not useful.

---

### Weight Update

After each weak learner, the example weights are updated.

{{% colour "green" %}}
{{< katex display=true >}}
w_i \leftarrow w_i \exp\left(-\alpha_t y_i h_t(x_i)\right)
{{< /katex >}}
{{% /colour %}}

Then the weights are normalised so that they sum to 1.

---

### Final AdaBoost Classifier

The final model is a weighted combination of weak learners.

{{% colour "green" %}}
{{< katex display=true >}}
H(x) = \text{sign}\left(\sum_{t=1}^{T} \alpha_t h_t(x)\right)
{{< /katex >}}
{{% /colour %}}

Where:

- {{< katex >}} T {{< /katex >}} is the number of weak learners
- {{< katex >}} \alpha_t {{< /katex >}} is the importance of learner {{< katex >}} t {{< /katex >}}
- {{< katex >}} h_t(x) {{< /katex >}} is the weak learner prediction

---

### AdaBoost Notes

- AdaBoost is sequential.
- It gives more attention to misclassified examples.
- It combines weak learners into a strong learner.
- It is sensitive to noisy data and outliers because difficult points receive higher weight.
- The number of learners is a hyperparameter.

---

## 6. Gradient Boosting ☆

Gradient Boosting is another boosting method.

It combines:

- boosting idea
- gradient descent idea
- loss function minimisation

AdaBoost focuses on changing the weights of misclassified examples.

Gradient Boosting focuses on fitting new models to the **errors** or **residuals** of previous models.

---

### Main Idea of Gradient Boosting

Suppose the current model makes predictions.

Some error remains.

The next model is trained to predict that remaining error.

Then this correction is added to the previous model.

This process continues step by step.

---

### Additive Model

Gradient Boosting builds the final model as a sum of smaller models.

{{% colour "green" %}}
{{< katex display=true >}}
F_M(x) = F_0(x) + \sum_{m=1}^{M} \gamma_m h_m(x)
{{< /katex >}}
{{% /colour %}}

Where:

- {{< katex >}} F_M(x) {{< /katex >}} is the final model
- {{< katex >}} F_0(x) {{< /katex >}} is the initial model
- {{< katex >}} h_m(x) {{< /katex >}} is the weak learner at step {{< katex >}} m {{< /katex >}}
- {{< katex >}} \gamma_m {{< /katex >}} is the step size or contribution of learner {{< katex >}} m {{< /katex >}}

---

### Residuals for Squared Error

For regression with squared error loss, the residual is:

{{% colour "green" %}}
{{< katex display=true >}}
r_i = y_i - F_{m-1}(x_i)
{{< /katex >}}
{{% /colour %}}

The next weak learner is trained to predict these residuals.

---

### General Gradient Boosting Idea

For a general loss function, Gradient Boosting uses the negative gradient of the loss.

{{% colour "green" %}}
{{< katex display=true >}}
r_{im} = -\left[\frac{\partial L(y_i, F(x_i))}{\partial F(x_i)}\right]_{F=F_{m-1}}
{{< /katex >}}
{{% /colour %}}

This means the next model is trained in the direction that reduces the loss.

---

### Gradient Boosting Workflow

{{< mermaid >}}
flowchart LR
    A["Initial Model"] --> B["Compute Residuals or Gradients"]
    B --> C["Train Weak Learner"]
    C --> D["Update Model"]
    D --> E["Repeat"]
    E --> F["Final Boosted Model"]

    style A fill:#E1F5FE,stroke:#5b7db1,color:#222
    style B fill:#FFF9C4,stroke:#b59b3b,color:#222
    style C fill:#C8E6C9,stroke:#5f8f6a,color:#222
    style D fill:#EDE7F6,stroke:#8a6fb3,color:#222
    style E fill:#FFF9C4,stroke:#b59b3b,color:#222
    style F fill:#E1F5FE,stroke:#5b7db1,color:#222
{{< /mermaid >}}

---

### Important Gradient Boosting Parameters

| Parameter | Meaning |
|---|---|
| Number of estimators | Number of weak learners added sequentially |
| Learning rate | Controls how much each new learner contributes |
| Maximum tree depth | Controls complexity of each weak learner |
| Loss function | Defines what error the model tries to minimise |
| Subsample | Uses a fraction of data for each learner, if enabled |

---

## 7. XGBoost ☆

XGBoost stands for **Extreme Gradient Boosting**.

It is an efficient and regularised implementation of Gradient Boosting.

It is widely used for structured or tabular data problems.

XGBoost became popular because it is fast, accurate, and includes strong regularisation techniques.

---

### Why XGBoost Is Powerful

XGBoost improves Gradient Boosting by adding:

- regularisation
- efficient tree construction
- handling of missing values
- parallel processing
- shrinkage using learning rate
- column subsampling
- better control over overfitting

---

### XGBoost Objective Function

XGBoost minimises an objective function that contains two parts:

1. Training loss
2. Regularisation term

{{% colour "green" %}}
{{< katex display=true >}}
\text{Objective} = \sum_{i=1}^{N} L(y_i, \hat{y}_i) + \sum_{k=1}^{K} \Omega(f_k)
{{< /katex >}}
{{% /colour %}}

Where:

- {{< katex >}} L(y_i, \hat{y}_i) {{< /katex >}} measures prediction error
- {{< katex >}} \Omega(f_k) {{< /katex >}} penalises model complexity
- {{< katex >}} f_k {{< /katex >}} is an individual tree

---

### Regularisation Term

A common form of tree regularisation is:

{{% colour "green" %}}
{{< katex display=true >}}
\Omega(f) = \gamma T + \frac{1}{2}\lambda \sum_{j=1}^{T} w_j^2
{{< /katex >}}
{{% /colour %}}

Where:

- {{< katex >}} T {{< /katex >}} is the number of leaves
- {{< katex >}} w_j {{< /katex >}} is the score on leaf {{< katex >}} j {{< /katex >}}
- {{< katex >}} \gamma {{< /katex >}} controls penalty for adding leaves
- {{< katex >}} \lambda {{< /katex >}} controls weight regularisation

---

## Bagging, Random Forest, Boosting, AdaBoost, Gradient Boosting and XGBoost

| Method | Core Idea | Training Style | Common Base Learner | Main Strength |
|---|---|---|---|---|
| Bagging | Train models on bootstrap samples | Parallel | Decision trees | Reduces variance |
| Random Forest | Bagging with random feature selection | Parallel | Decision trees | Reduces overfitting of trees |
| Boosting | Each model fixes previous mistakes | Sequential | Weak learners | Builds strong learner |
| AdaBoost | Reweights misclassified examples | Sequential | Decision stumps | Focuses on hard examples |
| Gradient Boosting | Fits new models to residuals or gradients | Sequential | Shallow trees | Minimises loss step by step |
| XGBoost | Regularised, efficient gradient boosting | Sequential with optimisations | Trees | Accuracy and regularisation |

---

## Notes

### Bagging Notes

- Bagging creates multiple datasets using bootstrap sampling.
- Models are trained independently.
- Final output is voting for classification or averaging for regression.
- It mainly reduces variance.
- Random Forest is a major bagging-based method.

### Random Forest Notes

- Random Forest is a collection of decision trees.
- It uses randomness in both rows and features.
- Classification uses majority vote.
- Regression uses average prediction.
- It is less interpretable than a single tree but usually more robust.

### Boosting Notes

- Boosting trains models sequentially.
- Each model focuses on the mistakes of earlier models.
- It can reduce bias and improve weak learners.
- It can overfit if too many learners are used or the data has many outliers.

### AdaBoost Notes

- AdaBoost increases weights of misclassified examples.
- Weak learners with lower error receive higher importance.
- Final prediction is a weighted combination of weak learners.
- It is sensitive to noisy examples.

### Gradient Boosting Notes

- Gradient Boosting fits new learners to residuals or negative gradients.
- It directly minimises a chosen loss function.
- Learning rate is important.
- Smaller learning rate usually needs more learners.

### XGBoost Notes

- XGBoost is an advanced version of gradient boosting.
- It adds regularisation to control overfitting.
- It is widely used for tabular ML tasks.
- It often performs very well in competitions and practical ML problems.

---

## Common Mistakes

| Mistake | Correction |
|---|---|
| Thinking Random Forest and Boosting are the same | Random Forest is bagging-based; Boosting is sequential |
| Thinking all trees in Random Forest use the same data | Each tree uses random rows and random feature subsets |
| Thinking AdaBoost trains all learners independently | AdaBoost trains learners sequentially |
| Ignoring outliers in AdaBoost | AdaBoost may give high weight to noisy points |
| Using too many boosting rounds without control | Use learning rate, validation, and regularisation |
| Assuming XGBoost is just ordinary Gradient Boosting | XGBoost includes regularisation and efficiency improvements |

---

## Practical Interpretation in ML

Use **Random Forest** when:

- you want a strong baseline for tabular data
- decision trees overfit
- interpretability is useful but not the only priority
- you want feature importance

Use **Boosting** when:

- you want high predictive accuracy
- weak learners need to be improved step by step
- you can tune hyperparameters carefully

Use **XGBoost** when:

- the dataset is structured or tabular
- performance is important
- regularisation and tuning are needed
- you want a strong practical ML model

---

## Revision

- Ensemble Learning combines multiple models.
- Classifier combination can use majority voting or weighted voting.
- Regression ensembles usually use averaging.
- Bagging trains models independently on bootstrap samples.
- Random Forest is a bagging method using many decision trees.
- Random Forest reduces overfitting by using random rows and random features.
- Boosting trains models sequentially.
- Boosting focuses on mistakes made by previous models.
- AdaBoost increases weights of misclassified examples.
- Gradient Boosting fits new learners to residuals or negative gradients.
- XGBoost is regularised and efficient Gradient Boosting.

---

## Summary

{{% hint success %}}
**Ensemble Learning** improves model performance by combining multiple learners.

**Bagging** reduces variance by training models independently on random samples.

**Random Forest** is a bagging method based on many decision trees.

**Boosting** builds models sequentially, where each model tries to correct previous mistakes.

**AdaBoost**, **Gradient Boosting**, and **XGBoost** are important boosting methods.
{{% /hint %}}

---
{{< home-link "Home" >}} | {{< section-index >}}
