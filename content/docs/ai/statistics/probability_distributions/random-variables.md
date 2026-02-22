---
title: "Random Variables"
date: 2026-02-22
draft: false
tags: ["AI", "Statistics", "Probability"]
categories: ["AI", "Statistics"]
weight: 310
menu: main
---

# Random Variables

A random variable is a way to attach numbers to outcomes of a random experiment.

It lets us move from:
“what happened?”
to:
“what number should we analyse?”

{{% hint info %}}
Key takeaway:
A random variable is a *function* from the sample space to real numbers.
Once you define the random variable clearly, the rest (pmf/pdf/cdf, mean, variance) becomes systematic.
{{% /hint %}}

---

## 1) Definition

Random variable:
a rule that assigns a number to each outcome.

{{% colour %}}
{{< katex display=true >}}
X: S \to \mathbb{R}
{{< /katex >}}
{{% /colour %}}

Where:
- {{< katex >}}S{{< /katex >}} is the sample space
- {{< katex >}}\mathbb{R}{{< /katex >}} is the set of real numbers

---

## 2) Types of random variables

### Discrete random variable

A discrete random variable can take a countable set of values
(e.g., 0, 1, 2, 3, …).

Examples:
- number of heads in 2 coin tosses
- number of customers arriving in an hour
- number of cars in a drive-thru queue

### Continuous random variable

A continuous random variable can take any value in an interval.

Examples:
- time to finish a task
- height of a person
- temperature

{{% hint info %}}
Rule of thumb:
Counts → usually discrete.
Measurements → usually continuous.
{{% /hint %}}

---

## 3) Probability functions: pmf, pdf, cdf

### 3.1 Discrete: probability mass function (pmf)

The pmf gives the probability of each possible value.

{{% colour %}}
{{< katex display=true >}}
p(x)=P(X=x)
{{< /katex >}}
{{% /colour %}}

A valid pmf must satisfy:

{{% colour %}}
{{< katex display=true >}}
p(x)\ge 0 \quad \text{for all }x
{{< /katex >}}
{{% /colour %}}

{{% colour %}}
{{< katex display=true >}}
\sum_x p(x)=1
{{< /katex >}}
{{% /colour %}}

#### Example: number of heads in 2 tosses

Let {{< katex >}}X{{< /katex >}} = number of heads.
Outcomes:
HH, HT, TH, TT.

So:
- {{< katex >}}P(X=0)=1/4{{< /katex >}}
- {{< katex >}}P(X=1)=2/4{{< /katex >}}
- {{< katex >}}P(X=2)=1/4{{< /katex >}}

---

### 3.2 Continuous: probability density function (pdf)

For a continuous random variable, we do not talk about:
{{< katex >}}P(X=c){{< /katex >}} (it is 0).

We talk about intervals.

{{% colour %}}
{{< katex display=true >}}
P(a\le X\le b)=\int_a^b f(x)\,dx
{{< /katex >}}
{{% /colour %}}

A valid pdf must satisfy:

{{% colour %}}
{{< katex display=true >}}
f(x)\ge 0
{{< /katex >}}
{{% /colour %}}

{{% colour %}}
{{< katex display=true >}}
\int_{-\infty}^{\infty} f(x)\,dx=1
{{< /katex >}}
{{% /colour %}}

---

### 3.3 Cumulative distribution function (cdf)

The cdf accumulates probability up to a point.

{{% colour %}}
{{< katex display=true >}}
F(x)=P(X\le x)
{{< /katex >}}
{{% /colour %}}

Discrete case:
it is the running total of pmf values.

Continuous case:
it is the area under the pdf to the left of x.

{{% hint warning %}}
Discrete:
cdf jumps (step-like).
Continuous:
cdf is smooth (usually).
{{% /hint %}}

---

## 4) Mean (Expectation) and Variance

### 4.1 Expectation

Discrete:

{{% colour %}}
{{< katex display=true >}}
E(X)=\sum_x x\,p(x)
{{< /katex >}}
{{% /colour %}}

