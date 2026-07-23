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
**Key takeaway:** Linear SVM maximises the margin by minimising {{< katex >}}\frac{1}{2}\|w\|^2{{< /katex >}} subject to correct-classification constraints.
The dual solution expresses {{< katex >}}w{{< /katex >}} as a weighted sum of support vectors:
{{% colour "green" %}}
{{< katex display=true >}}
w=\sum_i\alpha_i y_i x_i
{{< /katex >}}
{{% /colour %}}
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
    style A fill:#E1F5FE,stroke:#78909C,stroke-width:1px,color:#263238
    style B fill:#C8E6C9,stroke:#78909C,stroke-width:1px,color:#263238
    style C fill:#FFF9C4,stroke:#78909C,stroke-width:1px,color:#263238
    style D fill:#EDE7F6,stroke:#78909C,stroke-width:1px,color:#263238
    style E fill:#E1F5FE,stroke:#78909C,stroke-width:1px,color:#263238
    style F fill:#C8E6C9,stroke:#78909C,stroke-width:1px,color:#263238
{{< /mermaid >}}


## Binary Classification Setup

Training data:

{{% colour "green" %}}
{{< katex display=true >}}
(x_1,y_1),(x_2,y_2),\ldots,(x_n,y_n)
{{< /katex >}}
{{% /colour %}}

where:

{{% colour "green" %}}
{{< katex display=true >}}
x_i\in\mathbb{R}^d
{{< /katex >}}
{{% /colour %}}

and labels are:

{{% colour "green" %}}
{{< katex display=true >}}
y_i\in\{-1,+1\}
{{< /katex >}}
{{% /colour %}}

A linear classifier uses:

{{% colour "green" %}}
{{< katex display=true >}}
f(x)=w^Tx+b
{{< /katex >}}
{{% /colour %}}

Prediction:

{{% colour "green" %}}
{{< katex display=true >}}
\hat{y}=\operatorname{sign}(w^Tx+b)
{{< /katex >}}
{{% /colour %}}

---

## Separating Hyperplane

The decision boundary is:

{{% colour "green" %}}
{{< katex display=true >}}
w^Tx+b=0
{{< /katex >}}
{{% /colour %}}

The two margin boundaries are:

{{% colour "green" %}}
{{< katex display=true >}}
w^Tx+b=+1
{{< /katex >}}
{{% /colour %}}

and:

{{% colour "green" %}}
{{< katex display=true >}}
w^Tx+b=-1
{{< /katex >}}
{{% /colour %}}

The margin width is:

{{% colour "green" %}}
{{< katex display=true >}}
\frac{2}{\|w\|}
{{< /katex >}}
{{% /colour %}}

To maximise the margin, we minimise {{< katex >}}\|w\|{{< /katex >}}, equivalently:

{{% colour "green" %}}
{{< katex display=true >}}
\frac{1}{2}\|w\|^2
{{< /katex >}}
{{% /colour %}}

---

## Hard-Margin SVM Primal Problem

For linearly separable data, the hard-margin SVM primal problem is:

{{% colour "green" %}}
{{< katex display=true >}}
\min_{w,b}\frac{1}{2}\|w\|^2
{{< /katex >}}
{{% /colour %}}

subject to:

{{% colour "green" %}}
{{< katex display=true >}}
y_i(w^Tx_i+b)\ge1,\quad i=1,\ldots,n
{{< /katex >}}
{{% /colour %}}

This single constraint covers both classes:

For {{< katex >}}y_i=+1{{< /katex >}}:

{{% colour "green" %}}
{{< katex display=true >}}
w^Tx_i+b\ge1
{{< /katex >}}
{{% /colour %}}

For {{< katex >}}y_i=-1{{< /katex >}}:

{{% colour "green" %}}
{{< katex display=true >}}
w^Tx_i+b\le-1
{{< /katex >}}
{{% /colour %}}

---

## Why This Is a Quadratic Optimisation Problem

The objective is quadratic:

{{% colour "green" %}}
{{< katex display=true >}}
\frac{1}{2}\|w\|^2
{{< /katex >}}
{{% /colour %}}

