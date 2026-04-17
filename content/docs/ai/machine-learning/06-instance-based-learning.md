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

This module in your course covers three linked ideas:

- $k$-Nearest Neighbour (KNN) learning
- Locally Weighted Regression (LWR)
- Radial Basis Functions (RBF)

{{% hint info %}}
Key takeaway:
Instance-based learning is a **lazy learning** approach.
Training is usually cheap because we mainly store the data.
Prediction is more expensive because the algorithm must search for similar records at query time.
The most important ideas for exams are:
choosing a good distance measure,
choosing $k$,
understanding weighted neighbours,
and knowing why nearby data should matter more than faraway data.
{{% /hint %}}

---

## Where this topic fits

In your course handout, **Instance-based Learning** is a full module and explicitly includes:

- $k$-Nearest Neighbour Learning
- Locally Weighted Regression (LWR) Learning
- Radial Basis Functions

So this is not a side topic.

It is part of the core syllabus and is useful for both understanding and exams.

---

## Model-based vs Instance-based learning

Before this module, most of your course topics were **model-based**.

Examples:

- Linear Regression
- Logistic Regression
- Decision Trees

These methods try to learn a compact predictive model during training.

Instance-based learning behaves differently.

It keeps the training data and postpones most of the computation until prediction time.

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

### Eager learners vs lazy learners

- **Model-based methods** are often called **eager learners**
- **Instance-based methods** are often called **lazy learners**

Why?

- Eager learner:
  spends more effort during training,
  less effort during prediction
- Lazy learner:
  spends little effort during training,
  more effort during prediction

This distinction is one of the most important conceptual points in this module.

---

## Core intuition

A simple intuition from the slides is:

“If it walks like a duck and quacks like a duck, it is probably a duck.”

The machine learning version is:

“If a new point looks similar to known points of a certain class, it is likely to belong to that class.”

So the whole method depends on two questions:

1. How do we define **similarity** or **distance**?
2. How many neighbours should influence the final answer?

---

## General workflow of instance-based learning

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

## k-Nearest Neighbour Learning

KNN is the most commonly discussed instance-based learning algorithm.

It can be used for:

- **classification**
- **regression**

### Basic notation

Suppose the training set contains examples $\langle x_i, f(x_i) \rangle$.

For a new query point $x_q$:

- find the nearest examples to $x_q$
- use their outputs to estimate $f^*(x_q)$

For **1-NN**:

{{% colour "blue" %}}
{{< katex display=true >}}
f^*(x_q)=f(x_n)
{{< /katex >}}
{{% /colour %}}

where $x_n$ is the nearest training example to $x_q$.

### KNN for classification

For a discrete target,
use **majority vote** among the $k$ nearest neighbours.

A compact mathematical form is:

{{% colour "blue" %}}
{{< katex display=true >}}
\hat{f}(x_q)=\arg\max_{v \in V}\sum_{i=1}^{k}\delta\!ig(v,f(x_i)\big)
{{< /katex >}}
{{% /colour %}}

where:

- $V$ is the set of possible class labels
- $\delta(a,b)=1$ if $a=b$, otherwise $0$

### KNN for regression

For a real-valued target,
use the average (or sometimes median) of the outputs of the $k$ nearest neighbours.

{{% colour "blue" %}}
{{< katex display=true >}}
\hat{f}(x_q)=\frac{1}{k}\sum_{i=1}^{k} f(x_i)
{{< /katex >}}
{{% /colour %}}

Median can be more robust when outliers are present.

---

## Training and prediction in KNN

### Training phase

KNN does almost no parameter learning.

It mainly:

- stores input features
- stores corresponding outputs

So training is simple and fast.

### Prediction phase

Prediction is costlier because for each query point the algorithm must:

- compare the query with many or all training examples
- compute distances
- sort or identify nearest points
- then vote or average

This is why KNN is called a lazy learner.

---

## Step-by-step KNN procedure

### For classification

1. Take the query point $x_q$
2. Compute distance from $x_q$ to every training point
3. Sort distances from smallest to largest
4. Select the first $k$ neighbours
5. Count the class labels among them
6. Return the class with the highest count

### For regression

1. Take the query point $x_q$
2. Compute distance to all training points
3. Select the first $k$ neighbours
4. Average their target values
5. Return the average as the prediction

