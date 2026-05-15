---
title: "Unsupervised Learning"
draft: false
tags: ["AI", "ML", "Unsupervised Learning", "K-means", "EM", "GMM", "Clustering"]
categories: ["AI", "ML"]
weight: 1000
menu: main
---

# Unsupervised Learning

Unsupervised Learning is used when we have input data but no target labels.

The model is not told the correct answer. Instead, it tries to discover hidden structure in the data.

---

## Supervised vs Unsupervised Learning

| Aspect | Supervised Learning | Unsupervised Learning |
|---|---|---|
| Data contains target label? | Yes | No |
| Learns from | Input-output pairs | Input features only |
| Main goal | Predict output | Discover structure |
| Example task | Classification, regression | Clustering |
| Example algorithm | Logistic regression, decision tree | K-means, GMM |

---

- Works on **unlabelled raw data**.
- The algorithm **discovers hidden patterns** without prior knowledge of outcomes.  
- Requires **no human intervention** during training.  
- Does not make direct predictions — it **groups or organises data** instead.
- Carries a **higher risk** because there’s no ground truth to verify results.  
- Common techniques include **Clustering**, **Association**, and **Dimensionality Reduction**.  

The most common example is **clustering**, where similar records are grouped together.

{{% hint info %}}
**Key takeaway:**  
Unsupervised Learning finds patterns in data without labelled outputs.

For this module, the main focus is clustering: K-means, mixture models, EM, and GMM-based soft clustering.
{{% /hint %}}

- K-means clustering and variants
- Review of EM algorithm
- GMM-based soft clustering
- Applications

---

{{< mermaid >}}
stateDiagram-v2

  %% ML maths-based colours (same palette as supervised)
  classDef probability fill:#d1fae5,stroke:#065f46,stroke-width:1px
  classDef geometry fill:#ffedd5,stroke:#9a3412,stroke-width:1px
  classDef category font-style:italic,font-weight:bold,fill:#f3f4f6,stroke:#374151

  %% Root
  USL: Unsupervised Learning

  %% Main branches
  USL --> CLU:::category
  CLU: Clustering

  USL --> DR:::category
  DR: Dimensionality Reduction

  %% Clustering algorithms
  CLU --> KM:::geometry
  KM: K-Means

  CLU --> HC:::geometry
  HC: Hierarchical Clustering

  CLU --> DB:::geometry
  DB: DBSCAN

  %% Probabilistic models
  USL --> PM:::category
  PM: Probabilistic Models

  PM --> GMM:::probability
  GMM: Gaussian Mixture Model

  PM --> HMM:::probability
  HMM: Hidden Markov Model
{{< /mermaid >}}

---

## Clustering
- Groups **similar data points** together based on shared features.  
- Commonly used for **market segmentation**, **image compression**, and **anomaly detection**.  

## Association
- Identifies **relationships or correlations** between variables in a dataset.  
- Commonly used in **market basket analysis** (e.g. "Customers who bought X also bought Y").  

### Common Techniques
- **Apriori Algorithm** – Finds frequent itemsets and generates association rules.  
- **Eclat Algorithm** – Similar to Apriori but uses set intersections for faster computation.  
---

## Dimensionality Reduction
- Reduces the **number of input variables** to simplify data.  
- Helps remove noise and redundancy.  
- Commonly used in **data pre-processing** and **visualisation**.  

### Common Techniques
- **Principal Component Analysis (PCA)** – Projects data onto fewer dimensions while keeping most variance.  
- **Linear Discriminant Analysis (LDA)** – Focuses on class separation.  
- **t-SNE (t-Distributed Stochastic Neighbour Embedding)** – Used for visualising high-dimensional data.  
- **Autoencoders** – Neural networks that compress and reconstruct data.  

---

{{< mermaid >}}
mindmap
  root(Unsupervised Learning)
    Clustering
      K Means
      Hierarchical Clustering
      DBSCAN
    Dimensionality Reduction
      PCA
      t SNE
      Autoencoders
    Probabilistic Models
      Gaussian Mixture Model
      Hidden Markov Model
{{< /mermaid >}}

---

## Clustering ☆

Clustering means grouping data points so that:

- points inside the same cluster are similar
- points in different clusters are dissimilar

For example:

- grouping customers by buying behaviour
- grouping documents by topic
- grouping images by visual similarity
- grouping patients by medical measurements

{{< mermaid >}}
flowchart LR
    A["Unlabelled Data"] --> B["Similarity / Distance Measure"]
    B --> C["Clustering Algorithm"]
    C --> D["Groups / Clusters"]
    D --> E["Interpret Hidden Structure"]

    style A fill:#E1F5FE,stroke:#5b7db1,color:#222
    style B fill:#C8E6C9,stroke:#5f8f6a,color:#222
    style C fill:#FFF9C4,stroke:#b59b3b,color:#222
    style D fill:#EDE7F6,stroke:#8a6fb3,color:#222
    style E fill:#FDE2E4,stroke:#b85c68,color:#222
{{< /mermaid >}}

### Common Types of Clustering
- **K-Means Clustering** – Divides data into *K* groups based on similarity.  
- **Hierarchical Clustering** – Builds a hierarchy (tree) of clusters.  
- **DBSCAN (Density-Based Spatial Clustering)** – Groups points close in density; identifies noise/outliers.  

---

## K-means Clustering ☆

K-means is a clustering algorithm that divides data into {{< katex >}} K {{< /katex >}} clusters.

Each cluster has a centre called a **centroid**.

The algorithm repeatedly:

1. assigns each point to the nearest centroid
2. updates each centroid using the mean of the assigned points
3. repeats until the centroids stop changing significantly

---

## K-means Objective Function ☆

The goal of K-means is to minimise the distance between points and their assigned cluster centroids.

{{% colour "green" %}}
{{< katex display=true >}}
J = \sum_{k=1}^{K} \sum_{x_i \in C_k} \|x_i - \mu_k\|^2
{{< /katex >}}
{{% /colour %}}

Where:

| Symbol | Meaning |
|---|---|
| {{< katex >}} K {{< /katex >}} | Number of clusters |
| {{< katex >}} C_k {{< /katex >}} | Cluster {{< katex >}} k {{< /katex >}} |
| {{< katex >}} x_i {{< /katex >}} | Data point |
| {{< katex >}} \mu_k {{< /katex >}} | Centroid of cluster {{< katex >}} k {{< /katex >}} |
| {{< katex >}} J {{< /katex >}} | Within-cluster squared error |

{{% hint info %}}
K-means tries to make points close to their own centroid.

Lower objective value means tighter clusters.
{{% /hint %}}

---

## K-means Algorithm Steps ☆

### Step 1: Choose K

Decide the number of clusters.

Example: {{< katex >}} K = 3 {{< /katex >}} means the algorithm should form three groups.

---

### Step 2: Initialise Centroids

Choose initial centroids randomly or using a better method such as K-means++.

The lecturer emphasised that the initial centroid can be random.

---

### Step 3: Assignment Step

Assign each data point to the nearest centroid.

{{% colour "green" %}}
{{< katex display=true >}}
c_i = \arg\min_k \|x_i - \mu_k\|^2
{{< /katex >}}
{{% /colour %}}

This means each point goes to the cluster whose centroid is closest.

---

### Step 4: Update Step

Recalculate each centroid as the mean of all points assigned to that cluster.

{{% colour "green" %}}
{{< katex display=true >}}
\mu_k = \frac{1}{|C_k|} \sum_{x_i \in C_k} x_i
{{< /katex >}}
{{% /colour %}}

---

### Step 5: Repeat Until Convergence

Repeat assignment and update until:

- centroids stop changing
- cluster assignments stop changing
- maximum number of iterations is reached

---

## K-means Workflow

{{< mermaid >}}
flowchart TD
    A["Choose K"] --> B["Initialise Centroids"]
    B --> C["Assign Points to Nearest Centroid"]
    C --> D["Update Centroids"]
    D --> E{"Centroids Stable?"}
    E -- "No" --> C
    E -- "Yes" --> F["Final Clusters"]

    style A fill:#E1F5FE,stroke:#5b7db1,color:#222
    style B fill:#C8E6C9,stroke:#5f8f6a,color:#222
    style C fill:#FFF9C4,stroke:#b59b3b,color:#222
    style D fill:#EDE7F6,stroke:#8a6fb3,color:#222
    style E fill:#FDE2E4,stroke:#b85c68,color:#222
    style F fill:#C8E6C9,stroke:#5f8f6a,color:#222
{{< /mermaid >}}