The constraints are linear in {{< katex >}}w{{< /katex >}} and {{< katex >}}b{{< /katex >}}:

{{% colour "green" %}}
{{< katex display=true >}}
y_i(w^Tx_i+b)\ge1
{{< /katex >}}
{{% /colour %}}

Therefore, linear SVM is a quadratic programming problem with linear inequality constraints.

---

## SVM Lagrangian

Rewrite the constraints as:

{{% colour "green" %}}
{{< katex display=true >}}
1-y_i(w^Tx_i+b)\le0
{{< /katex >}}
{{% /colour %}}

Introduce Lagrange multipliers:

{{% colour "green" %}}
{{< katex display=true >}}
\alpha_i\ge0
{{< /katex >}}
{{% /colour %}}

The Lagrangian is:

{{% colour "green" %}}
{{< katex display=true >}}
\mathcal{L}(w,b,\alpha)
=
\frac{1}{2}\|w\|^2
-\sum_{i=1}^{n}\alpha_i[y_i(w^Tx_i+b)-1]
{{< /katex >}}
{{% /colour %}}

Equivalent expanded form:

{{% colour "green" %}}
{{< katex display=true >}}
\mathcal{L}(w,b,\alpha)
=
\frac{1}{2}\|w\|^2
-\sum_i\alpha_i y_i w^Tx_i
-\sum_i\alpha_i y_i b
+\sum_i\alpha_i
{{< /katex >}}
{{% /colour %}}

---

## Stationarity with Respect to {{< katex >}}w{{< /katex >}}

Set:

{{% colour "green" %}}
{{< katex display=true >}}
\frac{\partial \mathcal{L}}{\partial w}=0
{{< /katex >}}
{{% /colour %}}

This gives:

{{% colour "green" %}}
{{< katex display=true >}}
w-\sum_i\alpha_i y_i x_i=0
{{< /katex >}}
{{% /colour %}}

Therefore:

{{% colour "green" %}}
{{< katex display=true >}}
w=\sum_i\alpha_i y_i x_i
{{< /katex >}}
{{% /colour %}}

This is a crucial SVM result.

The weight vector is a weighted sum of training examples.

---

## Stationarity with Respect to {{< katex >}}b{{< /katex >}}

Set:

{{% colour "green" %}}
{{< katex display=true >}}
\frac{\partial \mathcal{L}}{\partial b}=0
{{< /katex >}}
{{% /colour %}}

This gives:

{{% colour "green" %}}
{{< katex display=true >}}
\sum_i\alpha_i y_i=0
{{< /katex >}}
{{% /colour %}}

This becomes one of the dual constraints.

---

## Dual Objective

Substitute:

{{% colour "green" %}}
{{< katex display=true >}}
w=\sum_i\alpha_i y_i x_i
{{< /katex >}}
{{% /colour %}}

and:

{{% colour "green" %}}
{{< katex display=true >}}
\sum_i\alpha_i y_i=0
{{< /katex >}}
{{% /colour %}}

into the Lagrangian.

The dual problem becomes:

{{% colour "green" %}}
{{< katex display=true >}}
\max_{\alpha}
\sum_i\alpha_i
-
\frac{1}{2}\sum_i\sum_j\alpha_i\alpha_jy_iy_jx_i^Tx_j
{{< /katex >}}
{{% /colour %}}

subject to:

{{% colour "green" %}}
{{< katex display=true >}}
\alpha_i\ge0
{{< /katex >}}
{{% /colour %}}

and:

{{% colour "green" %}}
{{< katex display=true >}}
\sum_i\alpha_i y_i=0
{{< /katex >}}
{{% /colour %}}

---

## Important Observation

The dual problem depends on the data only through inner products:

{{% colour "green" %}}
{{< katex display=true >}}
x_i^Tx_j
{{< /katex >}}
{{% /colour %}}

This is why kernels can be used later.

In nonlinear SVM, we replace:

{{% colour "green" %}}
{{< katex display=true >}}
x_i^Tx_j
{{< /katex >}}
{{% /colour %}}