---

## Choosing the value of $k$

The value of $k$ is the key hyperparameter in KNN.

### Why $k$ matters

- If $k$ is too small,
  the model becomes too sensitive to noise
- If $k$ is too large,
  the model becomes too smooth and may ignore local structure

### Practical observations from the lesson

- In classification,
  using an **odd** value of $k$ is often recommended for binary classification,
  because it helps avoid ties in voting
- The transcript explicitly discusses that if you use an even $k$,
  you may end up with equal votes for two classes

### Rule of thumb

The slides give a rough rule:

{{% colour "blue" %}}
{{< katex display=true >}}
k=\sqrt{N}
{{< /katex >}}
{{% /colour %}}

where $N$ is the number of training points.

This is only a starting point,
not a guaranteed best choice.

### Elbow method for selecting $k$

The lesson also discusses an elbow-style tuning idea:

1. Try several values of $k$
2. Compute an error function such as SSE
3. Plot error against $k$
4. Choose the point after which improvement becomes much smaller

This point is called the **elbow**.

The transcript repeatedly uses an example where the best visual choice is $k=3$.

### Interpretation of small vs large $k$

- Small $k$:
  can overfit,
  highly local,
  high variance
- Large $k$:
  smoother boundary,
  less sensitive to noise,
  but higher bias

---

## Numerical template for KNN classification problems

This is a very common exam pattern.

### Step 1
Write the query point.

### Step 2
Compute distance from the query to every training point.

### Step 3
Sort the distances in ascending order.

### Step 4
Take the first $k$ rows.

### Step 5
For classification:
count labels and apply majority vote.

### Step 6
For regression:
average the target values of the selected neighbours.

### Step 7
State the conclusion clearly:

- “Hence the predicted class is ...”
- or
- “Hence the predicted value is ...”

---

## Why distance matters so much

KNN does not learn a separating equation in advance.

So the quality of its prediction depends heavily on the quality of the distance or similarity function.

If the distance is badly chosen,
then the “nearest” neighbours may not be the most meaningful ones.

That is why the lesson spends time on **measures of dissimilarity**.

---

## Data matrix vs distance matrix

The slides distinguish two related objects.

### Data matrix

- $n$ data points
- each with $p$ attributes

### Distance or dissimilarity matrix

- stores pairwise distances between points
- usually symmetric
- often shown in triangular form because distances repeat across the diagonal

---

## Measures of distance and similarity

### A metric should satisfy

For a valid distance function $d(i,j)$:

- $d(i,j) > 0$ if $i \neq j$
- $d(i,i)=0$
- $d(i,j)=d(j,i)$
- $d(i,j) \le d(i,k)+d(k,j)$

These are:

- positive definiteness
- identity
- symmetry
- triangle inequality

---

## Minkowski distance

A general distance family used in the lesson is the **Minkowski distance**.

{{% colour "blue" %}}
{{< katex display=true >}}
d(i,j)=\left(\sum_{f=1}^{p}|x_{if}-x_{jf}|^h\right)^{1/h}
{{< /katex >}}
{{% /colour %}}

where:

- $i=(x_{i1},x_{i2},\dots,x_{ip})$
- $j=(x_{j1},x_{j2},\dots,x_{jp})$
- $h$ is the order of the norm

### Important special cases

#### Manhattan distance ($L_1$)

{{% colour "blue" %}}
{{< katex display=true >}}
d(i,j)=\sum_{f=1}^{p}|x_{if}-x_{jf}|
{{< /katex >}}
{{% /colour %}}

#### Euclidean distance ($L_2$)

{{% colour "blue" %}}
{{< katex display=true >}}
d(i,j)=\sqrt{\sum_{f=1}^{p}(x_{if}-x_{jf})^2}
{{< /katex >}}
{{% /colour %}}

#### Supremum distance ($L_\infty$)

{{% colour "blue" %}}
{{< katex display=true >}}
d(i,j)=\max_f |x_{if}-x_{jf}|
{{< /katex >}}
{{% /colour %}}

### When to use which?

- Manhattan:
  useful when absolute coordinate-wise difference matters
- Euclidean:
  the most common geometric distance
- Supremum:
  controlled by the largest coordinate difference

