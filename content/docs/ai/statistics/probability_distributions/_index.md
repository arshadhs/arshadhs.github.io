---
title: "Probability Distributions"
date: 2026-02-22
draft: false
tags: ["AI", "Statistics", "Probability"]
categories: ["AI", "Statistics"]
weight: 300
menu: main
---

# Probability Distributions

Probability distributions are the bridge between:
real-world randomness and mathematical modelling.

A random experiment produces outcomes.
A random variable turns those outcomes into numbers.
A probability distribution tells you how likely each number (or range of numbers) is.

{{% hint info %}}
Key takeaway:
A distribution is a complete “story” about uncertainty:
what values are possible, how likely they are, and how we summarise them (mean, variance).
{{% /hint %}}

---

## What you will learn in Module 3

- How to define random variables (discrete and continuous)
- How to describe them using:
pmf, pdf, and cdf
- How to compute:
expectation, variance, and covariance
- How common named distributions work (Bernoulli, Binomial, Poisson, Normal)
- Why t, chi-square, and F distributions matter later (hypothesis testing)

---

## Roadmap (this module)

{{< mermaid >}}
flowchart TD
  A[Module 3: Probability Distributions] --> B[3.1 Random Variables]
  A --> C[3.2 Common Distributions]

  B --> B1[Discrete RVs]
  B --> B2[Continuous RVs]
  B --> B3[pmf / pdf / cdf]
  B --> B4[Mean, Variance, Covariance]
  B --> B5[Joint & Marginal Distributions]
  B --> B6[Transformations]

  C --> C1[Bernoulli]
  C --> C2[Binomial]
  C --> C3[Poisson]
  C --> C4[Normal (Gaussian)]
  C --> C5[t, Chi-square, F (intro)]
{{< /mermaid >}}

---

## How this connects to AI/ML

- Many ML models are probabilistic:
they assume data (or errors) follow a distribution.
- Loss functions often come from distribution assumptions:
squared loss aligns with Gaussian noise.
- Naïve Bayes (from the previous module) becomes practical once you can model:
{{< katex >}}P(X\mid Y){{< /katex >}} using suitable distributions.

{{% hint warning %}}
In practice:
choosing a distribution is a modelling decision.
It affects:
prediction, uncertainty estimates, and what “rare” or “typical” means in your data.
{{% /hint %}}

---

## Pages in this module

- {{< relref "random-variables" >}}3.1 Random Variables{{< /relref >}}
- {{< relref "common-distributions" >}}3.2 Probability Distributions (Bernoulli, Binomial, Poisson, Normal, …){{< /relref >}}

---

## Quick glossary

Random variable:
a rule that maps each outcome to a number.

pmf:
for discrete variables, gives {{< katex >}}P(X=x){{< /katex >}}.

pdf:
for continuous variables, probabilities come from areas:
{{< katex >}}P(a\le X\le b)=\int_a^b f(x)\,dx{{< /katex >}}.

cdf:
accumulates probability up to x:
{{< katex >}}F(x)=P(X\le x){{< /katex >}}.

---

## References

- Devore:
Probability and Statistics for Engineering and the Sciences (Ch. 3–4; plus t/chi-square/F later in the book)
- ISM Session notes:
Random Variables (Session 5)

---

{{< home-link "Home" >}} | {{< section-index >}}

---