with:

{{% colour "green" %}}
{{< katex display=true >}}
K(x_i,x_j)
{{< /katex >}}
{{% /colour %}}

---

## KKT Complementary Slackness for SVM

The SVM complementary slackness condition is:

{{% colour "green" %}}
{{< katex display=true >}}
\alpha_i[y_i(w^Tx_i+b)-1]=0
{{< /katex >}}
{{% /colour %}}

This means:

| Case | Meaning |
|---|---|
| {{< katex >}}\alpha_i=0{{< /katex >}} | point is not a support vector |
| {{< katex >}}\alpha_i>0{{< /katex >}} | point lies on the margin |
| {{< katex >}}y_i(w^Tx_i+b)=1{{< /katex >}} | constraint is active |

Support vectors are the points for which:

{{% colour "green" %}}
{{< katex display=true >}}
y_i(w^Tx_i+b)=1
{{< /katex >}}
{{% /colour %}}

---

## Support Vectors

Support vectors are the training points closest to the separating hyperplane.

They lie on:

{{% colour "green" %}}
{{< katex display=true >}}
w^Tx+b=+1
{{< /katex >}}
{{% /colour %}}

or:

{{% colour "green" %}}
{{< katex display=true >}}
w^Tx+b=-1
{{< /katex >}}
{{% /colour %}}

They determine the maximum-margin hyperplane.

If a non-support vector is removed, the hyperplane usually does not change.
If a support vector is removed, the hyperplane may change.

---

## Computing {{< katex >}}b{{< /katex >}}

For any support vector:

{{% colour "green" %}}
{{< katex display=true >}}
y_i(w^Tx_i+b)=1
{{< /katex >}}
{{% /colour %}}

So:

{{% colour "green" %}}
{{< katex display=true >}}
w^Tx_i+b=y_i
{{< /katex >}}
{{% /colour %}}

because {{< katex >}}y_i\in\{-1,+1\}{{< /katex >}}.

Therefore:

{{% colour "green" %}}
{{< katex display=true >}}
b=y_i-w^Tx_i
{{< /katex >}}
{{% /colour %}}

In practice, if there are multiple support vectors, average the computed {{< katex >}}b{{< /katex >}} values.

---

## Classification Function

Using:

{{% colour "green" %}}
{{< katex display=true >}}
w=\sum_i\alpha_i y_i x_i
{{< /katex >}}
{{% /colour %}}

the classifier becomes:

{{% colour "green" %}}
{{< katex display=true >}}
f(x)=\operatorname{sign}(w^Tx+b)
{{< /katex >}}
{{% /colour %}}

Substitute {{< katex >}}w{{< /katex >}}:

{{% colour "green" %}}
{{< katex display=true >}}
f(x)=
\operatorname{sign}
\left(
\sum_i\alpha_i y_i x_i^Tx+b
\right)
{{< /katex >}}
{{% /colour %}}

Only support vectors have {{< katex >}}\alpha_i>0{{< /katex >}}, so only support vectors contribute to the classifier.

---

## Exam Template: Linear SVM from Support Vectors

If a question gives support vectors and labels, do this:

### Step 1: Write Support Vector Equations

For positive support vector:

{{% colour "green" %}}
{{< katex display=true >}}
w^Tx_i+b=1
{{< /katex >}}
{{% /colour %}}

For negative support vector:

{{% colour "green" %}}
{{< katex display=true >}}
w^Tx_i+b=-1
{{< /katex >}}
{{% /colour %}}

### Step 2: Solve for {{< katex >}}w{{< /katex >}} and {{< katex >}}b{{< /katex >}}

If {{< katex >}}w=[w_1,w_2]^T{{< /katex >}}, then:

{{% colour "green" %}}
{{< katex display=true >}}
w^Tx_i+b=w_1x_{i1}+w_2x_{i2}+b
{{< /katex >}}
{{% /colour %}}

Use simultaneous equations.

### Step 3: Write Decision Boundary

