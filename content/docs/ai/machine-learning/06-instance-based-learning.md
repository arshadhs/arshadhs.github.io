---
title: 'Instance-based Learning'
draft: false
tags: ["AI", "ML"]
categories: ["AI", "ML"]
weight: 600
---

# Instance-based Learning

Instance-based learning is a family of methods that **do not build one explicit global model during training**. Instead, they **store training examples** and delay most of the work until a new query arrives.

When a new point must be classified or predicted, the algorithm compares it with previously seen examples, finds the most relevant neighbours, and uses them to produce the answer.

Instance-based Learning covers three linked ideas:

- $k$-Nearest Neighbour (KNN) learning
- Locally Weighted Regression (LWR)
- Radial Basis Functions (RBF)

{{% hint info %}}
**Key takeaway:** Instance-based learning is a **lazy learning** approach. Training is usually cheap because we mainly store the data. Prediction is more expensive because the algorithm must search for similar records at query time. The most important ideas for exams are: choosing a good distance measure, choosing $k$, understanding weighted neighbours, and knowing why nearby data should matter more than faraway data.
{{% /hint %}}

---

## Model-Based vs Instance-Based

Model-based methods try to learn a compact predictive model during training. Examples include Linear Regression, Logistic Regression, and Decision Trees. Instance-based learning keeps the training data instead, postponing most computation until prediction time.

| Feature | Instance-Based Learning | Model-Based Learning |
| :--- | :--- | :--- |
| **Approach** | Memorises examples | Learns general patterns/rules |
| **Training Time** | Minimal (just stores data) | Can be long (building the model) |
| **Prediction Time** | Slow (searches all data) | Fast (uses a pre-built formula) |
| **Storage** | High (must keep all data) | Low (only stores the model) |
| **Adaptability** | Adapts instantly to new data | Requires retraining to update |

{{< mermaid >}}
graph LR
    IBL[Instance-Based Learning]
    IBL --> Numeric[Numerical / Distance-Based]
    IBL --> Symbolic[Symbolic / Reasoning-Based]
    IBL --> Density[Density / Distribution-Based]
    Numeric --> KNN[k-Nearest Neighbours]
    Numeric --> LWR[Locally Weighted Regression]
    Numeric --> RBF[Radial Basis Functions]
    Numeric --> LVQ[Learning Vector Quantization]
    Symbolic --> CBR[Case-Based Reasoning]
    Density --> KDE[Kernel Density Estimation]
    Density --> PW[Parzen Windows]
    style IBL fill:#F8D7E8,stroke:#7A6170,stroke-width:3px,color:#2F2F2F
    style Numeric fill:#D8EAFE,stroke:#5E7084,color:#2F2F2F
    style Symbolic fill:#DDF5E4,stroke:#637A68,color:#2F2F2F
    style Density fill:#FFF1C7,stroke:#8A7A54,color:#2F2F2F
    style KNN fill:#E6E0FF,stroke:#6F6690,color:#2F2F2F
    style LWR fill:#D7F3F0,stroke:#5B7D7A,color:#2F2F2F
    style RBF fill:#FFE0CC,stroke:#8B6A58,color:#2F2F2F
    style LVQ fill:#EDE7D9,stroke:#7B7062,color:#2F2F2F
    style CBR fill:#E2F0CB,stroke:#687A55,color:#2F2F2F
    style KDE fill:#FFE5EC,stroke:#8A6570,color:#2F2F2F
    style PW fill:#FDE2E4,stroke:#8A6570,color:#2F2F2F
{{< /mermaid >}}

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
  style A fill:#FFF1C7,stroke:#8A7A54,color:#2F2F2F
  style B fill:#F8D7E8,stroke:#7A6170,color:#2F2F2F
  style C fill:#D8EAFE,stroke:#5E7084,color:#2F2F2F
  style D fill:#DDF5E4,stroke:#637A68,color:#2F2F2F
  style E fill:#EAF4FF,stroke:#6B7D90,color:#2F2F2F
  style F fill:#EAF4FF,stroke:#6B7D90,color:#2F2F2F
  style G fill:#EAF4FF,stroke:#6B7D90,color:#2F2F2F
  style H fill:#EAF8EE,stroke:#637A68,color:#2F2F2F
  style I fill:#EAF8EE,stroke:#637A68,color:#2F2F2F
  style J fill:#EAF8EE,stroke:#637A68,color:#2F2F2F
{{< /mermaid >}}

