---
title: 'Support Vector Machine'
date: 2026-05-08
draft: false
weight: 700
tags:
  - AI
  - ML
  - Support Vector Machine
  - SVM
  - Classification
  - Kernel Trick
  - Maximum Margin
categories:
  - AI
  - ML
---

# Support Vector Machine (SVM)

**Support Vector Machine (SVM)** is a **supervised machine learning algorithm** used for:

- **Classification** (most common)
- **Regression** (SVR – Support Vector Regression)

It connects many earlier ideas:

- classification and decision boundaries
- linear classifiers
- margins
- optimisation
- constrained optimisation
- kernels for non-linear data

SVM is a **discriminative classifier**.

That means it does not try to model how each class is generated.

Instead, it tries to find the **best separating boundary** between classes.

{{% hint info %}}
**Key takeaway:**  
SVM tries to find a decision boundary that separates the classes with the **largest possible margin**.

The training points that decide this margin are called **support vectors**.
{{% /hint %}}

---

## Where SVM Fits in Machine Learning ☆

In supervised learning, we normally work with input-output pairs.

For classification, the output is categorical.

For example:

| Input features | Target |
|---|---|
| CGPA, entrance score | admission: yes / no |
| tumour size, cell shape | benign / malignant |
| email words | spam / not spam |

SVM is commonly introduced for **binary classification**, where the target has two classes.

The two classes are often represented as:

{{% colour "green" %}}
{{< katex display=true >}}
y \in \{-1, +1\}
{{< /katex >}}
{{% /colour %}}

For multiclass classification, SVM can be extended using strategies such as:

- one-vs-rest
- one-vs-one

```mermaid
flowchart LR
    A[Training Data] --> B[Find Separating Boundary]
    B --> C[Maximise Margin]
    C --> D[Identify Support Vectors]
    D --> E[Classify New Point]

    style A fill:#E1F5FE
    style B fill:#FFF9C4
    style C fill:#C8E6C9
    style D fill:#EDE7F6
    style E fill:#C8E6C9
```

The central question in SVM is:

> If many boundaries can separate the classes, which one should we choose?

SVM answers:

> Choose the boundary with the **maximum margin**.

---

## Decision Boundary ☆

A linear classifier separates classes using a line in 2D, a plane in 3D, or a hyperplane in higher dimensions.

The decision boundary is written as:

{{% colour "green" %}}
{{< katex display=true >}}
w^T x + b = 0
{{< /katex >}}
{{% /colour %}}

Where:

| Symbol | Meaning |
|---|---|
| {{< katex >}}x{{< /katex >}} | input feature vector |
| {{< katex >}}w{{< /katex >}} | weight vector, perpendicular to the boundary |
| {{< katex >}}b{{< /katex >}} | bias or intercept |
| {{< katex >}}w^T x + b{{< /katex >}} | score used for classification |

The class is predicted using the sign of the score:

{{% colour "green" %}}
{{< katex display=true >}}
f(x) = \operatorname{sign}(w^T x + b)
{{< /katex >}}
{{% /colour %}}

If the score is positive, classify the point as +1.

If the score is negative, classify the point as −1.

---

## Why Not Just Any Separating Line?

For linearly separable data, there may be many possible decision boundaries.

Some boundaries separate the classes but pass very close to the training points.

That is risky because a small change in the input may cause misclassification.

SVM prefers the boundary that is **furthest away from the nearest points of both classes**.

This gives better generalisation.

```mermaid
flowchart TB
    A[Many possible separating lines] --> B[Choose the one with largest margin]
    B --> C[Nearest points define the boundary]
    C --> D[Nearest points become support vectors]

    style A fill:#E1F5FE
    style B fill:#C8E6C9
    style C fill:#FFF9C4
    style D fill:#EDE7F6
```

---

## Margin ☆

The **margin** is the distance between the decision boundary and the nearest training points.

SVM uses two margin boundaries:

{{% colour "green" %}}
{{< katex display=true >}}
w^T x + b = +1
{{< /katex >}}
{{% /colour %}}

{{% colour "green" %}}
{{< katex display=true >}}
w^T x + b = -1
{{< /katex >}}
{{% /colour %}}

The decision boundary lies in the middle:

{{% colour "green" %}}
{{< katex display=true >}}
w^T x + b = 0
{{< /katex >}}
{{% /colour %}}

The margin width is:

{{% colour "green" %}}
{{< katex display=true >}}
\text{Margin Width} = \frac{2}{\|w\|}
{{< /katex >}}
{{% /colour %}}

Therefore, maximising the margin is equivalent to minimising the size of {{< katex >}}w{{< /katex >}}.

---

## Support Vectors ☆

**Support vectors** are the training points closest to the decision boundary.

They lie on or near the margin boundaries.

They are called support vectors because they **support** or **define** the final hyperplane.

If a non-support-vector point is moved slightly, the boundary may not change.

If a support vector is moved, the boundary can change significantly.

{{% hint success %}}
In exams, remember this line:

**Support vectors are the critical training points that determine the maximum-margin hyperplane.**
{{% /hint %}}

---

## Hard Margin SVM ☆

A **hard margin SVM** assumes the data is perfectly linearly separable.

No data point is allowed inside the margin or on the wrong side of the boundary.

The condition is:

{{% colour "green" %}}
{{< katex display=true >}}
y_i(w^T x_i + b) \ge 1
{{< /katex >}}
{{% /colour %}}

This means:

- positive points must be on the positive side
- negative points must be on the negative side
- all points must be outside the margin

The optimisation problem is:

{{% colour "green" %}}
{{< katex display=true >}}
\min_{w,b} \frac{1}{2}\|w\|^2
{{< /katex >}}
{{% /colour %}}

subject to:

{{% colour "green" %}}
{{< katex display=true >}}
y_i(w^T x_i + b) \ge 1
{{< /katex >}}
{{% /colour %}}

Why minimise {{< katex >}}\frac{1}{2}\|w\|^2{{< /katex >}}?

Because maximising the margin {{< katex >}}\frac{2}{\|w\|}{{< /katex >}} is equivalent to minimising {{< katex >}}\|w\|{{< /katex >}}.

The square and the factor {{< katex >}}\frac{1}{2}{{< /katex >}} make the mathematics easier during differentiation.

---

## Hard Margin Intuition

| Situation | Hard Margin Behaviour |
|---|---|
| Classes are perfectly separable | Works well |
| No outliers | Good choice |
| Classes overlap | Fails or gives poor boundary |
| Data has noise | Too strict |

Hard margin is useful for understanding the core mathematics of SVM.

In real data, soft margin SVM is usually more practical.

---

## Lagrange Multiplier View ☆

SVM is a constrained optimisation problem.

To solve it, we can use Lagrange multipliers.

The important result is that the weight vector can be written as:

{{% colour "green" %}}
{{< katex display=true >}}
w = \sum_i \alpha_i y_i x_i
{{< /katex >}}
{{% /colour %}}

Where:

| Symbol | Meaning |
|---|---|
| {{< katex >}}\alpha_i{{< /katex >}} | Lagrange multiplier for training point {{< katex >}}i{{< /katex >}} |
| {{< katex >}}y_i{{< /katex >}} | class label, usually +1 or −1 |
| {{< katex >}}x_i{{< /katex >}} | training data point |

A very important interpretation is:

{{% hint warning %}}
Only training points with non-zero {{< katex >}}\alpha_i{{< /katex >}} become support vectors.

Most other training points have {{< katex >}}\alpha_i = 0{{< /katex >}}, so they do not directly affect the final decision boundary.
{{% /hint %}}

Once {{< katex >}}w{{< /katex >}} is known, the bias can be calculated using any support vector:

{{% colour "green" %}}
{{< katex display=true >}}
b = y_i - w^T x_i
{{< /katex >}}
{{% /colour %}}

The classifier becomes:

{{% colour "green" %}}
{{< katex display=true >}}
f(x) = \operatorname{sign}\left(\sum_i \alpha_i y_i (x_i^T x) + b\right)
{{< /katex >}}
{{% /colour %}}

The term {{< katex >}}x_i^T x{{< /katex >}} is a dot product between a training point and a new test point.

This dot product becomes important when we discuss kernels.

---

## Soft Margin SVM ☆

Real-world data is often noisy.

The classes may overlap.

A strict hard margin may not be possible.

Soft margin SVM allows some violations using slack variables.

