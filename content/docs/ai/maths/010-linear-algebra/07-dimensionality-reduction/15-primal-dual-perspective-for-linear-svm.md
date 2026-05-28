---
title: "Primal and Dual Perspective for Linear SVM"
date: 2026-05-28
draft: false
weight: 15
tags: ["MFML", "SVM", "Linear SVM", "Primal", "Dual", "Support Vectors", "Hinge Loss"]
categories: ["Mathematical Foundations for Machine Learning"]
---

# Primal and Dual Perspective for Linear SVM

A linear Support Vector Machine finds a hyperplane that separates two classes with the maximum possible margin.

The primal view gives the direct geometric optimisation problem.
The dual view rewrites the problem using Lagrange multipliers and reveals why only support vectors matter.

{{% hint info %}}
**Key takeaway:** Linear SVM maximises the margin by minimising \(\frac{1}{2}\|w\|^2\) subject to correct-classification constraints.
The dual solution expresses \(w\) as a weighted sum of support vectors:
\[
w=\sum_i\alpha_i y_i x_i
\]
{{% /hint %}}

---


## Linear SVM Flow

{{< mermaid >}}
flowchart LR
    A[Training Data] --> B[Primal Problem]
    B --> C[Lagrangian]
    C --> D[Dual Problem]
    D --> E[Support Vectors]
    E --> F[Classifier]
{{< /mermaid >}}


## Binary Classification Setup

Training data:

{{% colour "green" %}}
\[
(x_1,y_1),(x_2,y_2),\ldots,(x_n,y_n)
\]
{{% /colour %}}

where:

{{% colour "green" %}}
\[
x_i\in\mathbb{R}^d
\]
{{% /colour %}}

and labels are:

{{% colour "green" %}}
\[
y_i\in\{-1,+1\}
\]
{{% /colour %}}

A linear classifier uses:

{{% colour "green" %}}
\[
f(x)=w^Tx+b
\]
{{% /colour %}}

Prediction:

{{% colour "green" %}}
\[
\hat{y}=\operatorname{sign}(w^Tx+b)
\]
{{% /colour %}}

---

## Separating Hyperplane

The decision boundary is:

{{% colour "green" %}}
\[
w^Tx+b=0
\]
{{% /colour %}}

The two margin boundaries are:

{{% colour "green" %}}
\[
w^Tx+b=+1
\]
{{% /colour %}}

and:

{{% colour "green" %}}
\[
w^Tx+b=-1
\]
{{% /colour %}}

The margin width is:

{{% colour "green" %}}
\[
\frac{2}{\|w\|}
\]
{{% /colour %}}

To maximise the margin, we minimise \(\|w\|\), equivalently:

{{% colour "green" %}}
\[
\frac{1}{2}\|w\|^2
\]
{{% /colour %}}

---

## Hard-Margin SVM Primal Problem

For linearly separable data, the hard-margin SVM primal problem is:

{{% colour "green" %}}
\[
\min_{w,b}\frac{1}{2}\|w\|^2
\]
{{% /colour %}}

subject to:

{{% colour "green" %}}
\[
y_i(w^Tx_i+b)\ge1,\quad i=1,\ldots,n
\]
{{% /colour %}}

This single constraint covers both classes:

For \(y_i=+1\):

{{% colour "green" %}}
\[
w^Tx_i+b\ge1
\]
{{% /colour %}}

For \(y_i=-1\):

{{% colour "green" %}}
\[
w^Tx_i+b\le-1
\]
{{% /colour %}}

---

## Why This Is a Quadratic Optimisation Problem

The objective is quadratic:

{{% colour "green" %}}
\[
\frac{1}{2}\|w\|^2
\]
{{% /colour %}}

The constraints are linear in \(w\) and \(b\):

{{% colour "green" %}}
\[
y_i(w^Tx_i+b)\ge1
\]
{{% /colour %}}

Therefore, linear SVM is a quadratic programming problem with linear inequality constraints.

---

## SVM Lagrangian

Rewrite the constraints as:

{{% colour "green" %}}
\[
1-y_i(w^Tx_i+b)\le0
\]
{{% /colour %}}

Introduce Lagrange multipliers:

{{% colour "green" %}}
\[
\alpha_i\ge0
\]
{{% /colour %}}

The Lagrangian is:

{{% colour "green" %}}
\[
\mathcal{L}(w,b,\alpha)
=
\frac{1}{2}\|w\|^2
-\sum_{i=1}^{n}\alpha_i[y_i(w^Tx_i+b)-1]
\]
{{% /colour %}}

Equivalent expanded form:

{{% colour "green" %}}
\[
\mathcal{L}(w,b,\alpha)
=
\frac{1}{2}\|w\|^2
-\sum_i\alpha_i y_i w^Tx_i
-\sum_i\alpha_i y_i b
+\sum_i\alpha_i
\]
{{% /colour %}}

---

## Stationarity with Respect to \(w\)

Set:

{{% colour "green" %}}
\[
\frac{\partial \mathcal{L}}{\partial w}=0
\]
{{% /colour %}}

This gives:

{{% colour "green" %}}
\[
w-\sum_i\alpha_i y_i x_i=0
\]
{{% /colour %}}

Therefore:

{{% colour "green" %}}
\[
w=\sum_i\alpha_i y_i x_i
\]
{{% /colour %}}

This is a crucial SVM result.

The weight vector is a weighted sum of training examples.

---

## Stationarity with Respect to \(b\)

Set:

{{% colour "green" %}}
\[
\frac{\partial \mathcal{L}}{\partial b}=0
\]
{{% /colour %}}

This gives:

{{% colour "green" %}}
\[
\sum_i\alpha_i y_i=0
\]
{{% /colour %}}

This becomes one of the dual constraints.

---

## Dual Objective

Substitute:

{{% colour "green" %}}
\[
w=\sum_i\alpha_i y_i x_i
\]
{{% /colour %}}

and:

{{% colour "green" %}}
\[
\sum_i\alpha_i y_i=0
\]
{{% /colour %}}

into the Lagrangian.

The dual problem becomes:

{{% colour "green" %}}
\[
\max_{\alpha}
\sum_i\alpha_i
-
\frac{1}{2}\sum_i\sum_j\alpha_i\alpha_jy_iy_jx_i^Tx_j
\]
{{% /colour %}}

subject to:

{{% colour "green" %}}
\[
\alpha_i\ge0
\]
{{% /colour %}}

and:

{{% colour "green" %}}
\[
\sum_i\alpha_i y_i=0
\]
{{% /colour %}}

---

## Important Observation

The dual problem depends on the data only through inner products:

{{% colour "green" %}}
\[
x_i^Tx_j
\]
{{% /colour %}}

This is why kernels can be used later.

In nonlinear SVM, we replace:

{{% colour "green" %}}
\[
x_i^Tx_j
\]
{{% /colour %}}

with:

{{% colour "green" %}}
\[
K(x_i,x_j)
\]
{{% /colour %}}

---

## KKT Complementary Slackness for SVM

The SVM complementary slackness condition is:

{{% colour "green" %}}
\[
\alpha_i[y_i(w^Tx_i+b)-1]=0
\]
{{% /colour %}}

This means:

| Case | Meaning |
|---|---|
| \(\alpha_i=0\) | point is not a support vector |
| \(\alpha_i>0\) | point lies on the margin |
| \(y_i(w^Tx_i+b)=1\) | constraint is active |

Support vectors are the points for which:

{{% colour "green" %}}
\[
y_i(w^Tx_i+b)=1
\]
{{% /colour %}}

---

## Support Vectors

Support vectors are the training points closest to the separating hyperplane.

They lie on:

{{% colour "green" %}}
\[
w^Tx+b=+1
\]
{{% /colour %}}

or:

{{% colour "green" %}}
\[
w^Tx+b=-1
\]
{{% /colour %}}

They determine the maximum-margin hyperplane.

If a non-support vector is removed, the hyperplane usually does not change.
If a support vector is removed, the hyperplane may change.

---

## Computing \(b\)

For any support vector:

{{% colour "green" %}}
\[
y_i(w^Tx_i+b)=1
\]
{{% /colour %}}

So:

{{% colour "green" %}}
\[
w^Tx_i+b=y_i
\]
{{% /colour %}}

because \(y_i\in\{-1,+1\}\).

