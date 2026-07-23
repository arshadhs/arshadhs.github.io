---
title: "Formula Sheet"
date: 2026-03-12
draft: false
tags: ["AI", "Statistics", "Probability", "Revision"]
categories: ["AI", "Statistics"]
weight: 010
menu: main
---

# Formula Sheet

This page is a quick reference of **definitions + formulas**, grouped by the modules.

---

## Notation

- Sample size: {{< katex >}}n{{< /katex >}} (sample), {{< katex >}}N{{< /katex >}} (population)
- Sample mean: {{< katex >}}\bar{x}{{< /katex >}}, population mean: {{< katex >}}\mu{{< /katex >}}
- Sample variance: {{< katex >}}s^2{{< /katex >}}, population variance: {{< katex >}}\sigma^2{{< /katex >}}
- Sample SD: {{< katex >}}s{{< /katex >}}, population SD: {{< katex >}}\sigma{{< /katex >}}
- Complement: {{< katex >}}A^c{{< /katex >}}
- Intersection (“and”): {{< katex >}}A\cap B{{< /katex >}}, union (“or”): {{< katex >}}A\cup B{{< /katex >}}
- Conditional probability: {{< katex >}}P(A\mid B){{< /katex >}}

---

# 1. Basic Probability & Statistics

## 1.1 Measures of Central Tendency

### Arithmetic mean

Sample mean (ungrouped):

{{< colour "red" >}}
{{< katex display=true >}}
\bar{x}=\frac{1}{n}\sum_{i=1}^{n}x_i
{{< /katex >}}
{{< /colour >}}

Population mean:

{{< colour "red" >}}
{{< katex display=true >}}
\mu=\frac{1}{N}\sum_{i=1}^{N}x_i
{{< /katex >}}
{{< /colour >}}

Grouped data mean (frequency table, {{< katex >}}m_i{{< /katex >}} = class midpoint):

{{< colour "red" >}}
{{< katex display=true >}}
\bar{x}=\frac{\sum_i f_i m_i}{\sum_i f_i}
{{< /katex >}}
{{< /colour >}}

### Median (location)

- If {{< katex >}}n{{< /katex >}} is odd: median is the {{< katex >}}\left(\frac{n+1}{2}\right){{< /katex >}}-th value (after sorting)
- If {{< katex >}}n{{< /katex >}} is even: median is the average of the {{< katex >}}\left(\frac{n}{2}\right){{< /katex >}}-th and {{< katex >}}\left(\frac{n}{2}+1\right){{< /katex >}}-th values

### Mode

- Mode = the value with the highest frequency  
- If multiple values tie for highest frequency → multimodal

### Other means (useful in some datasets)

Geometric mean:

{{< colour "red" >}}
{{< katex display=true >}}
\bar{x}_G=\left(\prod_{i=1}^{n}x_i\right)^{1/n}
{{< /katex >}}
{{< /colour >}}

Weighted mean:

{{< colour "red" >}}
{{< katex display=true >}}
\bar{x}_W=\frac{\sum_i w_i x_i}{\sum_i w_i}
{{< /katex >}}
{{< /colour >}}

Harmonic mean:

{{< colour "red" >}}
{{< katex display=true >}}
\bar{x}_H=\frac{n}{\sum_{i=1}^{n}\frac{1}{x_i}}
{{< /katex >}}
{{< /colour >}}

Root mean square:

{{< colour "red" >}}
{{< katex display=true >}}
x_{rms}=\sqrt{\frac{1}{n}\sum_{i=1}^{n}x_i^2}
{{< /katex >}}
{{< /colour >}}

---

## 1.2 Measures of Variability

### Range

{{< colour "red" >}}
{{< katex display=true >}}
\text{Range}=x_{max}-x_{min}
{{< /katex >}}
{{< /colour >}}

Midrange:

{{< colour "red" >}}
{{< katex display=true >}}
\text{MidRange}=\frac{x_{max}+x_{min}}{2}
{{< /katex >}}
{{< /colour >}}

### Variance and standard deviation

Population variance and SD:

{{< colour "red" >}}
{{< katex display=true >}}
\sigma^2=\frac{\sum_{i=1}^{N}(x_i-\mu)^2}{N},
\qquad
\sigma=\sqrt{\sigma^2}
{{< /katex >}}
{{< /colour >}}

Sample variance and SD:

