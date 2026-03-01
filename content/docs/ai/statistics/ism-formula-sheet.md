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

- Sample size: {{< katex >}}n{{< /katex >}} (sample), {{< katex >}}N{{< /katex >}} (population)
- Mean: {{< katex >}}\bar{x}{{< /katex >}} (sample), {{< katex >}}\mu{{< /katex >}} (population)
- Variance: {{< katex >}}s^2{{< /katex >}} (sample), {{< katex >}}\sigma^2{{< /katex >}} (population)
- Standard deviation: {{< katex >}}s{{< /katex >}} (sample), {{< katex >}}\sigma{{< /katex >}} (population)

---

## Module 1: Basic Statistics

### Measures of Central Tendency

**Sample mean (ungrouped):**

{{% colour %}}
{{< katex display=true >}}
\bar{x}=\frac{1}{n}\sum_{i=1}^{n}x_i
{{< /katex >}}
{{% /colour %}}

**Population mean:**

{{% colour %}}
{{< katex display=true >}}
\mu=\frac{1}{N}\sum_{i=1}^{N}x_i
{{< /katex >}}
{{% /colour %}}

**Mean (grouped / frequency table):**
$m_i$ = class midpoint.

{{% colour %}}
{{< katex display=true >}}
\bar{x}=\frac{\sum_i f_i m_i}{\sum_i f_i}
{{< /katex >}}
{{% /colour %}}

**Median (ungrouped):**
- Odd \(n\): middle value after sorting
- Even \(n\): average of the two middle values

**Mode (ungrouped):**
- value with the highest frequency

**Empirical relationship (moderately skewed data):**

{{% colour %}}
{{< katex display=true >}}
\text{Mode}\approx 3\,\text{Median}-2\,\text{Mean}
{{< /katex >}}
{{% /colour %}}

---

### Measures of Variability

**Range:**

{{% colour %}}
{{< katex display=true >}}
\text{Range}=x_{\max}-x_{\min}
{{< /katex >}}
{{% /colour %}}

**Sum of squares (SS):**

{{% colour %}}
{{< katex display=true >}}
SS=\sum_{i=1}^{n}(x_i-\bar{x})^2
{{< /katex >}}
{{% /colour %}}

**Population variance and SD:**

{{% colour %}}
{{< katex display=true >}}
\sigma^2=\frac{\sum_{i=1}^{N}(x_i-\mu)^2}{N},\qquad \sigma=\sqrt{\sigma^2}
{{< /katex >}}
{{% /colour %}}

**Sample variance and SD:**

{{% colour %}}
{{< katex display=true >}}
s^2=\frac{\sum_{i=1}^{n}(x_i-\bar{x})^2}{n-1}=\frac{SS}{n-1},\qquad s=\sqrt{s^2}
{{< /katex >}}
{{% /colour %}}

**Shortcut (sample or population):**

{{% colour %}}
{{< katex display=true >}}
\operatorname{Var}(X)=E(X^2)-[E(X)]^2
{{< /katex >}}
{{% /colour %}}

**Coefficient of variation (CV):**

{{% colour %}}
{{< katex display=true >}}
CV=\frac{s}{\bar{x}}\times 100\%
{{< /katex >}}
{{% /colour %}}

---

### Five-number summary, IQR, and Outliers

Five-number summary:
\(\min,\; Q_1,\; Q_2\;(\text{median}),\; Q_3,\; \max\)

**Interquartile range (IQR):**

{{% colour %}}
{{< katex display=true >}}
IQR=Q_3-Q_1
{{< /katex >}}
{{% /colour %}}

**Quartile deviation (QD):**

{{% colour %}}
{{< katex display=true >}}
QD=\frac{IQR}{2}
{{< /katex >}}
{{% /colour %}}

**Outlier fences (boxplot rule):**

{{% colour %}}
{{< katex display=true >}}
\text{Lower fence}=Q_1-1.5\,IQR,\qquad \text{Upper fence}=Q_3+1.5\,IQR
{{< /katex >}}
{{% /colour %}}

**Major outlier fences (sometimes used):**

{{% colour %}}
{{< katex display=true >}}
Q_1-3\,IQR,\qquad Q_3+3\,IQR
{{< /katex >}}
{{% /colour %}}

---

## Module 1: Basic Probability

### Axioms

{{% colour %}}
{{< katex display=true >}}
P(S)=1,\qquad 0\le P(A)\le 1
{{< /katex >}}
{{% /colour %}}

If $A\cap B=\varnothing$ (mutually exclusive):

{{% colour %}}
{{< katex display=true >}}
P(A\cup B)=P(A)+P(B)
{{< /katex >}}
{{% /colour %}}

---

### Core rules

**Complement:**

{{% colour %}}
{{< katex display=true >}}
P(A^c)=1-P(A)
{{< /katex >}}
{{% /colour %}}

