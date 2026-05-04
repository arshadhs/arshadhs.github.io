---
title: 'Instance-based Learning'
draft: false
tags: ["AI", "ML"]
categories: ["AI", "ML"]
weight: 600
---

# Instance-based Learning

Instance-based learning is a family of methods that **do not build one explicit global model during training**.

Instead, they **store training examples** and delay most of the work until a new query arrives.

When a new point must be classified or predicted, the algorithm compares it with previously seen examples, finds the most relevant neighbours, and uses them to produce the answer.

Instance-based Learning covers three linked ideas:

- $k$-Nearest Neighbour (KNN) learning
- Locally Weighted Regression (LWR)
- Radial Basis Functions (RBF)

{{% hint info %}}
**Key takeaway:** Instance-based learning is a **lazy learning** approach. Training is usually cheap because we mainly store the data. Prediction is more expensive because the algorithm must search for similar records at query time. The most important ideas for exams are: choosing a good distance measure, choosing $k$, understanding weighted neighbours, and knowing why nearby data should matter more than faraway data.
{{% /hint %}}

---

### Comparison: Instance-Based vs. Model-Based


| Feature | Instance-Based Learning | Model-Based Learning |
| :--- | :--- | :--- |
| **Approach** | Memorizes examples | Learns general patterns/rules |
| **Training Time** | Minimal (just stores data) | Can be long (building the model) |
| **Prediction Time** | Slow (searches all data) | Fast (uses a pre-built formula) |
| **Storage** | High (must keep all data) | Low (only stores the model) |
| **Adaptability** | Adapts instantly to new data | Requires retraining to update |

---

{{< mermaid >}}
graph LR
    IBL[Instance-Based Learning]
    
    %% Main Branches
    IBL --> Numeric[Numerical / Distance-Based]
    IBL --> Symbolic[Symbolic / Reasoning-Based]
    IBL --> Density[Density / Distribution-Based]
    
    %% Numerical Examples
    Numeric --> KNN[k-Nearest Neighbours]
    Numeric --> LWR[Locally Weighted Regression]
    Numeric --> RBF[Radial Basis Functions]
    Numeric --> LVQ[Learning Vector Quantization]
    
    %% Symbolic Examples
    Symbolic --> CBR[Case-Based Reasoning]
    
    %% Density Examples
    Density --> KDE[Kernel Density Estimation]
    Density --> PW[Parzen Windows]
    
    %% Styling
    style IBL fill:#f9f,stroke:#333,stroke-width:4px
    style Numeric fill:#bbf,stroke:#333
    style Symbolic fill:#dfd,stroke:#333
    style Density fill:#ffd,stroke:#333
{{< /mermaid >}}

---

## Model-based vs Instance-based Learning

**Model-based** includes:

- Linear Regression
- Logistic Regression
- Decision Trees

These methods try to learn a compact predictive model during training. Instance-based learning behaves differently: it keeps the training data and postpones most of the computation until prediction time.

{{< mermaid >}}
flowchart LR
  A[Training data] --> B{Learning style}
  B --> C[Model-based learning]
  B --> D[Instance-based learning]
  C --> E[Learn parameters or rules during training]
  C --> F[More training time]
  C --> G[Less prediction time]
  D --> H[Store training instances]
  D --> I[Less training time]
  D --> J[More prediction time]
{{< /mermaid >}}

### Eager Learners vs Lazy Learners

- **Model-based methods** are often called **eager learners**
- **Instance-based methods** are often called **lazy learners**

- Eager learner:
  spends more effort during training,
  less effort during prediction
- Lazy learner:
  spends little effort during training,
  more effort during prediction

This distinction is one of the most important conceptual points.

---

## Core Intuition

A simple intuition:

> "If it walks like a duck and quacks like a duck, it is probably a duck."

The machine learning version:

> "If a new point looks similar to known points of a certain class, it is likely to belong to that class."

The whole method depends on two questions:

1. How do we define **similarity** or **distance**?
2. How many neighbours should influence the final answer?

---

## General Workflow

{{< mermaid >}}
flowchart TD
  A[Store training examples] --> B[Receive query point x_q]
  B --> C[Compute distance or similarity]
  C --> D[Select relevant neighbours]
  D --> E{Task type}
  E --> F[Classification: vote]
  E --> G[Regression: average or weighted average]
  F --> H[Predicted class]
  G --> I[Predicted value]
{{< /mermaid >}}