{{< colour "red" >}}
{{< katex display=true >}}
s^2=\frac{\sum_{i=1}^{n}(x_i-\bar{x})^2}{n-1},
\qquad
s=\sqrt{s^2}
{{< /katex >}}
{{< /colour >}}

### Coefficient of variation (CV)

{{< colour "red" >}}
{{< katex display=true >}}
CV=\frac{\text{SD}}{\text{Mean}}\times 100\%
{{< /katex >}}
{{< /colour >}}

### Covariance

Sample covariance:

{{< colour "red" >}}
{{< katex display=true >}}
s_{xy}=\frac{\sum_{i=1}^{n}(x_i-\bar{x})(y_i-\bar{y})}{n-1}
{{< /katex >}}
{{< /colour >}}

Population covariance:

{{< colour "red" >}}
{{< katex display=true >}}
\sigma_{xy}=\frac{\sum_{i=1}^{N}(x_i-\mu_x)(y_i-\mu_y)}{N}
{{< /katex >}}
{{< /colour >}}

---

## 1.2.1 Five-number summary, IQR, and outliers

Five-number summary:
- minimum
- {{< katex >}}Q_1{{< /katex >}}
- median ({{< katex >}}Q_2{{< /katex >}})
- {{< katex >}}Q_3{{< /katex >}}
- maximum

Interquartile range:

{{< colour "red" >}}
{{< katex display=true >}}
IQR=Q_3-Q_1
{{< /katex >}}
{{< /colour >}}

Quartile deviation:

{{< colour "red" >}}
{{< katex display=true >}}
QD=\frac{Q_3-Q_1}{2}=\frac{IQR}{2}
{{< /katex >}}
{{< /colour >}}

Coefficient of quartile deviation:

{{< colour "red" >}}
{{< katex display=true >}}
CQD=\frac{Q_3-Q_1}{Q_3+Q_1}
{{< /katex >}}
{{< /colour >}}

Outlier fences (boxplot rule):

{{< colour "red" >}}
{{< katex display=true >}}
\text{Lower fence}=Q_1-1.5\,IQR,
\qquad
\text{Upper fence}=Q_3+1.5\,IQR
{{< /katex >}}
{{< /colour >}}

Rule:
any point outside {{< katex >}}[Q_1-1.5IQR,\;Q_3+1.5IQR]{{< /katex >}} is an outlier.

### Quartiles, percentiles, deciles (position formulas)

Quartile position:

{{< colour "red" >}}
{{< katex display=true >}}
Q_k=\frac{k(n+1)}{4}\text{-th term}
\qquad (k=1,2,3)
{{< /katex >}}
{{< /colour >}}

Percentile position:

{{< colour "red" >}}
{{< katex display=true >}}
P_k=\frac{k(n+1)}{100}\text{-th term}
{{< /katex >}}
{{< /colour >}}

Decile position:

{{< colour "red" >}}
{{< katex display=true >}}
D_k=\frac{k(n+1)}{10}\text{-th term}
{{< /katex >}}
{{< /colour >}}

---

## 1.2.2 Grouped data (frequency table) formulas

Grouped data standard deviation (sample form):

{{< colour "red" >}}
{{< katex display=true >}}
s=\sqrt{\frac{\sum f(m-\bar{x})^2}{n-1}}
{{< /katex >}}
{{< /colour >}}

Grouped data shortcut for variance (sample form):

{{< colour "red" >}}
{{< katex display=true >}}
s^2=\frac{\sum f m^2-\frac{(\sum f m)^2}{n}}{n-1}
{{< /katex >}}
{{< /colour >}}

Quartiles for grouped data (common form):
- {{< katex >}}L{{< /katex >}} = lower class boundary of quartile class  
- {{< katex >}}w{{< /katex >}} = class width  
- {{< katex >}}f{{< /katex >}} = frequency of quartile class  
- {{< katex >}}C{{< /katex >}} = cumulative frequency before quartile class  

First quartile:

{{< colour "red" >}}
{{< katex display=true >}}
Q_1=L+\frac{w}{f}\left(\frac{n}{4}-C\right)
{{< /katex >}}
{{< /colour >}}

Median (second quartile):

{{< colour "red" >}}
{{< katex display=true >}}
Q_2=L+\frac{w}{f}\left(\frac{n}{2}-C\right)
{{< /katex >}}
{{< /colour >}}

Third quartile:

