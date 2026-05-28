---
title: "Dimensionality reduction and PCA"
draft: false
tags: ["Machine Learning", "Mathematics", "Linear Algebra"]
categories: ["AI", "ML"]
weight: 1700
bookCollapseSection: true
---
# Dimensionality reduction and PCA

PCA and SVM connect linear algebra, geometry, and optimisation.

Dimensionality reduction means representing high-dimensional data using fewer dimensions while trying to preserve the important structure of the data.

Principal Components Analysis, or PCA, is a **linear dimensionality reduction** method.
It finds directions in the data along which the variance is maximum, and projects the data onto those directions.

{{% hint info %}}
**Key takeaway:** PCA chooses the eigenvectors of the covariance matrix corresponding to the largest eigenvalues.
These eigenvectors form the principal subspace.
The largest eigenvalues represent the directions that preserve the most variance.
{{% /hint %}}

---

## Why Dimensionality Reduction?

High-dimensional data can be difficult to:

- visualise
- interpret
- store efficiently
- process computationally
- use in machine learning without noise or redundancy

Often, high-dimensional data is **overcomplete**.
This means some dimensions are redundant or correlated with others.

For example, if two features are highly correlated, most of the useful information may lie close to a lower-dimensional line or plane.

---

## Principal Component Analysis (PCA)

- dimensionality reduction technique 
- helps us to **reduce the number of features** in a dataset while keeping the most important information.
- changes complex datasets by transforming correlated features into a smaller set of uncorrelated components.
- uses **linear algebra** to transform data into **new features** called principal components.
- finds these by calculating **eigenvectors (directions)** and **eigenvalues (importance)** from the **covariance matrix**.
- PCA **selects the top components with the highest eigenvalues** and **projects the data onto them simplify the dataset**.

{{% hint %}}

PCA prioritizes the directions where the data varies the most because more variation = more useful information.

{{% /hint %}}

---

{{% steps %}}

1. Standardize the Data
3. Calculate **Covariance Matrix**
4. Find the Principal Components
5. Pick the Top Directions & Transform Data

{{% /steps %}}

---

## PCA Problem Setting

Assume we have centred data:

{{% colour "green" %}}
{{< katex display=true >}}
x_1, x_2, \ldots, x_N \in \mathbb{R}^D
{{< /katex >}}
{{% /colour %}}

The data has mean zero:

{{% colour "green" %}}
{{< katex display=true >}}
\mathbb{E}[x] = 0
{{< /katex >}}
{{% /colour %}}

The covariance matrix is:

{{% colour "green" %}}
{{< katex display=true >}}
S = \frac{1}{N}\sum_{n=1}^{N}x_nx_n^T
{{< /katex >}}
{{% /colour %}}

If the data matrix is:

{{% colour "green" %}}
{{< katex display=true >}}
X = [x_1, x_2, \ldots, x_N] \in \mathbb{R}^{D \times N}
{{< /katex >}}
{{% /colour %}}

then:

{{% colour "green" %}}
{{< katex display=true >}}
S = \frac{1}{N}XX^T
{{< /katex >}}
{{% /colour %}}

---

## Low-Dimensional Representation

PCA looks for a lower-dimensional code:

{{% colour "green" %}}
{{< katex display=true >}}
z_n \in \mathbb{R}^M
{{< /katex >}}
{{% /colour %}}

where:

{{% colour "green" %}}
{{< katex display=true >}}
M < D
{{< /katex >}}
{{% /colour %}}

Let:

{{% colour "green" %}}
{{< katex display=true >}}
B = [b_1, b_2, \ldots, b_M] \in \mathbb{R}^{D \times M}
{{< /katex >}}
{{% /colour %}}

where the columns of {{< katex >}}B{{< /katex >}} are orthonormal basis vectors.

The compressed representation is:

{{% colour "green" %}}
{{< katex display=true >}}
z = B^Tx
{{< /katex >}}
{{% /colour %}}

The reconstructed data is:

{{% colour "green" %}}
{{< katex display=true >}}
\tilde{x} = Bz
{{< /katex >}}
{{% /colour %}}

Substituting {{< katex >}}z = B^Tx{{< /katex >}}:

{{% colour "green" %}}
{{< katex display=true >}}
\tilde{x} = BB^Tx
{{< /katex >}}
{{% /colour %}}

---

## Bottleneck View of PCA

PCA can be understood as a bottleneck:

```text
Original data x  →  compressed code z  →  reconstructed data x̃
     R^D                    R^M                    R^D
```

The low-dimensional code {{< katex >}}z{{< /katex >}} controls how much information flows from the original data to the reconstructed data.

If {{< katex >}}M{{< /katex >}} is small, compression is strong.
If {{< katex >}}M{{< /katex >}} is large, more information is preserved.

---

## Centring the Data

Before PCA, we usually subtract the mean.

If the mean of the data is {{< katex >}}\mu{{< /katex >}}, then the centred data is:

{{% colour "green" %}}
{{< katex display=true >}}
x_c = x - \mu
{{< /katex >}}
{{% /colour %}}

This is important because PCA is based on variance around the centre of the data.

The slides emphasise that for centred data:

{{% colour "green" %}}
{{< katex display=true >}}
\mathbb{E}[x] = 0
{{< /katex >}}
{{% /colour %}}

and:

{{% colour "green" %}}
{{< katex display=true >}}
S = \frac{1}{N}\sum_{n=1}^{N}x_nx_n^T
{{< /katex >}}
{{% /colour %}}

---

## Standardisation

In practice, PCA is often applied after standardisation.

For each dimension {{< katex >}}d{{< /katex >}}:

{{% colour "green" %}}
{{< katex display=true >}}
x_*^{(d)} = \frac{x_*^{(d)} - \mu_d}{\sigma_d}
{{< /katex >}}
{{% /colour %}}

where:

- {{< katex >}}\mu_d{{< /katex >}} is the training-data mean for dimension {{< katex >}}d{{< /katex >}}
- {{< katex >}}\sigma_d{{< /katex >}} is the training-data standard deviation for dimension {{< katex >}}d{{< /katex >}}

Standardisation makes the data unit-free and gives each feature comparable scale.

{{% hint warning %}}
For exam numericals, do not use the test point's own mean and standard deviation.
Use the **training data mean and standard deviation**.
{{% /hint %}}

---

## Maximum Variance Perspective

The key PCA idea is:

> choose the direction where the projected data has maximum variance.

For the first principal component, let {{< katex >}}b_1{{< /katex >}} be a unit vector.

The projection of {{< katex >}}x_n{{< /katex >}} onto {{< katex >}}b_1{{< /katex >}} is:

{{% colour "green" %}}
{{< katex display=true >}}
z_{1n} = b_1^Tx_n
{{< /katex >}}
{{% /colour %}}

The variance of the projected data is:

{{% colour "green" %}}
{{< katex display=true >}}
V_1 = \frac{1}{N}\sum_{n=1}^{N}z_{1n}^2
{{< /katex >}}
{{% /colour %}}

Substitute {{< katex >}}z_{1n} = b_1^Tx_n{{< /katex >}}:

{{% colour "green" %}}
{{< katex display=true >}}
V_1 = \frac{1}{N}\sum_{n=1}^{N}(b_1^Tx_n)^2
{{< /katex >}}
{{% /colour %}}

This becomes:

{{% colour "green" %}}
{{< katex display=true >}}
V_1 = b_1^TSb_1
{{< /katex >}}
{{% /colour %}}

So PCA solves:

{{% colour "green" %}}
{{< katex display=true >}}
\max_{b_1} b_1^TSb_1
{{< /katex >}}
{{% /colour %}}

subject to:

{{% colour "green" %}}
{{< katex display=true >}}
\|b_1\| = 1
{{< /katex >}}
{{% /colour %}}

The constraint is needed because otherwise we could increase the magnitude of {{< katex >}}b_1{{< /katex >}} and artificially increase the variance.