In words:

1. Store the training set
2. Receive a query point $x_q$
3. Compute the distance between $x_q$ and training points
4. Select one or more nearest neighbours
5. Use those neighbours to infer the output

---

## k-Nearest Neighbour (KNN) Learning

KNN is the most commonly discussed instance-based learning algorithm. It can be used for both **classification** and **regression**.

### Basic Notation

Suppose the training set contains examples $\langle x_i, f(x_i) \rangle$. For a new query point $x_q$, find the nearest examples and use their outputs to estimate $f^*(x_q)$.

For **1-NN**:

{{% colour "blue" %}}
{{< katex display=true >}}
f^*(x_q)=f(x_n)
{{< /katex >}}
{{% /colour %}}

where $x_n$ is the nearest training example to $x_q$.

### KNN for Classification

For a discrete target, use **majority vote** among the $k$ nearest neighbours:

{{% colour "blue" %}}
{{< katex display=true >}}
\hat{f}(x_q)=\arg\max_{v \in V}\sum_{i=1}^{k}\delta\!\big(v,f(x_i)\big)
{{< /katex >}}
{{% /colour %}}

where:

- $V$ is the set of possible class labels
- $\delta(a,b)=1$ if $a=b$, otherwise $0$

### KNN for Regression

For a real-valued target, use the average (or median) of the outputs of the $k$ nearest neighbours:

{{% colour "blue" %}}
{{< katex display=true >}}
\hat{f}(x_q)=\frac{1}{k}\sum_{i=1}^{k} f(x_i)
{{< /katex >}}
{{% /colour %}}

Median can be more robust when outliers are present.

---

## Training and Prediction in KNN

### Training Phase

KNN does almost no parameter learning. It mainly stores input features and corresponding outputs, so training is simple and fast.

### Prediction Phase

Prediction is costlier because for each query point the algorithm must:

- compare the query with many or all training examples
- compute distances
- sort or identify nearest points
- then vote or average

This is why KNN is called a lazy learner.

---

## Step-by-Step KNN Procedure

### For Classification

1. Take the query point $x_q$
2. Compute distance from $x_q$ to every training point
3. Sort distances from smallest to largest
4. Select the first $k$ neighbours
5. Count the class labels among them
6. Return the class with the highest count

### For Regression

1. Take the query point $x_q$
2. Compute distance to all training points
3. Select the first $k$ neighbours
4. Average their target values
5. Return the average as the prediction

---

## Choosing the Value of $k$

The value of $k$ is the key hyperparameter in KNN.

### Why $k$ Matters

- If $k$ is too small, the model becomes too sensitive to noise
- If $k$ is too large, the model becomes too smooth and may ignore local structure

### Practical Recommendations

In classification, using an **odd** value of $k$ is often recommended for binary classification, because it helps avoid ties in voting. Using an even $k$ may result in equal votes for two classes.

### Rule of Thumb

{{% colour "blue" %}}
{{< katex display=true >}}
k=\sqrt{N}
{{< /katex >}}
{{% /colour %}}

where $N$ is the number of training points. This is only a starting point, not a guaranteed best choice.

### Elbow Method for Selecting $k$

1. Try several values of $k$
2. Compute an error function such as SSE
3. Plot error against $k$
4. Choose the point after which improvement becomes much smaller (the **elbow**)

A common example in the lesson identifies $k=3$ as the visual elbow point.

### Interpretation of Small vs Large $k$

| $k$ size | Behaviour |
|----------|-----------|
| Small | High variance, can overfit, highly local |
| Large | Higher bias, smoother boundary, less sensitive to noise |

---

## Numerical Template: KNN Problems

This is a very common exam pattern.

1. Write the query point
2. Compute distance from the query to every training point
3. Sort the distances in ascending order
4. Take the first $k$ rows
5. Apply the decision rule:
   - **Classification**: count labels and apply majority vote
   - **Regression**: average the target values of the selected neighbours
6. State the conclusion clearly: *"Hence the predicted class is ..."* or *"Hence the predicted value is ..."*

---

## Why Distance Matters