{{< colour "red" >}}
{{< katex display=true >}}
Q_3=L+\frac{w}{f}\left(\frac{3n}{4}-C\right)
{{< /katex >}}
{{< /colour >}}

Mode (grouped):

{{< colour "red" >}}
{{< katex display=true >}}
\text{Mode}=L+h\left(\frac{f_m-f_1}{2f_m-f_1-f_2}\right)
{{< /katex >}}
{{< /colour >}}

---

## 1.3 Basic Probability concepts

## 1.3.1 Axioms and basic rules

Probability bounds:

{{< colour "red" >}}
{{< katex display=true >}}
0\le P(A)\le 1
{{< /katex >}}
{{< /colour >}}

Complement:

{{< colour "red" >}}
{{< katex display=true >}}
P(A^c)=1-P(A)
{{< /katex >}}
{{< /colour >}}

Addition rule (general):

{{< colour "red" >}}
{{< katex display=true >}}
P(A\cup B)=P(A)+P(B)-P(A\cap B)
{{< /katex >}}
{{< /colour >}}

Mutually exclusive events:
if {{< katex >}}A\cap B=\varnothing{{< /katex >}}, then

{{< colour "red" >}}
{{< katex display=true >}}
P(A\cup B)=P(A)+P(B)
{{< /katex >}}
{{< /colour >}}

Independent events:

{{< colour "red" >}}
{{< katex display=true >}}
P(A\cap B)=P(A)\,P(B)
{{< /katex >}}
{{< /colour >}}

---

## 1.3.2 Counting (permutations and combinations)

Combinations:

{{< colour "red" >}}
{{< katex display=true >}}
{N\choose n}=\frac{N!}{n!(N-n)!}
{{< /katex >}}
{{< /colour >}}

Permutations:

{{< colour "red" >}}
{{< katex display=true >}}
{}^NP_n=\frac{N!}{(N-n)!}
{{< /katex >}}
{{< /colour >}}

---

# 2. Conditional Probability & Bayes theorem

## 2.1 Conditional probability

{{< colour "red" >}}
{{< katex display=true >}}
P(A\mid B)=\frac{P(A\cap B)}{P(B)},
\qquad P(B)>0
{{< /katex >}}
{{< /colour >}}

Also:

{{< colour "red" >}}
{{< katex display=true >}}
P(B\mid A)=\frac{P(A\cap B)}{P(A)},
\qquad P(A)>0
{{< /katex >}}
{{< /colour >}}

## 2.2 Multiplication rule (joint probability)

{{< colour "red" >}}
{{< katex display=true >}}
P(A\cap B)=P(A\mid B)P(B)=P(B\mid A)P(A)
{{< /katex >}}
{{< /colour >}}

Chain rule (three events):

{{< colour "red" >}}
{{< katex display=true >}}
P(A\cap B\cap C)=P(A)\,P(B\mid A)\,P(C\mid A\cap B)
{{< /katex >}}
{{< /colour >}}

## 2.2 Conditional probability for independent events

If {{< katex >}}A{{< /katex >}} and {{< katex >}}B{{< /katex >}} are independent:

{{< colour "red" >}}
{{< katex display=true >}}
P(A\mid B)=P(A),
\qquad
P(B\mid A)=P(B)
{{< /katex >}}
{{< /colour >}}

---

## 2.3 Bayes’ theorem

Two-event Bayes:

{{< colour "red" >}}
{{< katex display=true >}}
P(A\mid B)=\frac{P(B\mid A)P(A)}{P(B)}
{{< /katex >}}
{{< /colour >}}

Bayes with partition (hypotheses {{< katex >}}A_1,\dots,A_n{{< /katex >}} are mutually exclusive and exhaustive):

{{< colour "red" >}}
{{< katex display=true >}}
P(A_i\mid B)=\frac{P(A_i)P(B\mid A_i)}{\sum_{j=1}^{n}P(A_j)P(B\mid A_j)}
{{< /katex >}}
{{< /colour >}}

---

# 3. Probability Distributions

## 3.1 Random Variables

Random variable:

{{< colour "red" >}}
{{< katex display=true >}}
X:S\to\mathbb{R}
{{< /katex >}}
{{< /colour >}}

### Discrete PMF

{{< colour "red" >}}
{{< katex display=true >}}
p(x)=P(X=x),\qquad \sum_x p(x)=1
{{< /katex >}}
{{< /colour >}}

