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


## PCA at a Glance

{{< mermaid >}}
flowchart TD
    A[High-dimensional Data] --> B[Centre / Standardise]
    B --> C[Covariance Matrix]
    C --> D[Eigenvalues and Eigenvectors]
    D --> E[Choose Top M Components]
    E --> F[Low-dimensional Representation]
    E --> G[Reconstruction]
{{< /mermaid >}}


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
\[
x_1, x_2, \ldots, x_N \in \mathbb{R}^D
\]
{{% /colour %}}

The data has mean zero:

{{% colour "green" %}}
\[
\mathbb{E}[x] = 0
\]
{{% /colour %}}

The covariance matrix is:

{{% colour "green" %}}
\[
S = \frac{1}{N}\sum_{n=1}^{N}x_nx_n^T
\]
{{% /colour %}}

If the data matrix is:

{{% colour "green" %}}
\[
X = [x_1, x_2, \ldots, x_N] \in \mathbb{R}^{D \times N}
\]
{{% /colour %}}

then:

{{% colour "green" %}}
\[
S = \frac{1}{N}XX^T
\]
{{% /colour %}}

---

## Low-Dimensional Representation

PCA looks for a lower-dimensional code:

{{% colour "green" %}}
\[
z_n \in \mathbb{R}^M
\]
{{% /colour %}}

where:

{{% colour "green" %}}
\[
M < D
\]
{{% /colour %}}

Let:

{{% colour "green" %}}
\[
B = [b_1, b_2, \ldots, b_M] \in \mathbb{R}^{D \times M}
\]
{{% /colour %}}

where the columns of \(B\) are orthonormal basis vectors.

The compressed representation is:

{{% colour "green" %}}
\[
z = B^Tx
\]
{{% /colour %}}

The reconstructed data is:

{{% colour "green" %}}
\[
\tilde{x} = Bz
\]
{{% /colour %}}

Substituting \(z = B^Tx\):

{{% colour "green" %}}
\[
\tilde{x} = BB^Tx
\]
{{% /colour %}}

---

## Bottleneck View of PCA

PCA can be understood as a bottleneck:

```text
Original data x  →  compressed code z  →  reconstructed data x̃
     R^D                    R^M                    R^D
```

The low-dimensional code \(z\) controls how much information flows from the original data to the reconstructed data.

If \(M\) is small, compression is strong.
If \(M\) is large, more information is preserved.

---

## Centring the Data

Before PCA, we usually subtract the mean.

If the mean of the data is \(\mu\), then the centred data is:

{{% colour "green" %}}
\[
x_c = x - \mu
\]
{{% /colour %}}

This is important because PCA is based on variance around the centre of the data.

The slides emphasise that for centred data:

{{% colour "green" %}}
\[
\mathbb{E}[x] = 0
\]
{{% /colour %}}

and:

{{% colour "green" %}}
\[
S = \frac{1}{N}\sum_{n=1}^{N}x_nx_n^T
\]
{{% /colour %}}

---

## Standardisation

In practice, PCA is often applied after standardisation.

For each dimension \(d\):

{{% colour "green" %}}
\[
x_*^{(d)} = \frac{x_*^{(d)} - \mu_d}{\sigma_d}
\]
{{% /colour %}}

where:

- \(\mu_d\) is the training-data mean for dimension \(d\)
- \(\sigma_d\) is the training-data standard deviation for dimension \(d\)

Standardisation makes the data unit-free and gives each feature comparable scale.

{{% hint warning %}}
For exam numericals, do not use the test point's own mean and standard deviation.
Use the **training data mean and standard deviation**.
{{% /hint %}}

---

## Maximum Variance Perspective

The key PCA idea is:

> choose the direction where the projected data has maximum variance.

For the first principal component, let \(b_1\) be a unit vector.

The projection of \(x_n\) onto \(b_1\) is:

{{% colour "green" %}}
\[
z_{1n} = b_1^Tx_n
\]
{{% /colour %}}

The variance of the projected data is:

{{% colour "green" %}}
\[
V_1 = \frac{1}{N}\sum_{n=1}^{N}z_{1n}^2
\]
{{% /colour %}}

Substitute \(z_{1n} = b_1^Tx_n\):

{{% colour "green" %}}
\[
V_1 = \frac{1}{N}\sum_{n=1}^{N}(b_1^Tx_n)^2
\]
{{% /colour %}}

This becomes:

{{% colour "green" %}}
\[
V_1 = b_1^TSb_1
\]
{{% /colour %}}