### Eager Learners vs Lazy Learners

**Model-based methods** are often called **eager learners** — they spend more effort during training and less during prediction.

**Instance-based methods** are often called **lazy learners** — they spend little effort during training and more during prediction.

This distinction is one of the most important conceptual points in this module.

---

## KNN

> "If it walks like a duck and quacks like a duck, it is probably a duck."

The machine learning version: if a new point looks similar to known points of a certain class, it is likely to belong to that class. The whole method depends on two questions:

1. How do we define **similarity** or **distance**?
2. How many neighbours should influence the final answer?

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
  style A fill:#DDF5E4,stroke:#637A68,color:#2F2F2F
  style B fill:#FFF1C7,stroke:#8A7A54,color:#2F2F2F
  style C fill:#D8EAFE,stroke:#5E7084,color:#2F2F2F
  style D fill:#E6E0FF,stroke:#6F6690,color:#2F2F2F
  style E fill:#F8D7E8,stroke:#7A6170,color:#2F2F2F
  style F fill:#FFE0CC,stroke:#8B6A58,color:#2F2F2F
  style G fill:#D7F3F0,stroke:#5B7D7A,color:#2F2F2F
  style H fill:#EAF8EE,stroke:#637A68,color:#2F2F2F
  style I fill:#EAF8EE,stroke:#637A68,color:#2F2F2F
{{< /mermaid >}}

1. Store the training set
2. Receive a query point $x_q$
3. Compute the distance between $x_q$ and training points
4. Select one or more nearest neighbours
5. Use those neighbours to infer the output

### Classification and Regression

KNN can be used for both **classification** and **regression**. Given training examples $\langle x_i, f(x_i) \rangle$, for a new query point $x_q$, find the nearest examples and use their outputs to estimate $f^*(x_q)$.

For **1-NN**:

{{% colour "blue" %}}
{{< katex display=true >}}
f^*(x_q)=f(x_n)
{{< /katex >}}
{{% /colour %}}

where $x_n$ is the nearest training example to $x_q$.

**For classification** — use **majority vote** among the $k$ nearest neighbours:

{{% colour "blue" %}}
{{< katex display=true >}}
\hat{f}(x_q)=\arg\max_{v \in V}\sum_{i=1}^{k}\delta\!\big(v,f(x_i)\big)
{{< /katex >}}
{{% /colour %}}

where $V$ is the set of possible class labels and $\delta(a,b)=1$ if $a=b$, otherwise $0$.

**For regression** — use the average (or median) of the $k$ nearest neighbours:

{{% colour "blue" %}}
{{< katex display=true >}}
\hat{f}(x_q)=\frac{1}{k}\sum_{i=1}^{k} f(x_i)
{{< /katex >}}
{{% /colour %}}

Median can be more robust when outliers are present.

### Training and Prediction

**Training phase:** KNN does almost no parameter learning. It stores input features and corresponding outputs, so training is simple and fast.

**Prediction phase:** For each query point the algorithm must compare against many or all training examples, compute distances, sort to identify nearest points, and then vote or average. This is why KNN is called a lazy learner.

### Step-by-Step Procedure

**For classification:**

1. Take the query point $x_q$
2. Compute distance from $x_q$ to every training point
3. Sort distances from smallest to largest
4. Select the first $k$ neighbours
5. Count the class labels among them
6. Return the class with the highest count

**For regression:**

1. Take the query point $x_q$
2. Compute distance to all training points
3. Select the first $k$ neighbours
4. Average their target values
5. Return the average as the prediction

### Choosing k

The value of $k$ is the key hyperparameter in KNN.

- If $k$ is too small, the model becomes too sensitive to noise
- If $k$ is too large, the model becomes too smooth and may ignore local structure

In binary classification, using an **odd** value of $k$ is recommended to avoid ties in voting.

**Rule of thumb:**

{{% colour "blue" %}}
{{< katex display=true >}}
k=\sqrt{N}
{{< /katex >}}
{{% /colour %}}

where $N$ is the number of training points. This is a starting point only, not a guaranteed best choice.

