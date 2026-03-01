---
title: "Stats Formula Sheet"
date: 2026-02-25
draft: false
tags: ["AI", "Statistics", "Probability", "Revision"]
categories: ["AI", "Statistics"]
weight: 999
---

# Stats Formula Sheet

Keep this page as a quick reference of **definitions + formulas**.

---

## Notation

- Sample size: $n$ (sample), $N$ (population)
- Mean: $\bar{x}$ (sample), $\mu$ (population)
- Variance: $s^2$ (sample), $\sigma^2$ (population)
- Standard deviation: $s$ (sample), $\sigma$ (population)

---

## Module 1: Basic Statistics

### Measures of Central Tendency

**Sample mean (ungrouped):**

{{% colour %}}
$$
\bar{x}=\frac{1}{n}\sum_{i=1}^{n}x_i
$$
{{% /colour %}}

**Population mean:**

{{% colour %}}
$$
\mu=\frac{1}{N}\sum_{i=1}^{N}x_i
$$
{{% /colour %}}

**Mean (grouped / frequency table):** ($m_i$ = class midpoint)

{{% colour %}}
$$
\bar{x}=\frac{\sum_i f_i m_i}{\sum_i f_i}
$$
{{% /colour %}}

**Median (ungrouped):**
- Odd $n$: middle value after sorting
- Even $n$: average of the two middle values

**Mode (ungrouped):**
- value with the highest frequency

**Empirical relationship (moderately skewed data):**

{{% colour %}}
$$
\text{Mode}\approx 3\,\text{Median}-2\,\text{Mean}
$$
{{% /colour %}}

---

### Measures of Variability

**Range:**

{{% colour %}}
$$
\text{Range}=x_{\max}-x_{\min}
$$
{{% /colour %}}

**Sum of squares (SS):**

{{% colour %}}
$$
SS=\sum_{i=1}^{n}(x_i-\bar{x})^2
$$
{{% /colour %}}

**Population variance and SD:**

{{% colour %}}
$$
\sigma^2=\frac{\sum_{i=1}^{N}(x_i-\mu)^2}{N},\qquad \sigma=\sqrt{\sigma^2}
$$
{{% /colour %}}

**Sample variance and SD:**

{{% colour %}}
$$
s^2=\frac{\sum_{i=1}^{n}(x_i-\bar{x})^2}{n-1}=\frac{SS}{n-1},\qquad s=\sqrt{s^2}
$$
{{% /colour %}}

**Shortcut (sample or population):**

{{% colour %}}
$$
\operatorname{Var}(X)=E(X^2)-[E(X)]^2
$$
{{% /colour %}}

**Coefficient of variation (CV):**

{{% colour %}}
$$
CV=\frac{s}{\bar{x}}\times 100\%
$$
{{% /colour %}}

---

### Five-number summary + IQR + Outliers

**Five-number summary:**  
$\min,\; Q_1,\; Q_2\;(\text{median}),\; Q_3,\; \max$

**Interquartile range (IQR):**

{{% colour %}}
$$
IQR=Q_3-Q_1
$$
{{% /colour %}}

**Quartile deviation (QD):**

{{% colour %}}
$$
QD=\frac{IQR}{2}
$$
{{% /colour %}}

**Outlier fences (boxplot rule):**

{{% colour %}}
$$
\text{Lower fence}=Q_1-1.5\,IQR,\qquad \text{Upper fence}=Q_3+1.5\,IQR
$$
{{% /colour %}}

**Major outlier fences (sometimes used):**

{{% colour %}}
$$
Q_1-3\,IQR,\qquad Q_3+3\,IQR
$$
{{% /colour %}}

---

## Module 1: Basic Probability

### Axioms

{{% colour %}}
$$
P(S)=1,\qquad 0\le P(A)\le 1
$$
{{% /colour %}}

If $A\cap B=\varnothing$ (mutually exclusive):

{{% colour %}}
$$
P(A\cup B)=P(A)+P(B)
$$
{{% /colour %}}

---