So PCA solves:

{{% colour "green" %}}
\[
\max_{b_1} b_1^TSb_1
\]
{{% /colour %}}

subject to:

{{% colour "green" %}}
\[
\|b_1\| = 1
\]
{{% /colour %}}

The constraint is needed because otherwise we could increase the magnitude of \(b_1\) and artificially increase the variance.

---

## Lagrangian Derivation of the First Principal Component

Set up the constrained optimisation problem:

{{% colour "green" %}}
\[
\max b_1^TSb_1
\]
{{% /colour %}}

subject to:

{{% colour "green" %}}
\[
b_1^Tb_1 = 1
\]
{{% /colour %}}

The Lagrangian is:

{{% colour "green" %}}
\[
\mathcal{L}(b_1,\lambda)=b_1^TSb_1+\lambda(1-b_1^Tb_1)
\]
{{% /colour %}}

Now set the derivatives to zero:

{{% colour "green" %}}
\[
\frac{\partial \mathcal{L}}{\partial \lambda}=1-b_1^Tb_1=0
\]
{{% /colour %}}

and:

{{% colour "green" %}}
\[
\frac{\partial \mathcal{L}}{\partial b_1}=0
\]
{{% /colour %}}

This gives:

{{% colour "green" %}}
\[
Sb_1=\lambda b_1
\]
{{% /colour %}}

So \(b_1\) is an eigenvector of the covariance matrix \(S\), and \(\lambda\) is the corresponding eigenvalue.

---

## Why the Largest Eigenvalue?

The variance in direction \(b_1\) is:

{{% colour "green" %}}
\[
V_1 = b_1^TSb_1
\]
{{% /colour %}}

Using:

{{% colour "green" %}}
\[
Sb_1=\lambda b_1
\]
{{% /colour %}}

we get:

{{% colour "green" %}}
\[
V_1 = b_1^T\lambda b_1
\]
{{% /colour %}}

Since \(b_1^Tb_1=1\):

{{% colour "green" %}}
\[
V_1 = \lambda
\]
{{% /colour %}}

Therefore, to maximise variance, choose the eigenvector with the largest eigenvalue.

This is the **first principal component**.

---

## More Than One Principal Component

For an \(M\)-dimensional PCA subspace, choose:

{{% colour "green" %}}
\[
B = [b_1,b_2,\ldots,b_M]
\]
{{% /colour %}}

where \(b_1,\ldots,b_M\) are the eigenvectors of \(S\) corresponding to the \(M\) largest eigenvalues.

The projected coordinates are:

{{% colour "green" %}}
\[
z = B^Tx
\]
{{% /colour %}}

The reconstruction is:

{{% colour "green" %}}
\[
\tilde{x}=Bz=BB^Tx
\]
{{% /colour %}}

---

## Variance Explained

If the eigenvalues are:

{{% colour "green" %}}
\[
\lambda_1 \ge \lambda_2 \ge \cdots \ge \lambda_D
\]
{{% /colour %}}

then the variance retained by the first \(M\) components is:

{{% colour "green" %}}
\[
\frac{\lambda_1+\lambda_2+\cdots+\lambda_M}{\lambda_1+\lambda_2+\cdots+\lambda_D}
\]
{{% /colour %}}

This is often called the **explained variance ratio**.

---

## Two Views of PCA

{{< mermaid >}}
flowchart LR
    A[PCA] --> B[Maximum Variance View]
    A --> C[Reconstruction View]
    B --> D[Choose directions with largest variance]
    C --> E[Minimise reconstruction error]
{{< /mermaid >}}


## Projection Perspective

PCA can also be derived as a reconstruction problem.

Instead of asking:

> Which directions maximise variance?

we ask:

> Which lower-dimensional subspace minimises reconstruction error?

The reconstruction error is:

{{% colour "green" %}}
\[
\sum_{n=1}^{N}\|x_n-\tilde{x}_n\|^2
\]
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
\[
S = \frac{1}{N}XX^T
\]
{{% /colour %}}

Assume the singular value decomposition of \(X\) is:

{{% colour "green" %}}
\[
X = U\Sigma V^T
\]
{{% /colour %}}

Then:

{{% colour "green" %}}
\[
S = \frac{1}{N}XX^T
= \frac{1}{N}U\Sigma \Sigma^T U^T
\]
{{% /colour %}}

The columns of \(U\) are the eigenvectors of \(S\).