**Elbow method:** Try several values of $k$, compute an error function (e.g., SSE), plot error against $k$, and choose the point after which improvement becomes much smaller. A common example identifies $k=3$ as the elbow point.

| $k$ size | Behaviour |
|----------|-----------|
| Small | High variance, can overfit, highly local |
| Large | Higher bias, smoother boundary, less sensitive to noise |

### Distance and Similarity

KNN does not learn a separating equation in advance, so prediction quality depends heavily on the distance or similarity function chosen.

A data matrix has $n$ data points each with $p$ attributes. The corresponding distance matrix stores pairwise distances between points — it is symmetric and often shown in triangular form.

**Properties of a valid metric** for $d(i,j)$:

- $d(i,j) > 0$ if $i \neq j$ (positive definiteness)
- $d(i,i)=0$ (identity)
- $d(i,j)=d(j,i)$ (symmetry)
- $d(i,j) \le d(i,k)+d(k,j)$ (triangle inequality)

**Minkowski distance** (general family):

{{% colour "blue" %}}
{{< katex display=true >}}
d(i,j)=\left(\sum_{f=1}^{p}|x_{if}-x_{jf}|^h\right)^{1/h}
{{< /katex >}}
{{% /colour %}}

where $h$ is the order of the norm.

**Manhattan ($L_1$)** — useful when absolute coordinate-wise difference matters:

{{% colour "blue" %}}
{{< katex display=true >}}
d(i,j)=\sum_{f=1}^{p}|x_{if}-x_{jf}|
{{< /katex >}}
{{% /colour %}}

**Euclidean ($L_2$)** — the most common geometric distance:

{{% colour "blue" %}}
{{< katex display=true >}}
d(i,j)=\sqrt{\sum_{f=1}^{p}(x_{if}-x_{jf})^2}
{{< /katex >}}
{{% /colour %}}

**Supremum ($L_\infty$)** — controlled by the largest coordinate difference:

{{% colour "blue" %}}
{{< katex display=true >}}
d(i,j)=\max_f |x_{if}-x_{jf}|
{{< /katex >}}
{{% /colour %}}

### Similarity for Different Attribute Types

{{< mermaid >}}
flowchart TD
    A["Attribute Similarity"] --> B["Nominal Attributes"]
    A --> C["Binary Attributes"]

    C --> D["Symmetric Binary"]
    C --> E["Asymmetric Binary"]

    D --> D1["Dissimilarity"]

    E --> E1["Dissimilarity"]
    E --> E2["Jaccard Similarity"]

    classDef root fill:#dbeafe,stroke:#5b7db1,color:#222;
    classDef nominal fill:#d8f3dc,stroke:#5f8f6a,color:#222;
    classDef binary fill:#fff3bf,stroke:#b59b3b,color:#222;
    classDef sym fill:#e1f5fe,stroke:#5b7db1,color:#222;
    classDef asym fill:#e9d8fd,stroke:#8a6fb3,color:#222;

    class A root;
    class B,B1,B2 nominal;
    class C binary;
    class D,D1 sym;
    class E,E1,E2 asym;
{{< /mermaid >}}

**Nominal attributes** (no natural order, e.g., profession, colour, mother tongue):

- Simple matching: matching values contribute similarity; differing values contribute dissimilarity
- Binary indicator encoding: one binary feature per possible nominal state (similar to one-hot encoding)

**Binary attributes:**

When comparing two objects `i` and `j`, binary attributes can be counted using the following table:

| | Object j = 1 | Object j = 0 |
|---|---:|---:|
| **Object i = 1** | q | r |
| **Object i = 0** | s | t |

Where:

- `q` = number of attributes where both objects are `1`
- `r` = number of attributes where object `i` is `1` and object `j` is `0`
- `s` = number of attributes where object `i` is `0` and object `j` is `1`
- `t` = number of attributes where both objects are `0`

- Symmetric binary: both states are equally important (e.g., gender)

{{% colour "blue" %}}
{{< katex display=true >}}
d_{\text{symmetric}}(i,j) = \frac{r+s}{q+r+s+t}
{{< /katex >}}
{{% /colour %}}

- Asymmetric binary: one state is more important, common in presence/absence situations (e.g., symptoms, test results)