---

## Lagrangian Derivation of the First Principal Component

Set up the constrained optimisation problem:

{{% colour "green" %}}
{{< katex display=true >}}
\max b_1^TSb_1
{{< /katex >}}
{{% /colour %}}

subject to:

{{% colour "green" %}}
{{< katex display=true >}}
b_1^Tb_1 = 1
{{< /katex >}}
{{% /colour %}}

The Lagrangian is:

{{% colour "green" %}}
{{< katex display=true >}}
\mathcal{L}(b_1,\lambda)=b_1^TSb_1+\lambda(1-b_1^Tb_1)
{{< /katex >}}
{{% /colour %}}

Now set the derivatives to zero:

{{% colour "green" %}}
{{< katex display=true >}}
\frac{\partial \mathcal{L}}{\partial \lambda}=1-b_1^Tb_1=0
{{< /katex >}}
{{% /colour %}}

and:

{{% colour "green" %}}
{{< katex display=true >}}
\frac{\partial \mathcal{L}}{\partial b_1}=0
{{< /katex >}}
{{% /colour %}}

This gives:

{{% colour "green" %}}
{{< katex display=true >}}
Sb_1=\lambda b_1
{{< /katex >}}
{{% /colour %}}

So {{< katex >}}b_1{{< /katex >}} is an eigenvector of the covariance matrix {{< katex >}}S{{< /katex >}}, and {{< katex >}}\lambda{{< /katex >}} is the corresponding eigenvalue.

---

## Why the Largest Eigenvalue?

The variance in direction {{< katex >}}b_1{{< /katex >}} is:

{{% colour "green" %}}
{{< katex display=true >}}
V_1 = b_1^TSb_1
{{< /katex >}}
{{% /colour %}}

Using:

{{% colour "green" %}}
{{< katex display=true >}}
Sb_1=\lambda b_1
{{< /katex >}}
{{% /colour %}}

we get:

{{% colour "green" %}}
{{< katex display=true >}}
V_1 = b_1^T\lambda b_1
{{< /katex >}}
{{% /colour %}}

Since {{< katex >}}b_1^Tb_1=1{{< /katex >}}:

{{% colour "green" %}}
{{< katex display=true >}}
V_1 = \lambda
{{< /katex >}}
{{% /colour %}}

Therefore, to maximise variance, choose the eigenvector with the largest eigenvalue.

This is the **first principal component**.

---

## More Than One Principal Component

For an {{< katex >}}M{{< /katex >}}-dimensional PCA subspace, choose:

{{% colour "green" %}}
{{< katex display=true >}}
B = [b_1,b_2,\ldots,b_M]
{{< /katex >}}
{{% /colour %}}

where {{< katex >}}b_1,\ldots,b_M{{< /katex >}} are the eigenvectors of {{< katex >}}S{{< /katex >}} corresponding to the {{< katex >}}M{{< /katex >}} largest eigenvalues.

The projected coordinates are:

{{% colour "green" %}}
{{< katex display=true >}}
z = B^Tx
{{< /katex >}}
{{% /colour %}}

The reconstruction is:

{{% colour "green" %}}
{{< katex display=true >}}
\tilde{x}=Bz=BB^Tx
{{< /katex >}}
{{% /colour %}}

---

## Variance Explained

If the eigenvalues are:

{{% colour "green" %}}
{{< katex display=true >}}
\lambda_1 \ge \lambda_2 \ge \cdots \ge \lambda_D
{{< /katex >}}
{{% /colour %}}

then the variance retained by the first {{< katex >}}M{{< /katex >}} components is:

{{% colour "green" %}}
{{< katex display=true >}}
\frac{\lambda_1+\lambda_2+\cdots+\lambda_M}{\lambda_1+\lambda_2+\cdots+\lambda_D}
{{< /katex >}}
{{% /colour %}}

This is often called the **explained variance ratio**.

---

## Projection Perspective

PCA can also be derived as a reconstruction problem.

Instead of asking:

> Which directions maximise variance?

we ask:

> Which lower-dimensional subspace minimises reconstruction error?

The reconstruction error is:

{{% colour "green" %}}
{{< katex display=true >}}
\sum_{n=1}^{N}\|x_n-\tilde{x}_n\|^2
{{< /katex >}}
{{% /colour %}}

PCA gives the subspace that minimises this average reconstruction error.

So PCA has two equivalent views:

| Perspective | Goal |
|---|---|
| Maximum variance | Preserve maximum spread/information |
| Projection/reconstruction | Minimise reconstruction error |

---

## PCA and SVD

From the slides:

{{% colour "green" %}}
{{< katex display=true >}}
S = \frac{1}{N}XX^T
{{< /katex >}}
{{% /colour %}}

Assume the singular value decomposition of {{< katex >}}X{{< /katex >}} is:

{{% colour "green" %}}
{{< katex display=true >}}
X = U\Sigma V^T
{{< /katex >}}
{{% /colour %}}

Then:

{{% colour "green" %}}
{{< katex display=true >}}
S = \frac{1}{N}XX^T
= \frac{1}{N}U\Sigma \Sigma^T U^T
{{< /katex >}}
{{% /colour %}}

The columns of {{< katex >}}U{{< /katex >}} are the eigenvectors of {{< katex >}}S{{< /katex >}}.

The eigenvalues of {{< katex >}}S{{< /katex >}} are related to the singular values of {{< katex >}}X{{< /katex >}}:

{{% colour "green" %}}
{{< katex display=true >}}
\lambda_d = \frac{\sigma_d^2}{N}
{{< /katex >}}
{{% /colour %}}

This connects PCA with SVD.

---

## PCA as Low-Rank Approximation

PCA can also be understood using the Eckart–Young theorem.

The best rank-{{< katex >}}M{{< /katex >}} approximation to {{< katex >}}X{{< /katex >}} is obtained by truncating the SVD:

{{% colour "green" %}}
{{< katex display=true >}}
\tilde{X}_M = U_M\Sigma_MV_M^T
{{< /katex >}}
{{% /colour %}}

where:

- {{< katex >}}U_M{{< /katex >}} contains the top {{< katex >}}M{{< /katex >}} left singular vectors
- {{< katex >}}\Sigma_M{{< /katex >}} contains the top {{< katex >}}M{{< /katex >}} singular values
- {{< katex >}}V_M{{< /katex >}} contains the corresponding right singular vectors

This gives the best low-rank approximation to {{< katex >}}X{{< /katex >}}.

---

## PCA in High Dimensions

In high dimensions, the covariance matrix is:

{{% colour "green" %}}
{{< katex display=true >}}
S \in \mathbb{R}^{D \times D}
{{< /katex >}}
{{% /colour %}}

Computing eigenvectors of a {{< katex >}}D \times D{{< /katex >}} matrix can be expensive.

This is especially difficult when:

{{% colour "green" %}}
{{< katex display=true >}}
D \gg N
{{< /katex >}}
{{% /colour %}}

A useful trick is to use:

{{% colour "green" %}}
{{< katex display=true >}}
X^TX \in \mathbb{R}^{N \times N}
{{< /katex >}}
{{% /colour %}}

instead of:

{{% colour "green" %}}
{{< katex display=true >}}
XX^T \in \mathbb{R}^{D \times D}
{{< /katex >}}
{{% /colour %}}

The non-zero eigenvalues of {{< katex >}}XX^T{{< /katex >}} and {{< katex >}}X^TX{{< /katex >}} are the same.

If {{< katex >}}c_m{{< /katex >}} is an eigenvector of {{< katex >}}X^TX{{< /katex >}}, then:

{{% colour "green" %}}
{{< katex display=true >}}
Xc_m
{{< /katex >}}
{{% /colour %}}

can be used to recover the corresponding eigenvector of {{< katex >}}XX^T{{< /katex >}}.

---

## Key Steps of PCA in Practice