---

## Similarity for nominal attributes

Nominal attributes have no natural order.

Examples:

- profession
- colour
- mother tongue

### Method 1: simple matching

If two nominal values match,
that contributes similarity.

If they differ,
that contributes dissimilarity.

### Method 2: convert to binary indicators

Create one binary feature for each possible nominal state.

This is conceptually similar to one-hot encoding.

---

## Similarity for binary attributes

The slides distinguish:

- symmetric binary attributes
- asymmetric binary attributes

### Symmetric binary attributes

Both states are equally important.

Example:

- gender in the slide example

### Asymmetric binary attributes

One state is more important than the other.

This is common in “presence vs absence” situations,
like symptoms or test results.

### Jaccard similarity

For asymmetric binary variables,
Jaccard is useful.

{{% colour "blue" %}}
{{< katex display=true >}}
\mathrm{sim}_{\mathrm{Jaccard}}(i,j)=\frac{q}{q+r+s}
{{< /katex >}}
{{% /colour %}}

where $q$ counts shared positives.

A related dissimilarity is:

{{% colour "blue" %}}
{{< katex display=true >}}
d(i,j)=\frac{r+s}{q+r+s}
{{< /katex >}}
{{% /colour %}}

---

## Ordinal attributes

Ordinal values have order.

Examples:

- Village
- Small Town
- Suburban
- Metropolitan

The lesson suggests:

1. replace values by ranks
2. map them to $[0,1]$
3. then treat them like interval-scaled variables

This is very useful in mixed-type KNN numerical problems.

---

## Mixed data types and Gower-style distance

Real datasets often contain a mixture of:

- nominal attributes
- binary attributes
- numeric attributes
- ordinal attributes

The slides mention **Gower distance** for mixed types.

The basic idea is:

- compute component-wise dissimilarity for each attribute type
- normalise appropriately
- combine them with equal or chosen weights

In mixed-data exam problems,
a typical workflow is:

1. normalise numeric attributes
2. rank and scale ordinal attributes
3. compare nominal attributes by match / mismatch
4. combine the partial distances

---

## Standardising numeric data

KNN is sensitive to scale.

If one numeric feature has much larger range than another,
it can dominate the distance.

That is why the slides explicitly discuss **Z-score standardisation**.

{{% colour "blue" %}}
{{< katex display=true >}}
z=\frac{x-\mu}{\sigma}
{{< /katex >}}
{{% /colour %}}

where:

- $x$ is the raw value
- $\mu$ is the mean
- $\sigma$ is the standard deviation

This expresses the value in standard deviation units.

In some exam questions,
min-max normalisation is also used instead.

---

## Why feature scaling is essential in KNN

Because KNN uses distances directly,
feature scaling is usually not optional.

Without scaling:

- high-range numeric features dominate
- the neighbourhood becomes misleading
- prediction quality drops

This is one of the most common practical mistakes in KNN.

---

## Curse of dimensionality

One of the key limitations discussed in both the slides and transcript is the **curse of dimensionality**.

### Main idea

If you use many features,
and only a few of them are truly relevant,
then two genuinely similar records may appear far apart because distance is being computed across many irrelevant dimensions.

So the nearest neighbour search becomes misleading.

### Why it happens

Traditional KNN uses **all attributes** unless you intervene.

If irrelevant attributes are included,
they distort the geometry of the space.

### Remedies from the lesson

#### 1. Remove less relevant attributes

If exploration suggests some features are irrelevant,
remove them and validate performance again.

#### 2. Weight attributes differently

Instead of treating all features equally,
give lower weight to less relevant features and higher weight to more relevant ones.

The transcript explains this visually as:

- shortening less relevant axes
- lengthening more relevant axes

### Exam wording to remember

- high-dimensional instance spaces can mislead nearest neighbour methods
- KNN works best when relevant similarity is well captured by the chosen distance function

---

## Challenges of KNN

The transcript highlights several practical issues.

### 1. Choosing the value of $k$

This is a hyperparameter.

Bad choice of $k$ can cause either overfitting or over-smoothing.

### 2. Choosing the distance metric

The result can differ depending on whether you use:

- Euclidean
- Manhattan
- mixed-type dissimilarity
- weighted distance