{{% colour "green" %}}
{{< katex display=true >}}
w^Tx+b=0
{{< /katex >}}
{{% /colour %}}

### Step 4: Classify New Point

{{% colour "green" %}}
{{< katex display=true >}}
\hat{y}=\operatorname{sign}(w^Tx+b)
{{< /katex >}}
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
    style A fill:#E1F5FE,stroke:#78909C,stroke-width:1px,color:#263238
    style B fill:#C8E6C9,stroke:#78909C,stroke-width:1px,color:#263238
    style C fill:#FFF9C4,stroke:#78909C,stroke-width:1px,color:#263238
    style D fill:#EDE7F6,stroke:#78909C,stroke-width:1px,color:#263238
    style E fill:#E1F5FE,stroke:#78909C,stroke-width:1px,color:#263238
    style F fill:#C8E6C9,stroke:#78909C,stroke-width:1px,color:#263238
{{< /mermaid >}}

{{% hint info %}}
In formulas, the slack variable is written as {{< katex >}}\xi_i{{< /katex >}}. In the diagram above, it is shown as `xi_i` to keep Mermaid rendering simple.
{{% /hint %}}


## Soft-Margin SVM

Hard-margin SVM requires all training points to be correctly classified.

For noisy data, this may be impossible or undesirable.

Soft-margin SVM introduces slack variables:

{{% colour "green" %}}
{{< katex display=true >}}
\xi_i\ge0
{{< /katex >}}
{{% /colour %}}

The soft-margin primal problem is:

{{% colour "green" %}}
{{< katex display=true >}}
\min_{w,b,\xi}
\frac{1}{2}\|w\|^2+C\sum_{i=1}^{n}\xi_i
{{< /katex >}}
{{% /colour %}}

subject to:

{{% colour "green" %}}
{{< katex display=true >}}
y_i(w^Tx_i+b)\ge1-\xi_i
{{< /katex >}}
{{% /colour %}}

and:

{{% colour "green" %}}
{{< katex display=true >}}
\xi_i\ge0
{{< /katex >}}
{{% /colour %}}

---

## Meaning of {{< katex >}}C{{< /katex >}}

The parameter {{< katex >}}C{{< /katex >}} controls the penalty for margin violations.

| {{< katex >}}C{{< /katex >}} value | Effect |
|---|---|
| Large {{< katex >}}C{{< /katex >}} | stronger penalty for misclassification; smaller margin may be chosen |
| Small {{< katex >}}C{{< /katex >}} | more tolerance for misclassification; larger margin may be chosen |

So {{< katex >}}C{{< /katex >}} controls the trade-off between:

- large margin
- training error penalty

---

## Hinge Loss

The hinge loss is:

{{% colour "green" %}}
{{< katex display=true >}}
L_i=\max(0,1-y_i(w^Tx_i+b))
{{< /katex >}}
{{% /colour %}}

If:

{{% colour "green" %}}
{{< katex display=true >}}
y_i(w^Tx_i+b)\ge1
{{< /katex >}}
{{% /colour %}}

then:

{{% colour "green" %}}
{{< katex display=true >}}
L_i=0
{{< /katex >}}
{{% /colour %}}

The point is correctly classified with enough margin.

If:

{{% colour "green" %}}
{{< katex display=true >}}
y_i(w^Tx_i+b)<1
{{< /katex >}}
{{% /colour %}}

then the point has positive hinge loss.

---

## Soft-Margin Objective Using Hinge Loss

The objective can be written as:

{{% colour "green" %}}
{{< katex display=true >}}
J(w,b)=
\frac{1}{2}\|w\|^2+
C\sum_{i=1}^{n}
\max(0,1-y_i(w^Tx_i+b))
{{< /katex >}}
{{% /colour %}}

This is the form often used in gradient-based SVM training.

---

## Hinge Loss Numerical Method

For each example:

### Step 1: Compute Score

{{% colour "green" %}}
{{< katex display=true >}}
s=w^Tx+b
{{< /katex >}}
{{% /colour %}}

### Step 2: Multiply by Label