The lecture gives the practical PCA pipeline as:

1. Mean subtraction
2. Standardisation
3. Eigendecomposition of the covariance matrix
4. Projection

### Step 1: Mean Subtraction

{{% colour "green" %}}
{{< katex display=true >}}
x_c = x - \mu
{{< /katex >}}
{{% /colour %}}

### Step 2: Standardisation

{{% colour "green" %}}
{{< katex display=true >}}
x_s^{(d)}=\frac{x^{(d)}-\mu_d}{\sigma_d}
{{< /katex >}}
{{% /colour %}}

### Step 3: Covariance Matrix

{{% colour "green" %}}
{{< katex display=true >}}
S = \frac{1}{N}XX^T
{{< /katex >}}
{{% /colour %}}

### Step 4: Eigenvectors and Eigenvalues

Solve:

{{% colour "green" %}}
{{< katex display=true >}}
Sb_i=\lambda_i b_i
{{< /katex >}}
{{% /colour %}}

Choose the eigenvectors with the largest eigenvalues.

### Step 5: Projection

{{% colour "green" %}}
{{< katex display=true >}}
z = B^Tx
{{< /katex >}}
{{% /colour %}}

### Step 6: Reconstruction

{{% colour "green" %}}
{{< katex display=true >}}
\tilde{x}=Bz
{{< /katex >}}
{{% /colour %}}

---

## Exam Method for PCA Numericals

Use this template.

### Given Data Matrix

Write the data matrix {{< katex >}}X{{< /katex >}}.

### Find the Mean

{{% colour "green" %}}
{{< katex display=true >}}
\mu = \frac{1}{N}\sum_{n=1}^{N}x_n
{{< /katex >}}
{{% /colour %}}

### Centre the Data

{{% colour "green" %}}
{{< katex display=true >}}
X_c = X - \mu
{{< /katex >}}
{{% /colour %}}

### Compute Covariance

{{% colour "green" %}}
{{< katex display=true >}}
S = \frac{1}{N}X_cX_c^T
{{< /katex >}}
{{% /colour %}}

### Find Eigenvalues and Eigenvectors

Solve:

{{% colour "green" %}}
{{< katex display=true >}}
\det(S-\lambda I)=0
{{< /katex >}}
{{% /colour %}}

Then find eigenvectors.

### Choose Principal Components

Choose eigenvectors corresponding to largest eigenvalues.

### Project Data

{{% colour "green" %}}
{{< katex display=true >}}
z = B^TX_c
{{< /katex >}}
{{% /colour %}}

### Reconstruct If Asked

{{% colour "green" %}}
{{< katex display=true >}}
\tilde{X}=Bz+\mu
{{< /katex >}}
{{% /colour %}}

---

## Common Exam Mistakes

| Mistake | Correction |
|---|---|
| Forgetting to centre the data | Always subtract the mean first |
| Choosing smallest eigenvalues | PCA chooses largest eigenvalues |
| Confusing {{< katex >}}B^Tx{{< /katex >}} and {{< katex >}}Bx{{< /katex >}} | {{< katex >}}z=B^Tx{{< /katex >}}, reconstruction is {{< katex >}}\tilde{x}=Bz{{< /katex >}} |
| Forgetting to add mean back after reconstruction | Use {{< katex >}}\tilde{x}=Bz+\mu{{< /katex >}} if returning to original space |
| Using test-data statistics | Standardise test points using training mean and standard deviation |

---

## Quick Memory Line

```text
Centre → Covariance → Eigenvectors → Largest Eigenvalues → Project → Reconstruct
```

---

## Source Focus

This page is based mainly on:

- Lecture 12: PCA problem setting, maximum variance perspective, Lagrangian derivation, first principal component, projection perspective
- Lecture 13: SVD connection, low-rank approximation, high-dimensional PCA, practical PCA steps
- Course handout: Module 7 and sessions 12–13

---

{{< section_tree >}}
{{< section_tree >}}

---

{{< home-link "Home" >}} | {{< section-index >}}