### Core rules

**Complement:**

{{% colour %}}
$$
P(A^c)=1-P(A)
$$
{{% /colour %}}

**Addition rule (general):**

{{% colour %}}
$$
P(A\cup B)=P(A)+P(B)-P(A\cap B)
$$
{{% /colour %}}

---

### Conditional probability + multiplication

**Conditional probability:**

{{% colour %}}
$$
P(A\mid B)=\frac{P(A\cap B)}{P(B)}\quad (P(B)>0)
$$
{{% /colour %}}

**Multiplication rule (two events):**

{{% colour %}}
$$
P(A\cap B)=P(A\mid B)P(B)=P(B\mid A)P(A)
$$
{{% /colour %}}

**Multiplication rule (three events):**

{{% colour %}}
$$
P(A\cap B\cap C)=P(A)\,P(B\mid A)\,P(C\mid A\cap B)
$$
{{% /colour %}}

---

### Independence

Events $A$ and $B$ are independent iff:

{{% colour %}}
$$
P(A\cap B)=P(A)P(B)
$$
{{% /colour %}}

Equivalent tests (when defined):

{{% colour %}}
$$
P(A\mid B)=P(A),\qquad P(B\mid A)=P(B)
$$
{{% /colour %}}

---

## Module 2: Total Probability + Bayes’ Theorem

### Total probability

If $E_1,\dots,E_n$ are **mutually exclusive and exhaustive** (a partition of $S$):

{{% colour %}}
$$
P(A)=\sum_{i=1}^{n} P(A\mid E_i)\,P(E_i)
$$
{{% /colour %}}

Special case (two-way split):

{{% colour %}}
$$
P(A)=P(A\mid B)P(B)+P(A\mid B^c)P(B^c)
$$
{{% /colour %}}

---

### Bayes’ theorem (two events)

{{% colour %}}
$$
P(A\mid B)=\frac{P(B\mid A)P(A)}{P(B)}
$$
{{% /colour %}}

---

### Bayes’ theorem (multiple hypotheses)

{{% colour %}}
$$
P(E_i\mid A)=\frac{P(A\mid E_i)P(E_i)}{\sum_{j=1}^{n} P(A\mid E_j)P(E_j)}
$$
{{% /colour %}}

---

## Bayesian Learning (MAP / ML)

Bayes for a hypothesis $h$ and data $D$:

{{% colour %}}
$$
P(h\mid D)=\frac{P(D\mid h)P(h)}{P(D)}
$$
{{% /colour %}}

**MAP hypothesis:**

{{% colour %}}
$$
h_{MAP}=\arg\max_{h\in H} P(h\mid D)=\arg\max_{h\in H} P(D\mid h)P(h)
$$
{{% /colour %}}

**Maximum likelihood (uniform prior):**

{{% colour %}}
$$
h_{ML}=\arg\max_{h\in H} P(D\mid h)
$$
{{% /colour %}}

---

## Naïve Bayes (Classifier)

### Conditional independence assumption

{{% colour %}}
$$
P(X_1,\dots,X_n\mid Y)=\prod_{j=1}^{n} P(X_j\mid Y)
$$
{{% /colour %}}

### Decision rule (classification)

{{% colour %}}
$$
\hat{y}=\arg\max_{y} \; P(y)\prod_{j=1}^{n} P(x_j\mid y)
$$
{{% /colour %}}

### Laplace smoothing (counts / text)

With vocabulary size $|V|$ and smoothing constant $k$ (often 1):

{{% colour %}}
$$
P(w\mid c)=\frac{\operatorname{count}(w,c)+k}{\operatorname{count}(c)+k|V|}
$$
{{% /colour %}}

---

## Module 3: Random Variables

### RV + distribution functions

Random variable (as a function):

{{% colour %}}
$$
X:S\to\mathbb{R}
$$
{{% /colour %}}

**Discrete pmf:**

{{% colour %}}
$$
p(x)=P(X=x),\qquad p(x)\ge 0,\qquad \sum_x p(x)=1
$$
{{% /colour %}}