KNN does not learn a separating equation in advance, so the quality of its prediction depends heavily on the quality of the distance or similarity function. If the distance is badly chosen, the "nearest" neighbours may not be the most meaningful ones.

---

## Data Matrix vs Distance Matrix

| Object | Description |
|--------|-------------|
| Data matrix | $n$ data points, each with $p$ attributes |
| Distance/dissimilarity matrix | Pairwise distances; symmetric; often shown in triangular form |

---

## Measures of Distance and Similarity

### Properties of a Valid Metric

For a valid distance function $d(i,j)$:

- $d(i,j) > 0$ if $i \neq j$ (positive definiteness)
- $d(i,i)=0$ (identity)
- $d(i,j)=d(j,i)$ (symmetry)
- $d(i,j) \le d(i,k)+d(k,j)$ (triangle inequality)

### Minkowski Distance

A general distance family used in the lesson:

{{% colour "blue" %}}
{{< katex display=true >}}
d(i,j)=\left(\sum_{f=1}^{p}|x_{if}-x_{jf}|^h\right)^{1/h}
{{< /katex >}}
{{% /colour %}}

where:

- $i=(x_{i1},x_{i2},\dots,x_{ip})$
- $j=(x_{j1},x_{j2},\dots,x_{jp})$
- $h$ is the order of the norm

#### Special Cases

**Manhattan distance ($L_1$)**

{{% colour "blue" %}}
{{< katex display=true >}}
d(i,j)=\sum_{f=1}^{p}|x_{if}-x_{jf}|
{{< /katex >}}
{{% /colour %}}

**Euclidean distance ($L_2$)**

{{% colour "blue" %}}
{{< katex display=true >}}
d(i,j)=\sqrt{\sum_{f=1}^{p}(x_{if}-x_{jf})^2}
{{< /katex >}}
{{% /colour %}}

**Supremum distance ($L_\infty$)**

{{% colour "blue" %}}
{{< katex display=true >}}
d(i,j)=\max_f |x_{if}-x_{jf}|
{{< /katex >}}
{{% /colour %}}

#### When to Use Which

- **Manhattan**: useful when absolute coordinate-wise difference matters
- **Euclidean**: the most common geometric distance
- **Supremum**: controlled by the largest coordinate difference

---

## Similarity for Different Attribute Types

### Nominal Attributes

Nominal attributes have no natural order (e.g., profession, colour, mother tongue).

- **Simple matching**: matching values contribute similarity; differing values contribute dissimilarity
- **Binary indicator encoding**: create one binary feature per possible nominal state (similar to one-hot encoding)

### Binary Attributes

- **Symmetric binary**: both states are equally important (e.g., gender)
- **Asymmetric binary**: one state is more important, common in presence/absence situations (e.g., symptoms, test results)

**Jaccard similarity** for asymmetric binary variables:

{{% colour "blue" %}}
{{< katex display=true >}}
\mathrm{sim}_{\mathrm{Jaccard}}(i,j)=\frac{q}{q+r+s}
{{< /katex >}}
{{% /colour %}}

where $q$ counts shared positives. The related dissimilarity is:

{{% colour "blue" %}}
{{< katex display=true >}}
d(i,j)=\frac{r+s}{q+r+s}
{{< /katex >}}
{{% /colour %}}

### Ordinal Attributes

Ordinal values have an order (e.g., Village → Small Town → Suburban → Metropolitan).

Recommended approach:

1. Replace values by ranks
2. Map ranks to $[0,1]$
3. Treat them like interval-scaled variables

### Mixed Data Types and Gower Distance

Real datasets often contain a mixture of:

- nominal attributes
- binary attributes
- numeric attributes
- ordinal attributes

The **Gower distance** handles mixed types by:

- computing component-wise dissimilarity per attribute type
- normalising appropriately
- combining with equal or chosen weights

**Typical exam workflow for mixed data:**

1. Normalise numeric attributes
2. Rank and scale ordinal attributes
3. Compare nominal attributes by match/mismatch
4. Combine the partial distances

---

## Standardising Numeric Data

KNN is sensitive to scale. If one numeric feature has a much larger range than another, it can dominate the distance computation.

**Z-score standardisation:**

{{% colour "blue" %}}
{{< katex display=true >}}
z=\frac{x-\mu}{\sigma}
{{< /katex >}}
{{% /colour %}}