The soft margin constraint is:

{{% colour "green" %}}
{{< katex display=true >}}
y_i(w^T x_i + b) \ge 1 - \xi_i
{{< /katex >}}
{{% /colour %}}

Where {{< katex >}}\xi_i{{< /katex >}} is the slack variable.

It measures how much a point violates the margin.

The soft margin objective is:

{{% colour "green" %}}
{{< katex display=true >}}
\min_{w,b} \frac{1}{2}\|w\|^2 + C\sum_i \xi_i
{{< /katex >}}
{{% /colour %}}

Where {{< katex >}}C{{< /katex >}} controls the penalty for margin violations.

---

## Role of C in Soft Margin SVM ☆

The hyperparameter {{< katex >}}C{{< /katex >}} controls the trade-off between:

- large margin
- low training error

| Value of {{< katex >}}C{{< /katex >}} | Behaviour | Interpretation |
|---|---|---|
| Small {{< katex >}}C{{< /katex >}} | Allows more violations | Wider margin, more tolerance |
| Large {{< katex >}}C{{< /katex >}} | Penalises violations strongly | Narrower margin, tries to classify training data correctly |

{{% hint info %}}
Low {{< katex >}}C{{< /katex >}} means the model is more tolerant of misclassification.

High {{< katex >}}C{{< /katex >}} means the model tries harder to avoid training errors.
{{% /hint %}}

A very large {{< katex >}}C{{< /katex >}} can overfit noisy data.

A very small {{< katex >}}C{{< /katex >}} can underfit.

---

## Hinge Loss

Soft margin SVM is closely related to hinge loss.

For one training example, hinge loss is:

{{% colour "green" %}}
{{< katex display=true >}}
\max(0, 1 - y_i(w^T x_i + b))
{{< /katex >}}
{{% /colour %}}

Interpretation:

| Condition | Hinge Loss |
|---|---|
| Correct and outside margin | 0 |
| Correct but inside margin | Positive loss |
| Misclassified | Larger positive loss |

SVM does not only care whether a point is classified correctly.

It also cares whether the point is classified with enough margin.

---

## Linearly Separable Data ☆

Data is linearly separable when a straight line or hyperplane can separate the classes.

For example, in two dimensions, a single line can separate positive and negative points.

A linear SVM is suitable when this is possible.

{{% colour "green" %}}
{{< katex display=true >}}
w^T x + b = 0
{{< /katex >}}
{{% /colour %}}

The aim is to choose the line with the maximum margin.

---

## Non-Linearly Separable Data ☆

Data is non-linearly separable when no straight line can separate the classes properly.

For example, one class may surround another class in a circular pattern.

In the original input space, a linear boundary fails.

The key idea is:

> Map the data into a higher-dimensional feature space where it may become linearly separable.

Example transformation:

{{% colour "green" %}}
{{< katex display=true >}}
x \mapsto (x, x^2)
{{< /katex >}}
{{% /colour %}}

In the original space, the boundary may be curved.

In the transformed space, the boundary can become linear.

```mermaid
flowchart LR
    A[Original Space: Not Linearly Separable] --> B[Feature Transformation]
    B --> C[Higher-Dimensional Space]
    C --> D[Linear Separation Possible]

    style A fill:#FFF9C4
    style B fill:#E1F5FE
    style C fill:#EDE7F6
    style D fill:#C8E6C9
```

---

## Kernel Trick ☆

Mapping data explicitly into a high-dimensional space can be computationally expensive.

The **kernel trick** avoids computing the transformed features directly.

Instead, it computes the dot product in the transformed space using a kernel function.

If {{< katex >}}\phi(x){{< /katex >}} is a transformation into a higher-dimensional space, then:

{{% colour "green" %}}
{{< katex display=true >}}
K(x_i, x_j) = \phi(x_i)^T\phi(x_j)
{{< /katex >}}
{{% /colour %}}

This means SVM can behave as if it is working in a higher-dimensional space without explicitly constructing all the transformed features.

The classifier becomes:

{{% colour "green" %}}
{{< katex display=true >}}
f(x) = \operatorname{sign}\left(\sum_i \alpha_i y_i K(x_i, x) + b\right)
{{< /katex >}}
{{% /colour %}}