---

## Choosing the Value of K ☆

K-means requires the number of clusters {{< katex >}} K {{< /katex >}} in advance.

Choosing {{< katex >}} K {{< /katex >}} is important.

If {{< katex >}} K {{< /katex >}} is too small, different groups may be forced together.

If {{< katex >}} K {{< /katex >}} is too large, natural groups may be split unnecessarily.

---

## Elbow Method

The elbow method plots clustering error against different values of {{< katex >}} K {{< /katex >}}.

Usually, as {{< katex >}} K {{< /katex >}} increases, the error decreases.

The “elbow” point is where improvement starts becoming smaller.

That point is often chosen as a reasonable {{< katex >}} K {{< /katex >}}.

{{% colour "green" %}}
{{< katex display=true >}}
WCSS = \sum_{k=1}^{K} \sum_{x_i \in C_k} \|x_i - \mu_k\|^2
{{< /katex >}}
{{% /colour %}}

WCSS means within-cluster sum of squares.

---

## K-means Variants

| Variant | Idea |
|---|---|
| Standard K-means | Randomly initialise centroids and iterate |
| K-means++ | Choose better initial centroids to improve stability |
| Mini-batch K-means | Use small batches for faster clustering on large datasets |
| Kernel K-means | Use kernel idea for more complex cluster shapes |

---

## Strengths of K-means

- Simple to understand
- Fast for many practical datasets
- Works well when clusters are compact and roughly spherical
- Easy to implement
- Useful as a baseline clustering method

---

## Limitations of K-means ☆

| Limitation | Explanation |
|---|---|
| Needs K in advance | User must choose number of clusters |
| Sensitive to initial centroids | Different initial points may give different clusters |
| Sensitive to scale | Feature scaling is important |
| Assumes compact clusters | Does not work well for irregular cluster shapes |
| Hard assignment | Each point belongs to exactly one cluster |
| Sensitive to outliers | Outliers can pull centroids away |

{{% hint warning %}}
Before applying K-means, scale numeric features.

Distance-based clustering can be dominated by features with larger ranges.
{{% /hint %}}

---

## Hard Clustering vs Soft Clustering ☆

K-means performs **hard clustering**.

Each point is assigned to exactly one cluster.

Gaussian Mixture Models perform **soft clustering**.

Each point receives a probability of belonging to each cluster.

| Aspect | Hard Clustering | Soft Clustering |
|---|---|---|
| Assignment | One cluster only | Probability for each cluster |
| Example | K-means | GMM |
| Output | Cluster label | Responsibility / probability |
| Useful when | Groups are clearly separated | Groups overlap |

---

## Mixture Models

A mixture model assumes the data comes from a mixture of several probability distributions.

For clustering, each distribution represents a cluster.

The data point is not simply assigned to one cluster immediately.

Instead, the model estimates how likely it is that the point came from each cluster.

---

## Gaussian Mixture Model ☆

A Gaussian Mixture Model assumes each cluster is represented by a Gaussian distribution.

The overall data distribution is a weighted combination of Gaussians.

{{% colour "green" %}}
{{< katex display=true >}}
p(x) = \sum_{k=1}^{K} \pi_k \mathcal{N}(x \mid \mu_k, \Sigma_k)
{{< /katex >}}
{{% /colour %}}

Where:

| Symbol | Meaning |
|---|---|
| {{< katex >}} K {{< /katex >}} | Number of Gaussian components |
| {{< katex >}} \pi_k {{< /katex >}} | Mixing coefficient for component {{< katex >}} k {{< /katex >}} |
| {{< katex >}} \mu_k {{< /katex >}} | Mean of component {{< katex >}} k {{< /katex >}} |
| {{< katex >}} \Sigma_k {{< /katex >}} | Covariance of component {{< katex >}} k {{< /katex >}} |
| {{< katex >}} \mathcal{N}(x \mid \mu_k, \Sigma_k) {{< /katex >}} | Gaussian density |