where $x$ is the raw value, $\mu$ is the mean, and $\sigma$ is the standard deviation.

Min-max normalisation is also used in some exam questions.

### Why Feature Scaling Is Essential

Without scaling:

- High-range numeric features dominate
- The neighbourhood becomes misleading
- Prediction quality drops

This is one of the most common practical mistakes in KNN.

---

## Curse of Dimensionality

### Main Idea

If you use many features and only a few are truly relevant, two genuinely similar records may appear far apart because distance is computed across many irrelevant dimensions.

### Why It Happens

Traditional KNN uses **all attributes** unless you intervene. Irrelevant attributes distort the geometry of the space.

### Remedies

**1. Remove less relevant attributes.** If exploration suggests some features are irrelevant, remove them and validate performance again.

**2. Weight attributes differently.** Give lower weight to less relevant features and higher weight to more relevant ones. Conceptually, this means shortening less relevant axes and lengthening more relevant ones.

### Key Exam Wording

- High-dimensional instance spaces can mislead nearest neighbour methods
- KNN works best when relevant similarity is well captured by the chosen distance function

---

## Challenges of KNN

| Challenge | Description |
|-----------|-------------|
| Choosing $k$ | A hyperparameter; bad choice causes overfitting or over-smoothing |
| Choosing the distance metric | Results differ across Euclidean, Manhattan, mixed-type, and weighted distance |
| Query-time cost | Each prediction may require comparison against all training points |
| Noise and irrelevant features | Can mislead the nearest-neighbour search |

---

## Distance-weighted KNN

### Why Plain KNN May Fail

Suppose a query point lies near a few very close positive examples, but the majority of slightly farther neighbours are negative. A plain majority vote may incorrectly predict the negative class.

Distance-weighted KNN fixes this by assigning **larger influence to closer points**.

### Core Idea

- Near neighbours get high weight
- Far neighbours get low weight

The question becomes: "How much weighted evidence does each class receive?" rather than simply "How many neighbours belong to each class?"

### Weight Function via a Kernel

{{% colour "blue" %}}
{{< katex display=true >}}
w_i = K\big(d(x_q,x_i)\big)
{{< /katex >}}
{{% /colour %}}

Example kernels:

{{% colour "blue" %}}
{{< katex display=true >}}
K\big(d(x_q,x_i)\big)=\frac{1}{d(x_q,x_i)^2}
{{< /katex >}}
{{% /colour %}}

{{% colour "blue" %}}
{{< katex display=true >}}
K\big(d(x_q,x_i)\big)=\frac{1}{(d_0+d(x_q,x_i))^2}
{{< /katex >}}
{{% /colour %}}

{{% colour "blue" %}}
{{< katex display=true >}}
K\big(d(x_q,x_i)\big)=\exp\left(-\left(\frac{d(x_q,x_i)}{\sigma_0}\right)^2\right)
{{< /katex >}}
{{% /colour %}}

### Weighted Classification Rule

{{% colour "blue" %}}
{{< katex display=true >}}
\hat{f}(x_q)=\arg\max_{v \in V}\sum_{i=1}^{k} w_i\,\delta\!\big(v,f(x_i)\big)
{{< /katex >}}
{{% /colour %}}

### Weighted Regression Rule

{{% colour "blue" %}}
{{< katex display=true >}}
\hat{f}(x_q)=\frac{\sum_{i=1}^{k} w_i f(x_i)}{\sum_{i=1}^{k} w_i}
{{< /katex >}}
{{% /colour %}}

**Interpretation:** A larger distance means smaller weight; a smaller distance means larger weight. Similarity increases the influence of that training instance on the final prediction.

---

## Numerical Template: Weighted KNN

1. Prepare the data: normalise numeric variables, rank ordinal variables, handle nominal attributes using match/mismatch distance
2. Compute the distance from query $x_q$ to each record
3. Apply the kernel function to each distance to compute the weight
4. For classification: sum total weight class-wise
5. Choose the class with the largest total weight
6. Explain the conclusion, noting how plain KNN may prefer the majority class while weighted KNN may prefer the more similar class

---

## Locally Weighted Regression (LWR)