---

## Common Kernel Functions ☆

| Kernel | Formula | When Useful |
|---|---|---|
| Linear | {{< katex >}}K(x_i,x_j)=x_i^T x_j{{< /katex >}} | linearly separable or high-dimensional sparse data |
| Polynomial | {{< katex >}}K(x_i,x_j)=(x_i^T x_j+c)^d{{< /katex >}} | curved boundaries with polynomial structure |
| RBF / Gaussian | {{< katex >}}K(x_i,x_j)=\exp(-\gamma\|x_i-x_j\|^2){{< /katex >}} | complex non-linear boundaries |
| Sigmoid | {{< katex >}}K(x_i,x_j)=\tanh(\alpha x_i^T x_j+c){{< /katex >}} | neural-network-like similarity behaviour |

The RBF kernel is popular because it can model very flexible non-linear decision boundaries.

---

## Mercer Condition

Not every similarity function is a valid SVM kernel.

A kernel should correspond to an inner product in some feature space.

Mercer's condition gives the mathematical requirement for a valid kernel.

A practical way to remember it:

{{% hint info %}}
A valid kernel must produce a kernel matrix that behaves like a proper inner-product matrix.

In simple terms, the kernel matrix should be symmetric and positive semi-definite.
{{% /hint %}}

For a set of training points, the kernel matrix is:

{{% colour "green" %}}
{{< katex display=true >}}
K_{ij} = K(x_i, x_j)
{{< /katex >}}
{{% /colour %}}

A valid kernel should satisfy:

{{% colour "green" %}}
{{< katex display=true >}}
K(x_i, x_j) = K(x_j, x_i)
{{< /katex >}}
{{% /colour %}}

and should be positive semi-definite.

---

## Linear SVM vs Kernel SVM

| Aspect | Linear SVM | Kernel SVM |
|---|---|---|
| Boundary | straight line / hyperplane | curved boundary in original space |
| Feature space | original features | implicit transformed features |
| Speed | faster | usually slower |
| Interpretability | easier | harder |
| Best for | linearly separable or high-dimensional sparse data | non-linear class patterns |

---

## Structured and Unstructured Data Applications

SVM can be used with both structured and unstructured data.

### Structured Data

Structured data is usually tabular.

Examples:

- medical diagnosis from patient attributes
- loan approval using income, credit score and account history
- customer churn prediction
- admission prediction using scores and profile attributes

For structured data, features are already in columns.

SVM can directly use numerical feature vectors after preprocessing and scaling.

### Unstructured Data

Unstructured data includes text, images, audio and documents.

SVM cannot directly process raw text or raw images.

They must first be converted into feature vectors.

Examples:

| Data Type | Feature Representation | SVM Use |
|---|---|---|
| Text | bag-of-words, TF-IDF, embeddings | spam detection, sentiment classification |
| Images | HOG, SIFT, CNN embeddings | object recognition, face detection |
| Documents | TF-IDF, topic vectors | document classification |
| Audio | MFCC features | speaker or sound classification |

SVM often performs well when the number of features is large, especially in text classification.

---

## Why Feature Scaling Matters ☆

SVM depends on distances, dot products and margins.

If one feature has a much larger scale than another, it can dominate the decision boundary.

For example:

| Feature | Range |
|---|---|
| Age | 18 to 80 |
| Income | 10,000 to 500,000 |

Income may dominate the classifier unless scaling is applied.

Common scaling methods:

- standardisation
- min-max normalisation

{{% hint warning %}}
For SVM, feature scaling is usually essential.

Always fit the scaler on the training data only, then transform both training and test data.
{{% /hint %}}

---

## Worked Mini Example ☆

Suppose an SVM has learned:

{{% colour "green" %}}
{{< katex display=true >}}
w = (1, -1), \quad b = -0.5
{{< /katex >}}
{{% /colour %}}

For a new point:

{{% colour "green" %}}
{{< katex display=true >}}
x = (2, 1)
{{< /katex >}}
{{% /colour %}}

Compute the score:

{{% colour "green" %}}
{{< katex display=true >}}
w^T x + b = (1)(2) + (-1)(1) - 0.5 = 0.5
{{< /katex >}}
{{% /colour %}}

Since the score is positive:

{{% colour "green" %}}
{{< katex display=true >}}
f(x) = +1
{{< /katex >}}
{{% /colour %}}

So the new point is classified as the positive class.

---

## SVM Compared with Logistic Regression

| Feature | Logistic Regression | SVM |
|---|---|---|
| Main output | probability | class boundary / score |
| Main idea | estimate probability using sigmoid | maximise margin |
| Boundary | usually linear unless features are transformed | linear or kernel-based non-linear |
| Loss | log loss | hinge loss |
| Outlier sensitivity | can be affected | soft margin controls effect through {{< katex >}}C{{< /katex >}} |
| Interpretability | probability is easier to explain | margin-based explanation |

Logistic regression is useful when probability interpretation matters.

SVM is useful when a strong separating boundary is more important.

---

## SVM Compared with Decision Trees

| Feature | Decision Tree | SVM |
|---|---|---|
| Decision boundary | axis-aligned splits | maximum-margin hyperplane |
| Interpretability | high for small trees | moderate to low |
| Feature scaling | usually not required | important |
| Non-linear data | handled by tree splits | handled by kernels |
| Overfitting control | pruning, depth, minimum samples | {{< katex >}}C{{< /katex >}}, kernel choice, {{< katex >}}\gamma{{< /katex >}} |

---

## Practical Scikit-Learn Example

```python
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler
from sklearn.svm import SVC
from sklearn.metrics import accuracy_score, classification_report
from sklearn.pipeline import Pipeline

# X = input features, y = class labels
X_train, X_test, y_train, y_test = train_test_split(
    X,
    y,
    test_size=0.2,
    random_state=42,
    stratify=y
)

model = Pipeline([
    ("scaler", StandardScaler()),
    ("svm", SVC(kernel="rbf", C=1.0, gamma="scale"))
])

model.fit(X_train, y_train)
y_pred = model.predict(X_test)

print("Accuracy:", accuracy_score(y_test, y_pred))
print(classification_report(y_test, y_pred))
```

The pipeline is important because scaling must be learned only from the training data.

---

## Common Exam Questions ☆

### 1. Define SVM

SVM is a supervised discriminative classifier that finds the maximum-margin decision boundary between classes.

### 2. What are support vectors?

Support vectors are the training points closest to the decision boundary.

They determine the final hyperplane and margin.

### 3. What is the difference between hard margin and soft margin?

Hard margin does not allow any violation.

Soft margin allows some violations using slack variables and controls the penalty through {{< katex >}}C{{< /katex >}}.

### 4. What is the kernel trick?

The kernel trick computes dot products in a transformed feature space without explicitly computing the transformation.

### 5. Why is SVM useful for non-linear data?

Kernel SVM can map data implicitly into a higher-dimensional feature space where a linear separator may exist.

---

## Common Mistakes

| Mistake | Correction |
|---|---|
| Thinking SVM only draws any separating line | SVM chooses the maximum-margin separator |
| Forgetting support vectors | Support vectors define the margin and boundary |
| Using hard margin for noisy data | Use soft margin with suitable {{< katex >}}C{{< /katex >}} |
| Forgetting scaling | Scale features before SVM training |
| Thinking kernels explicitly create features | Kernel trick avoids explicit transformation |
| Treating SVM output as probability by default | SVM gives a decision score unless probability calibration is enabled |

---

## Quick Revision Summary

- SVM is mainly a supervised classification algorithm.
- It is a discriminative classifier.
- It finds the best decision boundary by maximising the margin.
- Support vectors are the closest points to the boundary.
- Hard margin assumes perfect separability.
- Soft margin allows violations using slack variables.
- {{< katex >}}C{{< /katex >}} controls the trade-off between margin width and training error.
- Kernel trick helps SVM handle non-linear data.
- Linear, polynomial and RBF are common kernels.
- Feature scaling is very important for SVM.

---

## References

- Course handout: Module 7, Support Vector Machines.
- Lecture transcript: Support Vector Machine sessions by Prof. Indumathi Prabakeran.
- Course slides: Instance-based Learning and SVM transition material.
- C. J. C. Burges, *A Tutorial on Support Vector Machines for Pattern Recognition*.

---
{{< home-link "Home" >}} | {{< section-index >}}