The eigenvalues of \(S\) are related to the singular values of \(X\):

{{% colour "green" %}}
\[
\lambda_d = \frac{\sigma_d^2}{N}
\]
{{% /colour %}}

This connects PCA with SVD.

---

## PCA as Low-Rank Approximation

PCA can also be understood using the Eckart–Young theorem.

The best rank-\(M\) approximation to \(X\) is obtained by truncating the SVD:

{{% colour "green" %}}
\[
\tilde{X}_M = U_M\Sigma_MV_M^T
\]
{{% /colour %}}

where:

- \(U_M\) contains the top \(M\) left singular vectors
- \(\Sigma_M\) contains the top \(M\) singular values
- \(V_M\) contains the corresponding right singular vectors

This gives the best low-rank approximation to \(X\).

---

## PCA in High Dimensions

In high dimensions, the covariance matrix is:

{{% colour "green" %}}
\[
S \in \mathbb{R}^{D \times D}
\]
{{% /colour %}}

Computing eigenvectors of a \(D \times D\) matrix can be expensive.

This is especially difficult when:

{{% colour "green" %}}
\[
D \gg N
\]
{{% /colour %}}

A useful trick is to use:

{{% colour "green" %}}
\[
X^TX \in \mathbb{R}^{N \times N}
\]
{{% /colour %}}

instead of:

{{% colour "green" %}}
\[
XX^T \in \mathbb{R}^{D \times D}
\]
{{% /colour %}}

The non-zero eigenvalues of \(XX^T\) and \(X^TX\) are the same.

If \(c_m\) is an eigenvector of \(X^TX\), then:

{{% colour "green" %}}
\[
Xc_m
\]
{{% /colour %}}

can be used to recover the corresponding eigenvector of \(XX^T\).

---

## Key Steps of PCA in Practice

The lecture gives the practical PCA pipeline as:

1. Mean subtraction
2. Standardisation
3. Eigendecomposition of the covariance matrix
4. Projection

### Step 1: Mean Subtraction

{{% colour "green" %}}
\[
x_c = x - \mu
\]
{{% /colour %}}

### Step 2: Standardisation

{{% colour "green" %}}
\[
x_s^{(d)}=\frac{x^{(d)}-\mu_d}{\sigma_d}
\]
{{% /colour %}}

### Step 3: Covariance Matrix

{{% colour "green" %}}
\[
S = \frac{1}{N}XX^T
\]
{{% /colour %}}

### Step 4: Eigenvectors and Eigenvalues

Solve:

{{% colour "green" %}}
\[
Sb_i=\lambda_i b_i
\]
{{% /colour %}}

Choose the eigenvectors with the largest eigenvalues.

### Step 5: Projection

{{% colour "green" %}}
\[
z = B^Tx
\]
{{% /colour %}}

### Step 6: Reconstruction

{{% colour "green" %}}
\[
\tilde{x}=Bz
\]
{{% /colour %}}

---

## Exam Method for PCA Numericals

Use this template.

### Given Data Matrix

Write the data matrix \(X\).

### Find the Mean

{{% colour "green" %}}
\[
\mu = \frac{1}{N}\sum_{n=1}^{N}x_n
\]
{{% /colour %}}

### Centre the Data

{{% colour "green" %}}
\[
X_c = X - \mu
\]
{{% /colour %}}

### Compute Covariance

{{% colour "green" %}}
\[
S = \frac{1}{N}X_cX_c^T
\]
{{% /colour %}}

### Find Eigenvalues and Eigenvectors

Solve:

{{% colour "green" %}}
\[
\det(S-\lambda I)=0
\]
{{% /colour %}}

Then find eigenvectors.

### Choose Principal Components

Choose eigenvectors corresponding to largest eigenvalues.

### Project Data

{{% colour "green" %}}
\[
z = B^TX_c
\]
{{% /colour %}}

### Reconstruct If Asked

{{% colour "green" %}}
\[
\tilde{X}=Bz+\mu
\]
{{% /colour %}}

---

## Common Exam Mistakes

| Mistake | Correction |
|---|---|
| Forgetting to centre the data | Always subtract the mean first |
| Choosing smallest eigenvalues | PCA chooses largest eigenvalues |
| Confusing \(B^Tx\) and \(Bx\) | \(z=B^Tx\), reconstruction is \(\tilde{x}=Bz\) |
| Forgetting to add mean back after reconstruction | Use \(\tilde{x}=Bz+\mu\) if returning to original space |
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