Therefore:

{{% colour "green" %}}
\[
b=y_i-w^Tx_i
\]
{{% /colour %}}

In practice, if there are multiple support vectors, average the computed \(b\) values.

---

## Classification Function

Using:

{{% colour "green" %}}
\[
w=\sum_i\alpha_i y_i x_i
\]
{{% /colour %}}

the classifier becomes:

{{% colour "green" %}}
\[
f(x)=\operatorname{sign}(w^Tx+b)
\]
{{% /colour %}}

Substitute \(w\):

{{% colour "green" %}}
\[
f(x)=
\operatorname{sign}
\left(
\sum_i\alpha_i y_i x_i^Tx+b
\right)
\]
{{% /colour %}}

Only support vectors have \(\alpha_i>0\), so only support vectors contribute to the classifier.

---

## Exam Template: Linear SVM from Support Vectors

If a question gives support vectors and labels, do this:

### Step 1: Write Support Vector Equations

For positive support vector:

{{% colour "green" %}}
\[
w^Tx_i+b=1
\]
{{% /colour %}}

For negative support vector:

{{% colour "green" %}}
\[
w^Tx_i+b=-1
\]
{{% /colour %}}

### Step 2: Solve for \(w\) and \(b\)

If \(w=[w_1,w_2]^T\), then:

{{% colour "green" %}}
\[
w^Tx_i+b=w_1x_{i1}+w_2x_{i2}+b
\]
{{% /colour %}}

Use simultaneous equations.

### Step 3: Write Decision Boundary

{{% colour "green" %}}
\[
w^Tx+b=0
\]
{{% /colour %}}

### Step 4: Classify New Point

{{% colour "green" %}}
\[
\hat{y}=\operatorname{sign}(w^Tx+b)
\]
{{% /colour %}}

---

## Hard vs Soft Margin

{{< mermaid >}}
flowchart LR
    A[SVM] --> B[Hard Margin]
    A --> C[Soft Margin]
    B --> D[No misclassification allowed]
    C --> E[Slack variables xi_i]
    C --> F[Penalty parameter C]
{{< /mermaid >}}

{{% hint info %}}
In formulas, the slack variable is written as {{< katex >}}\xi_i{{< /katex >}}. In the diagram above, it is shown as `xi_i` to keep Mermaid rendering simple.
{{% /hint %}}


## Soft-Margin SVM

Hard-margin SVM requires all training points to be correctly classified.

For noisy data, this may be impossible or undesirable.

Soft-margin SVM introduces slack variables:

{{% colour "green" %}}
\[
\xi_i\ge0
\]
{{% /colour %}}

The soft-margin primal problem is:

{{% colour "green" %}}
\[
\min_{w,b,\xi}
\frac{1}{2}\|w\|^2+C\sum_{i=1}^{n}\xi_i
\]
{{% /colour %}}

subject to:

{{% colour "green" %}}
\[
y_i(w^Tx_i+b)\ge1-\xi_i
\]
{{% /colour %}}

and:

{{% colour "green" %}}
\[
\xi_i\ge0
\]
{{% /colour %}}

---

## Meaning of \(C\)

The parameter \(C\) controls the penalty for margin violations.

| \(C\) value | Effect |
|---|---|
| Large \(C\) | stronger penalty for misclassification; smaller margin may be chosen |
| Small \(C\) | more tolerance for misclassification; larger margin may be chosen |

So \(C\) controls the trade-off between:

- large margin
- training error penalty

---

## Hinge Loss

The hinge loss is:

{{% colour "green" %}}
\[
L_i=\max(0,1-y_i(w^Tx_i+b))
\]
{{% /colour %}}

If:

{{% colour "green" %}}
\[
y_i(w^Tx_i+b)\ge1
\]
{{% /colour %}}

then:

{{% colour "green" %}}
\[
L_i=0
\]
{{% /colour %}}

The point is correctly classified with enough margin.

If:

{{% colour "green" %}}
\[
y_i(w^Tx_i+b)<1
\]
{{% /colour %}}

then the point has positive hinge loss.

---

## Soft-Margin Objective Using Hinge Loss

The objective can be written as:

{{% colour "green" %}}
\[
J(w,b)=
\frac{1}{2}\|w\|^2+
C\sum_{i=1}^{n}
\max(0,1-y_i(w^Tx_i+b))
\]
{{% /colour %}}

This is the form often used in gradient-based SVM training.

---

## Hinge Loss Numerical Method

For each example:

### Step 1: Compute Score

{{% colour "green" %}}
\[
s=w^Tx+b
\]
{{% /colour %}}

### Step 2: Multiply by Label

{{% colour "green" %}}
\[
ys=y(w^Tx+b)
\]
{{% /colour %}}

### Step 3: Apply Hinge Loss

{{% colour "green" %}}
\[
L=\max(0,1-ys)
\]
{{% /colour %}}

---

## Quick Example

Suppose:

{{% colour "green" %}}
\[
y=+1,\quad w^Tx+b=0.5
\]
{{% /colour %}}

Then:

{{% colour "green" %}}
\[
L=\max(0,1-(1)(0.5))=0.5
\]
{{% /colour %}}

Suppose:

{{% colour "green" %}}
\[
y=-1,\quad w^Tx+b=0.5
\]
{{% /colour %}}

Then:

{{% colour "green" %}}
\[
L=\max(0,1-(-1)(0.5))=1.5
\]
{{% /colour %}}

---

## Exam Method: Full Linear SVM Derivation

If asked to derive the dual, write the following sequence.

### 1. Primal

{{% colour "green" %}}
\[
\min_{w,b}\frac{1}{2}\|w\|^2
\]
{{% /colour %}}

subject to:

{{% colour "green" %}}
\[
y_i(w^Tx_i+b)\ge1
\]
{{% /colour %}}

### 2. Lagrangian

{{% colour "green" %}}
\[
\mathcal{L}
=
\frac{1}{2}\|w\|^2
-
\sum_i\alpha_i[y_i(w^Tx_i+b)-1]
\]
{{% /colour %}}

### 3. Stationarity

{{% colour "green" %}}
\[
\frac{\partial \mathcal{L}}{\partial w}=0
\Rightarrow
w=\sum_i\alpha_i y_i x_i
\]
{{% /colour %}}

{{% colour "green" %}}
\[
\frac{\partial \mathcal{L}}{\partial b}=0
\Rightarrow
\sum_i\alpha_i y_i=0
\]
{{% /colour %}}

### 4. Dual

{{% colour "green" %}}
\[
\max_{\alpha}
\sum_i\alpha_i
-
\frac{1}{2}
\sum_i\sum_j
\alpha_i\alpha_jy_iy_jx_i^Tx_j
\]
{{% /colour %}}

subject to:

{{% colour "green" %}}
\[
\alpha_i\ge0,\quad \sum_i\alpha_i y_i=0
\]
{{% /colour %}}

### 5. Classifier

{{% colour "green" %}}
\[
\hat{y}
=
\operatorname{sign}
\left(
\sum_i\alpha_i y_i x_i^Tx+b
\right)
\]
{{% /colour %}}

---

## Common Exam Mistakes

| Mistake | Correction |
|---|---|
| Maximising \(\|w\|\) | Maximise margin \(2/\|w\|\), so minimise \(\frac{1}{2}\|w\|^2\) |
| Forgetting \(y_i\) in constraint | Use \(y_i(w^Tx_i+b)\ge1\) |
| Forgetting \(\sum_i\alpha_i y_i=0\) | This comes from derivative with respect to \(b\) |
| Thinking all points are support vectors | Only points with \(\alpha_i>0\) are support vectors |
| Using \(w^Tx+b=0\) for support vectors | Support vectors lie on \(w^Tx+b=\pm1\), not the centre line |

---

## Quick Memory Line

```text
Max margin → minimise ||w||² → constraints → Lagrangian → α → support vectors → classifier
```

---

## Source Focus

This page is based mainly on:

- Lecture 15: solving the SVM optimisation problem, Lagrangian, support vectors, linear SVM numerical method, hinge loss and soft margin
- Lecture 16: maximum margin geometry and hard-margin linear SVM formulation
- Lecture 14: KKT and hyperplane preliminaries
- Course handout: session 15 and SVM practical lab outcomes

---

{{< home-link "Home" >}} | {{< section-index >}}

---