{{% colour "blue" %}}
{{< katex display=true >}}
d_{\text{asymmetric}}(i,j) = \frac{r+s}{q+r+s}
{{< /katex >}}
{{% /colour %}}

- Jaccard coefficient (similarity measure for asymmetric binary variables):

{{% colour "blue" %}}
{{< katex display=true >}}
\mathrm{sim}_{\mathrm{Jaccard}}(i,j)=\frac{q}{q+r+s}
{{< /katex >}}
{{% /colour %}}

where $q$ counts shared positives.

**Ordinal attributes** (ordered values, e.g., Village → Small Town → Suburban → Metropolitan):

1. Replace values by ranks
2. Map ranks to $[0,1]$
3. Treat them like interval-scaled variables

**Mixed data types — Gower distance:** Handles a mix of nominal, binary, numeric, and ordinal attributes by computing component-wise dissimilarity per attribute type, normalising appropriately, and combining with equal or chosen weights.

Typical workflow for mixed data:

1. Normalise numeric attributes
2. Rank and scale ordinal attributes
3. Compare nominal attributes by match/mismatch
4. Combine the partial distances

### Feature Scaling

KNN is sensitive to scale. If one numeric feature has a much larger range than another, it can dominate the distance computation. Without scaling, high-range features dominate, the neighbourhood becomes misleading, and prediction quality drops.

**Z-score standardisation:**

{{% colour "blue" %}}
{{< katex display=true >}}
z=\frac{x-\mu}{\sigma}
{{< /katex >}}
{{% /colour %}}

where $x$ is the raw value, $\mu$ is the mean, and $\sigma$ is the standard deviation. Min-max normalisation is also used in some exam questions.

### Curse of Dimensionality

If many features are used but only a few are truly relevant, two genuinely similar records may appear far apart because distance is computed across many irrelevant dimensions. Traditional KNN uses **all attributes** unless you intervene, so irrelevant attributes distort the geometry of the space.

Remedies:

- **Remove less relevant attributes** — validate performance after removal
- **Weight attributes differently** — give lower weight to less relevant features; conceptually this shortens less relevant axes and lengthens more relevant ones

### Challenges of KNN

| Challenge | Description |
|-----------|-------------|
| Choosing $k$ | A hyperparameter; bad choice causes overfitting or over-smoothing |
| Choosing the distance metric | Results differ across Euclidean, Manhattan, mixed-type, and weighted distance |
| Query-time cost | Each prediction may require comparison against all training points |
| Noise and irrelevant features | Can mislead the nearest-neighbour search |

### Distance-Weighted KNN

Plain KNN may fail when a query point lies near a few very close examples of one class, but the majority of slightly farther neighbours belong to another class. A plain majority vote may predict incorrectly.

Distance-weighted KNN fixes this by assigning **larger influence to closer points** using a kernel weight function:

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

**Weighted classification rule:**

{{% colour "blue" %}}
{{< katex display=true >}}
\hat{f}(x_q)=\arg\max_{v \in V}\sum_{i=1}^{k} w_i\,\delta\!\big(v,f(x_i)\big)
{{< /katex >}}
{{% /colour %}}

**Weighted regression rule:**

{{% colour "blue" %}}
{{< katex display=true >}}
\hat{f}(x_q)=\frac{\sum_{i=1}^{k} w_i f(x_i)}{\sum_{i=1}^{k} w_i}
{{< /katex >}}
{{% /colour %}}

A larger distance means smaller weight; a smaller distance means larger weight. Similarity increases the influence of that training instance on the final prediction.

---

## Locally Weighted Regression

LWR is the regression counterpart of the "nearby points should matter more" idea, and is one of the key syllabus items in this module.

- **Global linear regression** fits **one line** for all data points
- **Locally weighted regression** fits a **small weighted local model around the query point**

If the query changes, its local fitted line may also change.

| Ordinary Linear Regression | Locally Weighted Regression |
|---------------------------|------------------------------|
| One global hypothesis | Local model per query point |
| Shared slope and intercept | Coefficients depend on the query |
| Same line used for all predictions | Different local line for different queries |

### Workflow

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

where $w_i(x_q)$ is the weight of training point $x_i$ relative to $x_q$, and $h_w(x_i)$ is the local regression prediction. The final query prediction is:

{{% colour "blue" %}}
{{< katex display=true >}}
\hat{y}_q = h_{w^{(q)}}(x_q)
{{< /katex >}}
{{% /colour %}}

The effective parameters $w^{(q)}$ depend on the query point, so the fitted local model changes from query to query.

### Two Design Viewpoints

**View 1 — Local approximation:** Use only nearby neighbours, then fit a local regression on that neighbourhood.

**View 2 — Weighted global approximation:** Use all data, but multiply each error term by a weight that decreases with distance from the query. Far points contribute very little, giving the benefit of the full dataset while still focusing on the neighbourhood.

### Kernel Choice and Bandwidth

The kernel is not one formula for all datasets — experimentation is needed. Possible choices include inverse distance, inverse squared distance, exponential/Gaussian weighting, and quadratic kernel variants.

**Bandwidth** guides locality:

- **Small bandwidth** — very local behaviour, strong emphasis on nearby points
- **Large bandwidth** — smoother behaviour, broader neighbourhood influence

### When LWR Is Useful

- One single global linear trend is too crude
- The relationship varies across the input space
- Nearby points are more informative than distant ones

---

## Radial Basis Functions (RBF)

An RBF is a function whose value depends mainly on the **distance from a centre**. A common Gaussian RBF:

{{% colour "blue" %}}
{{< katex display=true >}}
\phi_j(x)=\exp\left(-\frac{\|x-c_j\|^2}{2\sigma^2}\right)
{{< /katex >}}
{{% /colour %}}

where $c_j$ is the centre and $\sigma$ controls the spread.

- If $x$ is close to $c_j$, then $\phi_j(x)$ is large
- If $x$ is far from $c_j$, then $\phi_j(x)$ becomes small

### Model Form

{{% colour "blue" %}}
{{< katex display=true >}}
\hat{y}(x)=w_0+\sum_{j=1}^{M} w_j\,\phi_j(x)
{{< /katex >}}
{{% /colour %}}

This is linear in the weights $w_j$ but non-linear in the original input $x$.

### Connection to the Module Theme

RBF shares the same local-neighbourhood philosophy as KNN and LWR:

- **KNN** — use nearby examples more
- **LWR** — fit a local weighted model around the query
- **RBF** — build features that are naturally strongest near selected centres and weaker farther away

---

## Comparison - KNN, LWR, and RBF

{{< mermaid >}}
flowchart TD
  A[One global regression line] --> B[Ordinary linear regression]
  C[Neighbour vote or average] --> D[KNN]
  E[Weighted neighbours] --> F[Distance-weighted KNN]
  G[Local fitted regression line] --> H[Locally weighted regression]
  I[Local basis centred at prototypes] --> J[Radial Basis Functions]
  style A fill:#D8EAFE,stroke:#5E7084,color:#2F2F2F
  style B fill:#EAF4FF,stroke:#6B7D90,color:#2F2F2F
  style C fill:#DDF5E4,stroke:#637A68,color:#2F2F2F
  style D fill:#EAF8EE,stroke:#637A68,color:#2F2F2F
  style E fill:#FFF1C7,stroke:#8A7A54,color:#2F2F2F
  style F fill:#FFF7DC,stroke:#8A7A54,color:#2F2F2F
  style G fill:#F8D7E8,stroke:#7A6170,color:#2F2F2F
  style H fill:#FDEAF3,stroke:#7A6170,color:#2F2F2F
  style I fill:#FFE0CC,stroke:#8B6A58,color:#2F2F2F
  style J fill:#FFF0E6,stroke:#8B6A58,color:#2F2F2F
{{< /mermaid >}}


| Method | Main idea | Output style | What changes with query? |
|---|---|---|---|
| KNN | Use nearest stored neighbours | Vote / average | Chosen neighbours |
| Weighted KNN | Use neighbours, weight close ones more | Weighted vote / weighted average | Weights and influence |
| LWR | Fit a local weighted regression model around query | Local regression prediction | Local fitted coefficients |
| RBF | Use local basis functions centred at prototypes | Weighted basis-function output | Basis activations |

### Weighted KNN vs LWR

| | Distance-weighted KNN | LWR |
|---|---|---|
| Local regression fit | No | Yes |
| Uses neighbour outputs directly | Yes | No |
| Decision rule | Weighted vote or weighted average | Value from local fitted model |