LWR is the regression counterpart of the "nearby points should matter more" idea, and is one of the key syllabus items in this module.

### Intuition

- **Global linear regression** fits **one line** for all data points
- **Locally weighted regression** fits a **small weighted local model around the query point**

If the query changes, its local fitted line may also change.

### How LWR Differs from Ordinary Linear Regression

| Ordinary Linear Regression | Locally Weighted Regression |
|---------------------------|------------------------------|
| One global hypothesis | Local model per query point |
| Shared slope and intercept | Coefficients depend on the query |
| Same line used for all predictions | Different local line for different queries |

### LWR Workflow

1. Take query point $x_q$
2. Compute distance from every training point to $x_q$
3. Convert distances to weights using a kernel
4. Fit a weighted local regression model
5. Evaluate the fitted model at $x_q$
6. Return the local prediction

### LWR as Weighted Least Squares

{{% colour "blue" %}}
{{< katex display=true >}}
J_q(w)=\frac{1}{2}\sum_{i=1}^{n} w_i(x_q)\big(h_w(x_i)-y_i\big)^2
{{< /katex >}}
{{% /colour %}}

where $w_i(x_q)$ is the weight of training point $x_i$ relative to $x_q$, and $h_w(x_i)$ is the local regression prediction.

The final query prediction is:

{{% colour "blue" %}}
{{< katex display=true >}}
\hat{y}_q = h_{w^{(q)}}(x_q)
{{< /katex >}}
{{% /colour %}}

The effective parameters $w^{(q)}$ depend on the query point, so the fitted local model changes from query to query.

### Two Design Viewpoints

**View 1 — Local approximation:** Use only nearby neighbours, then fit a local regression on that neighbourhood.

**View 2 — Weighted global approximation:** Use all data, but multiply each error term by a weight that decreases with distance from the query. Far points contribute very little. This gives the benefit of using the full dataset while still focusing on the neighbourhood.

### Kernel Choice and Bandwidth

The kernel is not "one formula for all datasets" — experimentation is needed. Possible choices include inverse distance, inverse squared distance, exponential/Gaussian weighting, and quadratic kernel variants.

**Bandwidth** is an important parameter that guides locality:

- **Small bandwidth**: very local behaviour, strong emphasis on nearby points
- **Large bandwidth**: smoother behaviour, broader neighbourhood influence

### When LWR Is Useful

- One single global linear trend is too crude
- The relationship varies across the input space
- Nearby points are more informative than distant ones

### Simple Exam Explanation of LWR

> "Locally weighted regression predicts a query by fitting a small regression model around the query point, with higher weights for nearby training examples and lower weights for faraway ones."

### Numerical Template: LWR Questions

1. Identify the query point $x_q$
2. Compute distance from each training point to $x_q$
3. Apply the chosen kernel to obtain weights
4. Form the weighted local loss
5. Fit the local coefficients or interpret the local fitted curve
6. Use the resulting local model to compute $\hat{y}_q$
7. Explain why the answer differs from a global regression line

---

## Relationship Between Weighted KNN and LWR

| | Distance-weighted KNN | LWR |
|---|---|---|
| Local regression fit | No | Yes |
| Uses neighbour outputs directly | Yes | No |
| Decision rule | Weighted vote or weighted average | Value from local fitted model |

LWR is a more structured local regression version of the "nearby points matter more" principle.

---

## Radial Basis Functions (RBF)

RBF is listed explicitly in the course handout as part of the core syllabus.

### Core Idea

An RBF is a function whose value depends mainly on the **distance from a centre**. A common Gaussian RBF:

{{% colour "blue" %}}
{{< katex display=true >}}
\phi_j(x)=\exp\left(-\frac{\|x-c_j\|^2}{2\sigma^2}\right)
{{< /katex >}}
{{% /colour %}}

where $c_j$ is the centre of the basis function and $\sigma$ controls the spread.

**Interpretation:**

- If $x$ is close to $c_j$, then $\phi_j(x)$ is large
- If $x$ is far from $c_j$, then $\phi_j(x)$ becomes small

### RBF Model Form

{{% colour "blue" %}}
{{< katex display=true >}}
\hat{y}(x)=w_0+\sum_{j=1}^{M} w_j\,\phi_j(x)
{{< /katex >}}
{{% /colour %}}