### Continuous PDF

{{< colour "red" >}}
{{< katex display=true >}}
f(x)\ge 0,\qquad \int_{-\infty}^{\infty} f(x)\,dx=1
{{< /katex >}}
{{< /colour >}}

Interval probability (continuous):

{{< colour "red" >}}
{{< katex display=true >}}
P(a\le X\le b)=\int_{a}^{b} f(x)\,dx
{{< /katex >}}
{{< /colour >}}

### CDF (both cases)

{{< colour "red" >}}
{{< katex display=true >}}
F(x)=P(X\le x)
{{< /katex >}}
{{< /colour >}}

---

## 3.1.3 Mean, Variance, Co-Variance of Random variables

Expected value (discrete):

{{< colour "red" >}}
{{< katex display=true >}}
E(X)=\sum_x x\,p(x)
{{< /katex >}}
{{< /colour >}}

Expected value (continuous):

{{< colour "red" >}}
{{< katex display=true >}}
E(X)=\int_{-\infty}^{\infty} x\,f(x)\,dx
{{< /katex >}}
{{< /colour >}}

Variance:

{{< colour "red" >}}
{{< katex display=true >}}
\operatorname{Var}(X)=E[(X-\mu)^2]=E(X^2)-[E(X)]^2
{{< /katex >}}
{{< /colour >}}

Covariance:

{{< colour "red" >}}
{{< katex display=true >}}
\operatorname{Cov}(X,Y)=E(XY)-E(X)E(Y)
{{< /katex >}}
{{< /colour >}}

---

## 3.2 Probability Distributions

## 3.2.1 Bernoulli Distribution

If {{< katex >}}X\sim \text{Bernoulli}(p){{< /katex >}}:

{{< colour "red" >}}
{{< katex display=true >}}
P(X=1)=p,\qquad P(X=0)=1-p
{{< /katex >}}
{{< /colour >}}

{{< colour "red" >}}
{{< katex display=true >}}
E(X)=p,\qquad \operatorname{Var}(X)=p(1-p)
{{< /katex >}}
{{< /colour >}}

---

## 3.2.2 Binomial Distribution

If {{< katex >}}X\sim \text{Binomial}(n,p){{< /katex >}}:

{{< colour "red" >}}
{{< katex display=true >}}
P(X=x)=\binom{n}{x}p^x(1-p)^{n-x},\qquad x=0,1,\dots,n
{{< /katex >}}
{{< /colour >}}

Mean and variance:

{{< colour "red" >}}
{{< katex display=true >}}
E(X)=np,\qquad \operatorname{Var}(X)=np(1-p)
{{< /katex >}}
{{< /colour >}}

---

## 3.2.3 Poisson Distribution

If {{< katex >}}X\sim \text{Poisson}(\lambda){{< /katex >}}:

{{< colour "red" >}}
{{< katex display=true >}}
P(X=x)=\frac{\lambda^x e^{-\lambda}}{x!},\qquad x=0,1,2,\dots
{{< /katex >}}
{{< /colour >}}

Mean and variance:

{{< colour "red" >}}
{{< katex display=true >}}
E(X)=\lambda,\qquad \operatorname{Var}(X)=\lambda
{{< /katex >}}
{{< /colour >}}

---

## 3.2.4 Normal (Gaussian) Distribution

If {{< katex >}}X\sim N(\mu,\sigma^2){{< /katex >}}:

{{< colour "red" >}}
{{< katex display=true >}}
f(x)=\frac{1}{\sigma\sqrt{2\pi}}\exp\left(-\frac{(x-\mu)^2}{2\sigma^2}\right)
{{< /katex >}}
{{< /colour >}}

Standardisation:

{{< colour "red" >}}
{{< katex display=true >}}
Z=\frac{X-\mu}{\sigma}\sim N(0,1)
{{< /katex >}}
{{< /colour >}}

Z conversion:

{{< colour "red" >}}
{{< katex display=true >}}
z=\frac{x-\mu}{\sigma},
\qquad
x=\mu+z\sigma
{{< /katex >}}
{{< /colour >}}

---

## 3.2.5 t, Chi-square, F (intro)

This course introduces these distributions.