### 3. Query-time cost

Prediction can be expensive because for each query the algorithm may need to compare against many or all training points.

### 4. Noise and irrelevant features

Noisy or irrelevant features can mislead the nearest-neighbour search.

---

## Distance-weighted KNN

A major extension discussed in the lesson is **distance-weighted KNN**,
also called **locally weighted KNN** in the transcript.

### Why plain KNN may fail

Suppose a query point lies near a few very close positive points,
but a majority of slightly farther neighbours are negative.

A plain majority vote may incorrectly predict the negative class.

Distance-weighted KNN fixes this by assigning **larger influence to closer points**.

### Core idea

- near neighbours get high weight
- far neighbours get low weight

So it is not just “how many neighbours belong to each class?”

It becomes:

“How much weighted evidence does each class receive?”

### Weight function via a kernel

The slides and transcript describe a kernel-based weight:

{{% colour "blue" %}}
{{< katex display=true >}}
w_i = K\big(d(x_q,x_i)\big)
{{< /katex >}}
{{% /colour %}}

Some example kernels discussed are:

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

### Weighted classification rule

For classification,
instead of plain vote,
add the weights for each class and choose the class with the highest total weight.

A general form is:

{{% colour "blue" %}}
{{< katex display=true >}}
\hat{f}(x_q)=\arg\max_{v \in V}\sum_{i=1}^{k} w_i\,\delta\!\big(v,f(x_i)\big)
{{< /katex >}}
{{% /colour %}}

### Weighted regression rule

For regression,
use weighted averaging.

{{% colour "blue" %}}
{{< katex display=true >}}
\hat{f}(x_q)=\frac{\sum_{i=1}^{k} w_i f(x_i)}{\sum_{i=1}^{k} w_i}
{{< /katex >}}
{{% /colour %}}

### Important interpretation

If the distance is larger,
weight becomes smaller.

If the distance is smaller,
weight becomes larger.

So **similarity increases the influence** of that training instance on the final prediction.

---

## Numerical template for weighted KNN

This is an important exam style question.

### Step 1
Prepare the data.

- normalise numeric variables if required
- rank ordinal variables if required
- handle nominal attributes using match / mismatch distance

### Step 2
Compute the distance from query $x_q$ to each record.

### Step 3
Apply the kernel function to each distance and compute the weight.

### Step 4
For classification:
add total weight class-wise.

### Step 5
Choose the class with the largest total weight.

### Step 6
Explain the conclusion:

- plain KNN may prefer the majority class
- weighted KNN may prefer the more similar class

This contrast is explicitly illustrated in the lesson’s customer-category example.

---

## Locally Weighted Regression (LWR)

LWR is the regression counterpart of the “nearby points should matter more” idea.

It is one of the key syllabus items in this module.

### Intuition

Global linear regression fits **one line** for all data points.

Locally weighted regression instead fits a **small weighted local model around the query point**.

So:

- global linear regression → one line for the entire dataset
- locally weighted regression → a different local line for different query points

This distinction is stressed clearly in the transcript.

### Meaning of “locally”

The approximation is built using the region around the query point.

### Meaning of “weighted”

Nearby samples receive more importance than distant samples.

### Meaning of “regression”

The target is real-valued,
so the output is continuous.

---

## How LWR differs from ordinary linear regression

### Ordinary linear regression

- one global hypothesis
- one shared slope and intercept
- same line used for all prediction points

### Locally weighted regression

- solve a regression problem **for a given query point**
- coefficients depend on the query
- closer points influence the local fit more strongly

So if the query changes,
its local fitted line may also change.

That is why the transcript says:

- one global line in ordinary linear regression
- many possible local lines in LWR

---

## LWR workflow

1. Take query point $x_q$
2. Compute distance from every training point to $x_q$
3. Convert those distances to weights using a kernel
4. Fit a weighted local regression model
5. Evaluate the fitted model at $x_q$
6. Return the local prediction

---

## LWR as a weighted least-squares idea

A standard and useful mathematical form is:

{{% colour "blue" %}}
{{< katex display=true >}}
J_q(w)=\frac{1}{2}\sum_{i=1}^{n} w_i(x_q)\big(h_w(x_i)-y_i\big)^2
{{< /katex >}}
{{% /colour %}}