This is linear in the weights $w_j$ but non-linear in the original input $x$.

### Why RBF Belongs in This Module

RBF shares the same local-neighbourhood philosophy as KNN and LWR:

- **KNN**: use nearby examples more
- **LWR**: fit a local weighted model around the query
- **RBF**: build features that are naturally strongest near selected centres and weaker farther away

---

## KNN, LWR, and RBF at a Glance

| Method | Main idea | Output style | What changes with query? |
|---|---|---|---|
| KNN | Use nearest stored neighbours | Vote / average | Chosen neighbours |
| Weighted KNN | Use neighbours, weight close ones more | Weighted vote / weighted average | Weights and influence |
| LWR | Fit a local weighted regression model around query | Local regression prediction | Local fitted coefficients |
| RBF | Use local basis functions centred at prototypes | Weighted basis-function output | Basis activations |

---

## Advantages and Limitations

### Advantages

- **Conceptually simple**: similar inputs should give similar outputs
- **Flexible**: no single global model shape is forced on the data
- **Works for both classification and regression**
- **Naturally local**: useful when local neighbourhood structure is more meaningful than one global formula
- **Little or no training cost**: training often means only storing the examples

### Limitations

- **Expensive prediction time**: distance must be computed at query time
- **Sensitive to scaling**: unscaled features can dominate distance
- **Sensitive to irrelevant features**: leads to misleading neighbourhoods
- **Sensitive to choice of $k$**: poor hyperparameter choice hurts performance
- **Suffers in high dimensions**: curse of dimensionality is a major issue
- **Memory-heavy**: training examples must be stored

---

## Common Exam Question Patterns

### KNN Classification
- Compute Euclidean distance, sort neighbours, apply majority vote
- Compare answers for $k=3$, $k=5$, $k=7$

### KNN Regression
- Compute nearest neighbours and average their target values

### Best Value of $k$
- Use odd $k$ to avoid ties in binary classification
- Identify elbow point from error plot
- Explain effect of small vs large $k$

### Mixed-data Dissimilarity
- Normalise numeric features, rank ordinal features, compare nominal features, combine partial distances

### Weighted KNN
- Compute distances, compute kernel weights, show why weighted result may differ from unweighted majority vote

### LWR Conceptual Questions
- Explain why LWR produces different local lines for different query points
- Compare global regression with locally weighted regression

### Curse of Dimensionality
- Define it, explain why KNN is misled in high dimensions, suggest remedies

---

## High-value Exam Points

- KNN is a **lazy learner**; model-based learning is an **eager learner**
- KNN solves both **classification** (majority vote) and **regression** (mean or median)
- $k$ is a **hyperparameter**; odd $k$ helps avoid ties in binary classification
- Distance metric choice matters; feature scaling matters
- High dimensionality can mislead nearest-neighbour methods
- Weighted KNN gives more influence to closer neighbours
- LWR builds a **local model around the query**
- RBF also captures **local influence through distance from centres**

---

## Quick Comparison: Global vs Local Thinking

{{< mermaid >}}
flowchart TD
  A[One global regression line] --> B[Ordinary linear regression]
  C[Neighbour vote or average] --> D[KNN]
  E[Weighted neighbours] --> F[Distance-weighted KNN]
  G[Local fitted regression line] --> H[Locally weighted regression]
  I[Local basis centred at prototypes] --> J[Radial Basis Functions]
{{< /mermaid >}}

---

## Final Summary

Instance-based learning is built on one central belief: **a useful prediction can often be made by looking at similar previously observed examples**.

KNN is the simplest and most direct version of this idea. Distance-weighted KNN improves it by giving stronger influence to closer neighbours. Locally weighted regression extends the same idea to local regression fitting. Radial basis functions provide another local-distance-based way to construct non-linear predictors.

Although these topics look different, they are all connected by the same local learning principle:

> **Nearby points should matter more than faraway points.**

---

## References

- [My Notes: Machine Learning Workflow]({{% relref "02-ml-workflow.md" %}})
- [My Notes: Linear Models for Regression]({{% relref "03-linear-models-regression.md" %}})
- [My Notes: Linear Models for Classification]({{% relref "04-linear-models-classification.md" %}})

---

{{< home-link "Home" >}} | {{< section-index >}}