{{% colour "green" %}}
{{< katex display=true >}}
ys=y(w^Tx+b)
{{< /katex >}}
{{% /colour %}}

### Step 3: Apply Hinge Loss

{{% colour "green" %}}
{{< katex display=true >}}
L=\max(0,1-ys)
{{< /katex >}}
{{% /colour %}}

---

## Quick Example

Suppose:

{{% colour "green" %}}
{{< katex display=true >}}
y=+1,\quad w^Tx+b=0.5
{{< /katex >}}
{{% /colour %}}

Then:

{{% colour "green" %}}
{{< katex display=true >}}
L=\max(0,1-(1)(0.5))=0.5
{{< /katex >}}
{{% /colour %}}

Suppose:

{{% colour "green" %}}
{{< katex display=true >}}
y=-1,\quad w^Tx+b=0.5
{{< /katex >}}
{{% /colour %}}

Then:

{{% colour "green" %}}
{{< katex display=true >}}
L=\max(0,1-(-1)(0.5))=1.5
{{< /katex >}}
{{% /colour %}}

---

## Exam Method: Full Linear SVM Derivation

If asked to derive the dual, write the following sequence.

### 1. Primal

{{% colour "green" %}}
{{< katex display=true >}}
\min_{w,b}\frac{1}{2}\|w\|^2
{{< /katex >}}
{{% /colour %}}

subject to:

{{% colour "green" %}}
{{< katex display=true >}}
y_i(w^Tx_i+b)\ge1
{{< /katex >}}
{{% /colour %}}

### 2. Lagrangian

{{% colour "green" %}}
{{< katex display=true >}}
\mathcal{L}
=
\frac{1}{2}\|w\|^2
-
\sum_i\alpha_i[y_i(w^Tx_i+b)-1]
{{< /katex >}}
{{% /colour %}}

### 3. Stationarity

{{% colour "green" %}}
{{< katex display=true >}}
\frac{\partial \mathcal{L}}{\partial w}=0
\Rightarrow
w=\sum_i\alpha_i y_i x_i
{{< /katex >}}
{{% /colour %}}

{{% colour "green" %}}
{{< katex display=true >}}
\frac{\partial \mathcal{L}}{\partial b}=0
\Rightarrow
\sum_i\alpha_i y_i=0
{{< /katex >}}
{{% /colour %}}

### 4. Dual

{{% colour "green" %}}
{{< katex display=true >}}
\max_{\alpha}
\sum_i\alpha_i
-
\frac{1}{2}
\sum_i\sum_j
\alpha_i\alpha_jy_iy_jx_i^Tx_j
{{< /katex >}}
{{% /colour %}}

subject to:

{{% colour "green" %}}
{{< katex display=true >}}
\alpha_i\ge0,\quad \sum_i\alpha_i y_i=0
{{< /katex >}}
{{% /colour %}}

### 5. Classifier

{{% colour "green" %}}
{{< katex display=true >}}
\hat{y}
=
\operatorname{sign}
\left(
\sum_i\alpha_i y_i x_i^Tx+b
\right)
{{< /katex >}}
{{% /colour %}}

---

## Common Exam Mistakes

| Mistake | Correction |
|---|---|
| Maximising {{< katex >}}\|w\|{{< /katex >}} | Maximise margin {{< katex >}}2/\|w\|{{< /katex >}}, so minimise {{< katex >}}\frac{1}{2}\|w\|^2{{< /katex >}} |
| Forgetting {{< katex >}}y_i{{< /katex >}} in constraint | Use {{< katex >}}y_i(w^Tx_i+b)\ge1{{< /katex >}} |
| Forgetting {{< katex >}}\sum_i\alpha_i y_i=0{{< /katex >}} | This comes from derivative with respect to {{< katex >}}b{{< /katex >}} |
| Thinking all points are support vectors | Only points with {{< katex >}}\alpha_i>0{{< /katex >}} are support vectors |
| Using {{< katex >}}w^Tx+b=0{{< /katex >}} for support vectors | Support vectors lie on {{< katex >}}w^Tx+b=\pm1{{< /katex >}}, not the centre line |

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