Quick reminders:
- {{< katex >}}t{{< /katex >}} distribution: mean-based inference when {{< katex >}}\sigma{{< /katex >}} is unknown (small-sample setting)
- {{< katex >}}\chi^2{{< /katex >}} distribution: variance-related inference and goodness-of-fit
- {{< katex >}}F{{< /katex >}} distribution: ratio of variances; used in ANOVA

---

# 4. Hypothesis Testing

## 4.1 Sampling and sampling distributions

Standard error of the mean:

{{< colour "red" >}}
{{< katex display=true >}}
\sigma_{\bar{x}}=\frac{\sigma}{\sqrt{n}}
{{< /katex >}}
{{< /colour >}}

---

## 4.2 Central Limit Theorem (core identities)

Mean and SD of sampling distribution of sample mean:

{{< colour "red" >}}
{{< katex display=true >}}
E(\bar{x})=\mu,
\qquad
\sigma_{\bar{x}}=\frac{\sigma}{\sqrt{n}}
{{< /katex >}}
{{< /colour >}}

---

## 4.3 Estimation (confidence intervals)

CI for population mean (sigma known):

{{< colour "red" >}}
{{< katex display=true >}}
\mu\in \bar{x}\pm Z_{\alpha/2}\frac{\sigma}{\sqrt{n}}
{{< /katex >}}
{{< /colour >}}

CI for population proportion:

{{< colour "red" >}}
{{< katex display=true >}}
p\in \hat{p}\pm Z_{\alpha/2}\sqrt{\frac{\hat{p}(1-\hat{p})}{n}}
{{< /katex >}}
{{< /colour >}}

---

## 4.4 Testing of hypothesis

Mean based (one-sample Z form):

{{< colour "red" >}}
{{< katex display=true >}}
Z=\frac{\bar{x}-\mu_0}{\sigma/\sqrt{n}}
{{< /katex >}}
{{< /colour >}}

Proportion related (one proportion Z form):

{{< colour "red" >}}
{{< katex display=true >}}
Z=\frac{\hat{p}-p_0}{\sqrt{\frac{p_0(1-p_0)}{n}}}
{{< /katex >}}
{{< /colour >}}

---

## 4.5 Maximum likelihood (MLE)

Likelihood (IID):

{{< colour "red" >}}
{{< katex display=true >}}
L(\theta)=\prod_{i=1}^{n} f(x_i\mid \theta)
{{< /katex >}}
{{< /colour >}}

Log-likelihood:

{{< colour "red" >}}
{{< katex display=true >}}
\ell(\theta)=\sum_{i=1}^{n}\log f(x_i\mid \theta)
{{< /katex >}}
{{< /colour >}}

MLE:

{{< colour "red" >}}
{{< katex display=true >}}
\hat{\theta}_{MLE}=\arg\max_{\theta} L(\theta)=\arg\max_{\theta}\ell(\theta)
{{< /katex >}}
{{< /colour >}}

---

# Extras (useful in practice)

These formulas appear in the provided sheets and are useful for extra practice.

## Uniform distribution

If {{< katex >}}X\sim U(a,b){{< /katex >}}:

{{< colour "red" >}}
{{< katex display=true >}}
f(x)=\frac{1}{b-a},\qquad a\le x\le b
{{< /katex >}}
{{< /colour >}}

{{< colour "red" >}}
{{< katex display=true >}}
P(c\le X\le d)=\frac{d-c}{b-a}
{{< /katex >}}
{{< /colour >}}

{{< colour "red" >}}
{{< katex display=true >}}
E(X)=\frac{a+b}{2},\qquad \sigma=\frac{b-a}{\sqrt{12}}
{{< /katex >}}
{{< /colour >}}

## Exponential distribution

If {{< katex >}}X\sim \text{Exponential}(\lambda){{< /katex >}}:

{{< colour "red" >}}
{{< katex display=true >}}
f(x)=\lambda e^{-\lambda x},\qquad x\ge 0
{{< /katex >}}
{{< /colour >}}

{{< colour "red" >}}
{{< katex display=true >}}
P(X\le n)=1-e^{-\lambda n},\qquad P(X\ge n)=e^{-\lambda n}
{{< /katex >}}
{{< /colour >}}

{{< colour "red" >}}
{{< katex display=true >}}
E(X)=\frac{1}{\lambda},\qquad \operatorname{Var}(X)=\frac{1}{\lambda^2}
{{< /katex >}}
{{< /colour >}}

---

{{< home-link "Home" >}} | {{< section-index >}}

---