**Addition rule (general):**

{{% colour %}}
{{< katex display=true >}}
P(A\cup B)=P(A)+P(B)-P(A\cap B)
{{< /katex >}}
{{% /colour %}}

---

### Conditional probability + multiplication

**Conditional probability:**

{{% colour %}}
{{< katex display=true >}}
P(A\mid B)=\frac{P(A\cap B)}{P(B)}\quad (P(B)>0)
{{< /katex >}}
{{% /colour %}}

**Multiplication rule (two events):**

{{% colour %}}
{{< katex display=true >}}
P(A\cap B)=P(A\mid B)P(B)=P(B\mid A)P(A)
{{< /katex >}}
{{% /colour %}}

**Multiplication rule (three events):**

{{% colour %}}
{{< katex display=true >}}
P(A\cap B\cap C)=P(A)\,P(B\mid A)\,P(C\mid A\cap B)
{{< /katex >}}
{{% /colour %}}

---

### Independence

Events $A$ and $B$ are independent iff:

{{% colour %}}
{{< katex display=true >}}
P(A\cap B)=P(A)P(B)
{{< /katex >}}
{{% /colour %}}

Equivalent tests (when defined):

{{% colour %}}
{{< katex display=true >}}
P(A\mid B)=P(A),\qquad P(B\mid A)=P(B)
{{< /katex >}}
{{% /colour %}}

---

## Module 2: Total Probability + Bayes’ Theorem

### Total probability

If $E_1,\dots,E_n$ are **mutually exclusive and exhaustive** (a partition of $S$):

{{% colour %}}
{{< katex display=true >}}
P(A)=\sum_{i=1}^{n} P(A\mid E_i)\,P(E_i)
{{< /katex >}}
{{% /colour %}}

Special case (two-way split):

{{% colour %}}
{{< katex display=true >}}
P(A)=P(A\mid B)P(B)+P(A\mid B^c)P(B^c)
{{< /katex >}}
{{% /colour %}}

---

### Bayes’ theorem (two events)

{{% colour %}}
{{< katex display=true >}}
P(A\mid B)=\frac{P(B\mid A)P(A)}{P(B)}
{{< /katex >}}
{{% /colour %}}

---

### Bayes’ theorem (multiple hypotheses)

{{% colour %}}
{{< katex display=true >}}
P(E_i\mid A)=\frac{P(A\mid E_i)P(E_i)}{\sum_{j=1}^{n} P(A\mid E_j)P(E_j)}
{{< /katex >}}
{{% /colour %}}

---

## Bayesian Learning

Bayes for a hypothesis $h$ and data $D$:

{{% colour %}}
{{< katex display=true >}}
P(h\mid D)=\frac{P(D\mid h)P(h)}{P(D)}
{{< /katex >}}
{{% /colour %}}

**MAP hypothesis:**

{{% colour %}}
{{< katex display=true >}}
h_{MAP}=\arg\max_{h\in H} P(h\mid D)=\arg\max_{h\in H} P(D\mid h)P(h)
{{< /katex >}}
{{% /colour %}}

**Maximum likelihood (uniform prior):**

{{% colour %}}
{{< katex display=true >}}
h_{ML}=\arg\max_{h\in H} P(D\mid h)
{{< /katex >}}
{{% /colour %}}

---

## Naïve Bayes (Classifier)

### Conditional independence assumption

{{% colour %}}
{{< katex display=true >}}
P(X_1,\dots,X_n\mid Y)=\prod_{j=1}^{n} P(X_j\mid Y)
{{< /katex >}}
{{% /colour %}}

### Decision rule (classification)

{{% colour %}}
{{< katex display=true >}}
\hat{y}=\arg\max_{y} \; P(y)\prod_{j=1}^{n} P(x_j\mid y)
{{< /katex >}}
{{% /colour %}}

### Laplace smoothing (counts / text)

Vocabulary size is $|V|$ and smoothing constant is $k$ (often 1).

{{% colour %}}
{{< katex display=true >}}
P(w\mid c)=\frac{\operatorname{count}(w,c)+k}{\operatorname{count}(c)+k|V|}
{{< /katex >}}
{{% /colour %}}

---

## Module 3: Random Variables

### RV + distribution functions

Random variable (as a function):

{{% colour %}}
{{< katex display=true >}}
X:S\to\mathbb{R}
{{< /katex >}}
{{% /colour %}}

**Discrete pmf:**

{{% colour %}}
{{< katex display=true >}}
p(x)=P(X=x),\qquad p(x)\ge 0,\qquad \sum_x p(x)=1
{{< /katex >}}
{{% /colour %}}

**Continuous pdf:**