The mixing coefficients must sum to one.

{{% colour "green" %}}
{{< katex display=true >}}
\sum_{k=1}^{K} \pi_k = 1
{{< /katex >}}
{{% /colour %}}

---

## GMM Responsibility ☆

The probability that data point {{< katex >}} x_i {{< /katex >}} belongs to component {{< katex >}} k {{< /katex >}} is called its responsibility.

{{% colour "green" %}}
{{< katex display=true >}}
\gamma_{ik} = P(z_i = k \mid x_i)
{{< /katex >}}
{{% /colour %}}

Using Bayes Rule:

{{% colour "green" %}}
{{< katex display=true >}}
\gamma_{ik} = \frac{\pi_k \mathcal{N}(x_i \mid \mu_k, \Sigma_k)}{\sum_{j=1}^{K} \pi_j \mathcal{N}(x_i \mid \mu_j, \Sigma_j)}
{{< /katex >}}
{{% /colour %}}

This is the soft-clustering equivalent of assigning a point to a cluster.

---

## K-means vs GMM

| Aspect | K-means | GMM |
|---|---|---|
| Type | Hard clustering | Soft clustering |
| Cluster representation | Centroid | Gaussian distribution |
| Assignment | Nearest centroid | Probability of each component |
| Cluster shape | Roughly spherical | Can model ellipses via covariance |
| Output | Cluster label | Cluster probabilities |
| Method | Distance minimisation | Probability maximisation |

---

## EM Algorithm ☆

EM stands for **Expectation-Maximisation**.

It is used when there are hidden or latent variables.

In GMM clustering, the hidden variable is the unknown cluster membership.

We observe data points, but we do not know which Gaussian component generated each point.

---

## EM Algorithm Idea

EM alternates between two steps:

1. **E-step**: estimate the hidden cluster responsibilities.
2. **M-step**: update model parameters using those responsibilities.

{{< mermaid >}}
flowchart LR
    A["Initial Parameters"] --> B["E-step: Estimate Responsibilities"]
    B --> C["M-step: Update Parameters"]
    C --> D{"Converged?"}
    D -- "No" --> B
    D -- "Yes" --> E["Final GMM"]

    style A fill:#E1F5FE,stroke:#5b7db1,color:#222
    style B fill:#C8E6C9,stroke:#5f8f6a,color:#222
    style C fill:#FFF9C4,stroke:#b59b3b,color:#222
    style D fill:#EDE7F6,stroke:#8a6fb3,color:#222
    style E fill:#FDE2E4,stroke:#b85c68,color:#222
{{< /mermaid >}}

---

## E-step

The E-step computes responsibilities using the current parameters.

{{% colour "green" %}}
{{< katex display=true >}}
\gamma_{ik} = \frac{\pi_k \mathcal{N}(x_i \mid \mu_k, \Sigma_k)}{\sum_{j=1}^{K} \pi_j \mathcal{N}(x_i \mid \mu_j, \Sigma_j)}
{{< /katex >}}
{{% /colour %}}

Interpretation:

> Given the current Gaussians, how much responsibility should component {{< katex >}} k {{< /katex >}} take for point {{< katex >}} x_i {{< /katex >}}?

---

## M-step

The M-step updates the GMM parameters using the responsibilities from the E-step.

First compute the effective number of points assigned to cluster {{< katex >}} k {{< /katex >}}:

{{% colour "green" %}}
{{< katex display=true >}}
N_k = \sum_{i=1}^{n} \gamma_{ik}
{{< /katex >}}
{{% /colour %}}

Update the mean:

{{% colour "green" %}}
{{< katex display=true >}}
\mu_k = \frac{1}{N_k} \sum_{i=1}^{n} \gamma_{ik} x_i
{{< /katex >}}
{{% /colour %}}

Update the covariance:

{{% colour "green" %}}
{{< katex display=true >}}
\Sigma_k = \frac{1}{N_k} \sum_{i=1}^{n} \gamma_{ik}(x_i - \mu_k)(x_i - \mu_k)^T
{{< /katex >}}
{{% /colour %}}

Update the mixing coefficient:

{{% colour "green" %}}
{{< katex display=true >}}
\pi_k = \frac{N_k}{n}
{{< /katex >}}
{{% /colour %}}

---

## EM Objective

EM tries to maximise the likelihood of the observed data.

For GMM:

{{% colour "green" %}}
{{< katex display=true >}}
\log p(X) = \sum_{i=1}^{n} \log \left( \sum_{k=1}^{K} \pi_k \mathcal{N}(x_i \mid \mu_k, \Sigma_k) \right)
{{< /katex >}}
{{% /colour %}}

Each EM iteration should not decrease this likelihood.

---

## K-means and EM Connection

K-means can be viewed as a simpler hard-assignment version of mixture modelling.

| Step | K-means | GMM with EM |
|---|---|---|
| Assignment | Assign point to nearest centroid | Compute probability for each Gaussian |
| Update | Recompute centroids | Recompute means, covariances, mixing weights |
| Cluster membership | Hard | Soft |
| Objective | Minimise squared distance | Maximise likelihood |

---

## Applications of Unsupervised Learning

Unsupervised Learning is used in:

- customer segmentation
- anomaly detection
- image segmentation
- document/topic grouping
- market basket analysis
- biological data analysis
- recommender systems
- exploratory data analysis

---

## Practical ML Workflow

1. Prepare data.
2. Scale numerical features.
3. Choose clustering method.
4. Choose number of clusters or components.
5. Fit the model.
6. Analyse clusters.
7. Validate using domain knowledge and metrics.

---

## Evaluation of Clustering

Since there are no labels, clustering evaluation is harder than supervised learning.

Common approaches include:

| Method | Meaning |
|---|---|
| Inertia / WCSS | Lower within-cluster distance is better |
| Silhouette score | Measures separation and compactness |
| Visual inspection | Useful for low-dimensional data |
| Domain interpretation | Clusters should make practical sense |
| Stability check | Similar clusters should appear across runs |

---

## Silhouette Score

For a data point:

- {{< katex >}} a {{< /katex >}} = average distance to points in its own cluster
- {{< katex >}} b {{< /katex >}} = average distance to points in the nearest different cluster

{{% colour "green" %}}
{{< katex display=true >}}
s = \frac{b-a}{\max(a,b)}
{{< /katex >}}
{{% /colour %}}

A higher value usually indicates better clustering.

---

## Notes ☆

You should be comfortable with:

1. Explaining why unsupervised learning does not use target labels.
2. Describing the K-means algorithm step by step.
3. Computing or explaining centroid updates.
4. Explaining why K-means is hard clustering.
5. Explaining why GMM is soft clustering.
6. Writing the GMM probability density formula.
7. Explaining the E-step and M-step of EM.
8. Comparing K-means and GMM.
9. Identifying suitable applications of clustering.

---

## Common Mistakes

| Mistake | Correction |
|---|---|
| Thinking clustering uses labels | Clustering works without target labels |
| Forgetting to scale data | Distance-based methods need scaling |
| Choosing K blindly | Use elbow method, domain knowledge, or validation |
| Treating GMM like K-means | GMM gives probabilities, not just labels |
| Confusing E-step and M-step | E-step estimates responsibilities; M-step updates parameters |
| Ignoring covariance in GMM | Covariance controls cluster shape |

---

## Revision

| Concept | Main Idea | Key Formula |
|---|---|---|
| K-means | Hard clustering using centroids | {{< katex >}} \sum_k \sum_{x_i \in C_k} \|x_i - \mu_k\|^2 {{< /katex >}} |
| Centroid update | Mean of assigned points | {{< katex >}} \mu_k = \frac{1}{|C_k|}\sum_{x_i \in C_k}x_i {{< /katex >}} |
| GMM | Soft clustering using Gaussians | {{< katex >}} p(x)=\sum_k \pi_k \mathcal{N}(x\mid\mu_k,\Sigma_k) {{< /katex >}} |
| Responsibility | Probability of component for a point | {{< katex >}} \gamma_{ik}=P(z_i=k\mid x_i) {{< /katex >}} |
| EM | Alternates E-step and M-step | Estimate responsibilities, then update parameters |

---

{{< home-link "Home" >}} | {{< section-index >}}