where:

- $x_q$ is the query point
- $w_i(x_q)$ is the weight of training point $x_i$ relative to $x_q$
- $h_w(x_i)$ is the local regression prediction

The final query prediction is then:

{{% colour "blue" %}}
{{< katex display=true >}}
\hat{y}_q = h_{w^{(q)}}(x_q)
{{< /katex >}}
{{% /colour %}}

The key point is that the effective parameters $w^{(q)}$ depend on the query point.

So the fitted local model changes from query to query.

---

## Two ways to think about LWR

The transcript suggests two design viewpoints.

### View 1: local approximation

Use only nearby neighbours,
then fit a local regression on that neighbourhood.

### View 2: weighted global approximation

Use all data,
but multiply each error term by a weight that decreases with distance from the query.

In practice,
far points then contribute very little.

This gives you the benefit of using the full dataset while still focusing on the neighbourhood of the query.

---

## Kernel choice in LWR

The transcript emphasises that the kernel is not “one formula for all datasets”.

You may need experimentation.

Possible choices include:

- inverse distance style weighting
- inverse squared distance
- exponential / Gaussian style weighting
- quadratic kernel variants in some examples

The lesson also mentions that **bandwidth** is an important parameter for locally weighted regression,
and that bandwidth helps guide kernel selection and locality.

### Interpretation of bandwidth

- small bandwidth:
  very local behaviour,
  strong emphasis on nearby points
- large bandwidth:
  smoother behaviour,
  broader neighbourhood influence

---

## When LWR is useful

LWR is useful when:

- one single global linear trend is too crude
- the relationship varies across the input space
- nearby points are more informative than distant ones

It is especially helpful when the function is locally smooth but not globally linear.

---

## Simple exam explanation of LWR

If asked conceptually,
you can write:

“Locally weighted regression predicts a query by fitting a small regression model around the query point, with higher weights for nearby training examples and lower weights for faraway ones.”

That one line captures the essence very well.

---

## Numerical template for LWR questions

### Step 1
Identify the query point $x_q$.

### Step 2
Compute distance from each training point to $x_q$.

### Step 3
Apply the chosen kernel to obtain weights.

### Step 4
Form the weighted local loss.

### Step 5
Fit the local coefficients or interpret the local fitted curve.

### Step 6
Use the resulting local model to compute $\hat{y}_q$.

### Step 7
Explain why the answer is different from a global regression line.

---

## Relationship between weighted KNN and LWR

These two ideas are very close.

### Distance-weighted KNN

- no explicit local regression line is fit
- uses neighbour outputs directly
- weighted vote or weighted average

### LWR

- fits a local regression model around the query
- uses weighted errors in a regression objective
- returns the value from that local model

So LWR is a more structured local regression version of the “nearby points matter more” principle.

---

## Radial Basis Functions (RBF)

RBF is listed explicitly in the course handout,
so it should be part of your notes even if the visible lesson material gives more emphasis to KNN and LWR.

### Core idea

An RBF is a function whose value depends mainly on the **distance from a centre**.

A common Gaussian RBF is:

{{% colour "blue" %}}
{{< katex display=true >}}
\phi_j(x)=\exp\left(-\frac{\|x-c_j\|^2}{2\sigma^2}\right)
{{< /katex >}}
{{% /colour %}}

where:

- $c_j$ is the centre of the basis function
- $\sigma$ controls the spread or width

### Interpretation

- if $x$ is close to $c_j$,
  then $\phi_j(x)$ is large
- if $x$ is far from $c_j$,
  then $\phi_j(x)$ becomes small

So RBF is another way of encoding **local influence**.

### RBF model form

A common RBF model is:

{{% colour "blue" %}}
{{< katex display=true >}}
\hat{y}(x)=w_0+\sum_{j=1}^{M} w_j\,\phi_j(x)
{{< /katex >}}
{{% /colour %}}

This is still linear in the weights $w_j$,
but non-linear in the original input $x$.

### Why RBF belongs in this module

Because it shares the same local-neighbourhood philosophy:

- nearer centres influence the prediction more
- farther centres influence it less

This is conceptually close to:

- distance-weighted KNN
- Gaussian kernels in LWR

### Connection to locality