{{% colour %}}
{{< katex display=true >}}
f(x)\ge 0,\qquad \int_{-\infty}^{\infty} f(x)\,dx=1
{{< /katex >}}
{{% /colour %}}

Interval probability:

{{% colour %}}
{{< katex display=true >}}
P(a\le X\le b)=\int_{a}^{b} f(x)\,dx
{{< /katex >}}
{{% /colour %}}

**CDF (both cases):**

{{% colour %}}
{{< katex display=true >}}
F(x)=P(X\le x)
{{< /katex >}}
{{% /colour %}}

---

### Expectation and variance

**Expectation (discrete):**

{{% colour %}}
{{< katex display=true >}}
E(X)=\sum_x x\,p(x)
{{< /katex >}}
{{% /colour %}}

**Expectation (continuous):**

{{% colour %}}
{{< katex display=true >}}
E(X)=\int_{-\infty}^{\infty} x\,f(x)\,dx
{{< /katex >}}
{{% /colour %}}

**Variance (both cases):**

{{% colour %}}
{{< katex display=true >}}
\operatorname{Var}(X)=E[(X-\mu)^2]=E(X^2)-[E(X)]^2
{{< /katex >}}
{{% /colour %}}

**Rules:**

{{% colour %}}
{{< katex display=true >}}
E(aX+b)=aE(X)+b
{{< /katex >}}
{{% /colour %}}

{{% colour %}}
{{< katex display=true >}}
\operatorname{Var}(aX+b)=a^2\operatorname{Var}(X)
{{< /katex >}}
{{% /colour %}}

---

## Two Random Variables (Joint / Marginal / Conditional)

**Joint pmf (discrete):**

{{% colour %}}
{{< katex display=true >}}
p(x,y)=P(X=x,\,Y=y)
{{< /katex >}}
{{% /colour %}}

**Marginals (discrete):**

{{% colour %}}
{{< katex display=true >}}
p_X(x)=\sum_y p(x,y),\qquad p_Y(y)=\sum_x p(x,y)
{{< /katex >}}
{{% /colour %}}

**Conditional pmf (discrete):**

{{% colour %}}
{{< katex display=true >}}
p_{Y\mid X}(y\mid x)=\frac{p(x,y)}{p_X(x)}\quad (p_X(x)>0)
{{< /katex >}}
{{% /colour %}}

**Independence (discrete):**

{{% colour %}}
{{< katex display=true >}}
X\perp Y\iff p(x,y)=p_X(x)p_Y(y)
{{< /katex >}}
{{% /colour %}}

**Covariance + correlation:**

{{% colour %}}
{{< katex display=true >}}
\operatorname{Cov}(X,Y)=E(XY)-E(X)E(Y)
{{< /katex >}}
{{% /colour %}}

{{% colour %}}
{{< katex display=true >}}
\rho_{XY}=\frac{\operatorname{Cov}(X,Y)}{\sigma_X\sigma_Y}
{{< /katex >}}
{{% /colour %}}

---

## Common Distributions

### Bernoulli distribution

Parameter:
$p$

{{% colour %}}
{{< katex display=true >}}
P(X=1)=p,\quad P(X=0)=1-p
{{< /katex >}}
{{% /colour %}}

{{% colour %}}
{{< katex display=true >}}
E(X)=p,\quad \operatorname{Var}(X)=p(1-p)
{{< /katex >}}
{{% /colour %}}

### Binomial distribution

Parameters:
$n, p$

{{% colour %}}
{{< katex display=true >}}
P(X=k)=\binom{n}{k}p^k(1-p)^{n-k},\quad k=0,1,\dots,n
{{< /katex >}}
{{% /colour %}}

{{% colour %}}
{{< katex display=true >}}
E(X)=np,\quad \operatorname{Var}(X)=np(1-p)
{{< /katex >}}
{{% /colour %}}

### Poisson distribution

Parameter:
$\lambda$

{{% colour %}}
{{< katex display=true >}}
P(X=k)=e^{-\lambda}\frac{\lambda^k}{k!},\quad k=0,1,2,\dots
{{< /katex >}}
{{% /colour %}}

{{% colour %}}
{{< katex display=true >}}
E(X)=\lambda,\quad \operatorname{Var}(X)=\lambda
{{< /katex >}}
{{% /colour %}}

### Normal distribution

Parameters:
$\mu, \sigma^2$

{{% colour %}}
{{< katex display=true >}}
f(x)=\frac{1}{\sqrt{2\pi}\sigma}\exp\left(-\frac{(x-\mu)^2}{2\sigma^2}\right)
{{< /katex >}}
{{% /colour %}}

**Standardisation:**

{{% colour %}}
{{< katex display=true >}}
Z=\frac{X-\mu}{\sigma}\sim N(0,1)
{{< /katex >}}
{{% /colour %}}

---

{{< home-link "Home" >}} | {{< section-index >}}

---