LWR is a more structured local regression version of the "nearby points matter more" principle.

### Advantages and Limitations

**Advantages:**

- **Conceptually simple** — similar inputs should give similar outputs
- **Flexible** — no single global model shape is forced on the data
- **Works for both classification and regression**
- **Naturally local** — useful when local neighbourhood structure is more meaningful than one global formula
- **Little or no training cost** — training often means only storing the examples

**Limitations:**

- **Expensive prediction time** — distance must be computed at query time
- **Sensitive to scaling** — unscaled features can dominate distance
- **Sensitive to irrelevant features** — leads to misleading neighbourhoods
- **Sensitive to choice of $k$** — poor hyperparameter choice hurts performance
- **Suffers in high dimensions** — curse of dimensionality is a major issue
- **Memory-heavy** — training examples must be stored

---

## Notes

### Key Facts to Remember

- KNN is a **lazy learner**; model-based learning is an **eager learner**
- KNN solves both **classification** (majority vote) and **regression** (mean or median)
- $k$ is a **hyperparameter**; odd $k$ helps avoid ties in binary classification
- Distance metric choice matters; feature scaling matters
- High dimensionality can mislead nearest-neighbour methods
- Weighted KNN gives more influence to closer neighbours
- LWR builds a **local model around the query**
- RBF captures **local influence through distance from centres**

### Numerical Template: KNN

1. Write the query point
2. Compute distance from the query to every training point
3. Sort distances in ascending order
4. Take the first $k$ rows
5. Apply the decision rule:
   - **Classification** — count labels and apply majority vote
   - **Regression** — average the target values of the selected neighbours
6. State the conclusion: *"Hence the predicted class is ..."* or *"Hence the predicted value is ..."*

### Numerical Template: Weighted KNN

1. Prepare the data — normalise numeric variables, rank ordinal variables, handle nominal attributes using match/mismatch distance
2. Compute the distance from query $x_q$ to each record
3. Apply the kernel function to each distance to compute the weight
4. Sum total weight class-wise
5. Choose the class with the largest total weight
6. Explain how plain KNN may prefer the majority class while weighted KNN may prefer the more similar class

### Numerical Template: LWR

1. Identify the query point $x_q$
2. Compute distance from each training point to $x_q$
3. Apply the chosen kernel to obtain weights
4. Form the weighted local loss
5. Fit the local coefficients or interpret the local fitted curve
6. Use the resulting local model to compute $\hat{y}_q$
7. Explain why the answer differs from a global regression line

### Quick Revision Checklist ☆

1. **KNN Classification** — compute Euclidean distance, sort neighbours, apply majority vote; compare answers for $k=3$, $k=5$, $k=7$
2. **KNN Regression** — compute nearest neighbours and average their target values
3. **Best value of $k$** — use odd $k$ for binary classification; identify elbow point from error plot; explain effect of small vs large $k$
4. **Mixed-data dissimilarity** — normalise numeric features, rank ordinal features, compare nominal features, combine partial distances
5. **Weighted KNN** — compute distances, compute kernel weights, show why weighted result may differ from unweighted majority vote
6. **LWR conceptual** — explain why LWR produces different local lines for different query points; compare global regression with LWR
7. **Curse of dimensionality** — define it, explain why KNN is misled in high dimensions, suggest remedies

---

## Summary

Instance-based learning is built on one central belief: **a useful prediction can often be made by looking at similar previously observed examples**.

KNN is the simplest and most direct version of this idea. Distance-weighted KNN improves it by giving stronger influence to closer neighbours. Locally weighted regression extends the same idea to local regression fitting. Radial basis functions provide another local-distance-based way to construct non-linear predictors.

Although these methods look different, they are all connected by the same local learning principle:

> **Nearby points should matter more than faraway points.**

---

## References

- [My Notes: Machine Learning Workflow]({{% relref "02-ml-workflow.md" %}})
- [My Notes: Linear Models for Regression]({{% relref "03-linear-models-regression.md" %}})
- [My Notes: Linear Models for Classification]({{% relref "04-linear-models-classification.md" %}})

---

{{< home-link "Home" >}} | {{< section-index >}}