If KNN says,
“use nearby examples more”,
and LWR says,
“fit a local weighted model around the query”,
then RBF says,
“build features that are naturally strongest near selected centres and weaker farther away”.

---

## KNN, LWR, and RBF at a glance

| Method | Main idea | Output style | What changes with query? |
|---|---|---|---|
| KNN | Use nearest stored neighbours | vote / average | chosen neighbours |
| Weighted KNN | Use neighbours, but weight close ones more | weighted vote / weighted average | weights and influence |
| LWR | Fit a local weighted regression model around query | local regression prediction | local fitted coefficients |
| RBF | Use local basis functions centred at prototypes | weighted basis-function output | basis activations |

---

## Advantages of instance-based learning

### 1. Conceptually simple

The reasoning is intuitive:
similar inputs should give similar outputs.

### 2. Flexible

No single global model shape is forced on the data.

### 3. Works for both classification and regression

KNN can handle both.

### 4. Naturally local

Useful when local neighbourhood structure is more meaningful than one global formula.

### 5. Little or no training cost

Training often means storing the examples.

---

## Limitations of instance-based learning

### 1. Expensive prediction time

Distance must be computed at query time.

### 2. Sensitive to scaling

Unscaled features can dominate distance.

### 3. Sensitive to irrelevant features

This leads to misleading neighbourhoods.

### 4. Sensitive to choice of $k$

Poor hyperparameter choice hurts performance.

### 5. Suffers in high dimensions

Curse of dimensionality is a major issue.

### 6. Memory-heavy

Training examples must be stored.

---

## Common exam-style questions from this module

You should be ready for the following patterns.

### KNN classification

- compute Euclidean distance
- sort neighbours
- apply majority vote
- compare answers for $k=3$, $k=5$, $k=7$

### KNN regression

- compute nearest neighbours
- average their target values

### Best value of $k$

- use odd $k$ to avoid tie in binary classification
- identify elbow point from error plot
- explain effect of small vs large $k$

### Mixed-data dissimilarity

- normalise numeric features
- rank ordinal features
- compare nominal features
- combine the partial distances

### Weighted KNN

- compute distances
- compute kernel weights
- show why weighted result may differ from unweighted majority vote

### LWR conceptual question

- explain why LWR produces different local lines for different query points
- compare global regression with local weighted regression

### Curse of dimensionality

- define it
- explain why KNN is misled in high dimensions
- suggest remedies

---

## High-value points to remember for exams

- KNN is a **lazy learner**
- Model-based learning is an **eager learner**
- KNN can solve **classification and regression**
- Classification uses **majority vote**
- Regression uses **mean or median**
- $k$ is a **hyperparameter**
- odd $k$ helps avoid ties in binary classification
- distance metric choice matters
- feature scaling matters
- high dimensionality can mislead nearest-neighbour methods
- weighted KNN gives more influence to closer neighbours
- LWR builds a **local model around the query**
- RBF also captures **local influence through distance from centres**

---

## Quick comparison: global vs local thinking

{{< mermaid >}}
flowchart TD
  A[One global regression line] --> B[Ordinary linear regression]
  C[Neighbour vote or average] --> D[KNN]
  E[Weighted neighbours] --> F[Distance-weighted KNN]
  G[Local fitted regression line] --> H[Locally weighted regression]
  I[Local basis centred at prototypes] --> J[Radial Basis Functions]
{{< /mermaid >}}

---

## Final summary

Instance-based learning is built on one central belief:

**a useful prediction can often be made by looking at similar previously observed examples**.

KNN is the simplest and most direct version of this idea.

Distance-weighted KNN improves it by giving stronger influence to closer neighbours.

Locally weighted regression extends the same idea to local regression fitting.

Radial basis functions provide another local-distance-based way to construct non-linear predictors.

So although these topics look different,
they are all connected by the same local learning principle:

**nearby points should matter more than faraway points**.

---

## References

- [My Notes: Machine Learning Workflow]({{% relref "02-ml-workflow.md" %}})
- [My Notes: Linear Models for Regression]({{% relref "03-linear-models-regression.md" %}})
- [My Notes: Linear Models for Classification]({{% relref "04-linear-models-classification.md" %}})

---

{{< home-link "Home" >}} | {{< section-index >}}