Continuous:

{{% colour %}}
{{< katex display=true >}}
E(X)=\int_{-\infty}^{\infty} x\,f(x)\,dx
{{< /katex >}}
{{% /colour %}}

Meaning:
the long-run average value of X if you repeat the experiment many times.

---

### 4.2 Variance and standard deviation

Variance measures spread around the mean.

Discrete:

{{% colour %}}
{{< katex display=true >}}
V(X)=\sum_x (x-\mu)^2\,p(x)
{{< /katex >}}
{{% /colour %}}

Continuous:

{{% colour %}}
{{< katex display=true >}}
V(X)=\int_{-\infty}^{\infty} (x-\mu)^2\,f(x)\,dx
{{< /katex >}}
{{% /colour %}}

Shortcut (both discrete and continuous):

{{% colour %}}
{{< katex display=true >}}
V(X)=E(X^2)-[E(X)]^2
{{< /katex >}}
{{% /colour %}}

Standard deviation:

{{% colour %}}
{{< katex display=true >}}
\sigma_X = \sqrt{V(X)}
{{< /katex >}}
{{% /colour %}}

---

## 5) Rules you should memorise

Expected value rules:

{{% colour %}}
{{< katex display=true >}}
E(aX+b)=aE(X)+b
{{< /katex >}}
{{% /colour %}}

Variance rules:

{{% colour %}}
{{< katex display=true >}}
V(aX+b)=a^2V(X)
{{< /katex >}}
{{% /colour %}}

---

## 6) Two random variables: joint, marginal, conditional

### 6.1 Joint distribution (discrete)

{{% colour %}}
{{< katex display=true >}}
p(x,y)=P(X=x,\,Y=y)
{{< /katex >}}
{{% /colour %}}

### 6.2 Marginal distributions

Sum over the other variable:

{{% colour %}}
{{< katex display=true >}}
p_X(x)=\sum_y p(x,y)
{{< /katex >}}
{{% /colour %}}

{{% colour %}}
{{< katex display=true >}}
p_Y(y)=\sum_x p(x,y)
{{< /katex >}}
{{% /colour %}}

### 6.3 Conditional distribution

{{% colour %}}
{{< katex display=true >}}
p_{Y\mid X}(y\mid x)=\frac{p(x,y)}{p_X(x)}\quad (p_X(x)>0)
{{< /katex >}}
{{% /colour %}}

---

## 7) Covariance (relationship between X and Y)

Covariance measures whether two variables move together.

{{% colour %}}
{{< katex display=true >}}
\operatorname{Cov}(X,Y)=E(XY)-E(X)E(Y)
{{< /katex >}}
{{% /colour %}}

- Positive covariance:
large X tends to come with large Y.
- Negative covariance:
large X tends to come with small Y.
- Zero covariance:
no linear relationship (but they could still be dependent).

---

## 8) Transformation of random variables (basic idea)

Sometimes we define a new random variable:
{{< katex >}}Y=g(X){{< /katex >}}.

Discrete:
compute {{< katex >}}P(Y=y){{< /katex >}} by summing probabilities of all {{< katex >}}x{{< /katex >}} values that map to y.

Continuous (monotone g):
use a change-of-variables approach.

{{% hint info %}}
You will use transformations later for:
standardisation, log transforms, and turning data into “nice” distributions.
{{% /hint %}}

---

## Mini-check

1) If X is continuous, what is {{< katex >}}P(X=c){{< /katex >}}?  
2) For a pmf, what must {{< katex >}}\sum_x p(x){{< /katex >}} equal?  
3) State the shortcut formula for variance.

{{% hint success %}}
Answers:
1) 0  
2) 1  
3) {{< katex >}}V(X)=E(X^2)-[E(X)]^2{{< /katex >}}
{{% /hint %}}

---

## References

- Devore:
Ch. 3 (discrete random variables) and Ch. 4 (continuous random variables)

---

{{< home-link "Home" >}} | {{< section-index >}}

---
