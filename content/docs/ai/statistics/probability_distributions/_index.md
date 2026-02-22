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

{{< mermaid >}}
flowchart TD
  M["Module 3<br/>Probability Distributions"] --> RV["3.1 Random Variables"]
  M --> DS["3.2 Probability Distributions"]

  RV --> RV1["Discrete RVs"]
  RV --> RV2["Continuous RVs"]
  RV --> RV3["PMF / PDF / CDF"]
  RV --> RV4["Mean, Variance, Covariance"]
  RV --> RV5["Joint & Marginal Distributions"]
  RV --> RV6["Transformations"]

  DS --> D1["Bernoulli"]
  DS --> D2["Binomial"]
  DS --> D3["Poisson"]
  DS --> D4["Normal (Gaussian)"]
  DS --> D5["t / Chi-square / F (intro)"]
{{< /mermaid >}}

---

## AI/ML Connection

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

## Glossary

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

---

{{< home-link "Home" >}} | {{< section-index >}}

---