**Continuous pdf:**

{{% colour %}}
$$
f(x)\ge 0,\qquad \int_{-\infty}^{\infty} f(x)\,dx=1
$$
{{% /colour %}}

Interval probability:

{{% colour %}}
$$
P(a\le X\le b)=\int_{a}^{b} f(x)\,dx
$$
{{% /colour %}}

**CDF (both cases):**

{{% colour %}}
$$
F(x)=P(X\le x)
$$
{{% /colour %}}

---

### Expectation + variance

**Expectation (discrete):**

{{% colour %}}
$$
E(X)=\sum_x x\,p(x)
$$
{{% /colour %}}

**Expectation (continuous):**

{{% colour %}}
$$
E(X)=\int_{-\infty}^{\infty} x\,f(x)\,dx
$$
{{% /colour %}}

**Variance (both cases):**

{{% colour %}}
$$
\operatorname{Var}(X)=E[(X-\mu)^2]=E(X^2)-[E(X)]^2
$$
{{% /colour %}}

**Rules:**

{{% colour %}}
$$
E(aX+b)=aE(X)+b
$$
{{% /colour %}}

{{% colour %}}
$$
\operatorname{Var}(aX+b)=a^2\operatorname{Var}(X)
$$
{{% /colour %}}

---

## Two Random Variables (Joint / Marginal / Conditional)

**Joint pmf (discrete):**

{{% colour %}}
$$
p(x,y)=P(X=x,\,Y=y)
$$
{{% /colour %}}

**Marginals (discrete):**

{{% colour %}}
$$
p_X(x)=\sum_y p(x,y),\qquad p_Y(y)=\sum_x p(x,y)
$$
{{% /colour %}}

**Conditional pmf (discrete):**

{{% colour %}}
$$
p_{Y\mid X}(y\mid x)=\frac{p(x,y)}{p_X(x)}\quad (p_X(x)>0)
$$
{{% /colour %}}

**Independence (discrete):**

{{% colour %}}
$$
X\perp Y\iff p(x,y)=p_X(x)p_Y(y)
$$
{{% /colour %}}

**Covariance + correlation:**

{{% colour %}}
$$
\operatorname{Cov}(X,Y)=E(XY)-E(X)E(Y)
$$
{{% /colour %}}

{{% colour %}}
$$
\rho_{XY}=\frac{\operatorname{Cov}(X,Y)}{\sigma_X\sigma_Y}
$$
{{% /colour %}}

---

## Common Distributions (quick reference)

### Bernoulli($p$)

{{% colour %}}
$$
P(X=1)=p,\quad P(X=0)=1-p
$$
{{% /colour %}}

{{% colour %}}
$$
E(X)=p,\quad \operatorname{Var}(X)=p(1-p)
$$
{{% /colour %}}

### Binomial($n,p$)

{{% colour %}}
$$
P(X=k)=\binom{n}{k}p^k(1-p)^{n-k},\quad k=0,1,\dots,n
$$
{{% /colour %}}

{{% colour %}}
$$
E(X)=np,\quad \operatorname{Var}(X)=np(1-p)
$$
{{% /colour %}}

### Poisson($\lambda$)

{{% colour %}}
$$
P(X=k)=e^{-\lambda}\frac{\lambda^k}{k!},\quad k=0,1,2,\dots
$$
{{% /colour %}}

{{% colour %}}
$$
E(X)=\lambda,\quad \operatorname{Var}(X)=\lambda
$$
{{% /colour %}}

### Normal($\mu,\sigma^2$)

{{% colour %}}
$$
f(x)=\frac{1}{\sqrt{2\pi}\sigma}\exp\left(-\frac{(x-\mu)^2}{2\sigma^2}\right)
$$
{{% /colour %}}

**Standardisation:**

{{% colour %}}
$$
Z=\frac{X-\mu}{\sigma}\sim N(0,1)
$$
{{% /colour %}}

---

{{< home-link "Home" >}} | {{< section-index >}}

---