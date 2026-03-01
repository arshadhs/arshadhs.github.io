---
title: "Common Probability Distributions"
date: 2026-02-22
draft: false
tags: ["AI", "Statistics", "Probability"]
categories: ["AI", "Statistics"]
weight: 320
menu: main
---

# Common Probability Distributions

Once you can describe a random variable using a pmf or pdf, the next step is to use
named distributions that appear repeatedly in real data and in ML models.

{{% hint info %}}
Key takeaway:
Named distributions give you ready-made probability models for common patterns:
binary outcomes, counts, and measurement noise.
{{% /hint %}}

---

{{< mermaid >}}
flowchart TD
PD["Probability<br/>distributions"] --> DS["Common<br/>distributions"]

DS --> DIS["Discrete"]
DS --> CON["Continuous"]

DIS --> D1["Bernoulli"]
DIS --> D2["Binomial"]
DIS --> D3["Poisson"]

CON --> D4["Normal<br/>(Gaussian)"]
CON --> D5["t / Chi-square / F<br/>(intro)"]

style PD fill:#90CAF9,stroke:#1E88E5,color:#000
style DS fill:#90CAF9,stroke:#1E88E5,color:#000

style DIS fill:#CE93D8,stroke:#8E24AA,color:#000
style CON fill:#CE93D8,stroke:#8E24AA,color:#000

style D1 fill:#C8E6C9,stroke:#2E7D32,color:#000
style D2 fill:#C8E6C9,stroke:#2E7D32,color:#000
style D3 fill:#C8E6C9,stroke:#2E7D32,color:#000
style D4 fill:#C8E6C9,stroke:#2E7D32,color:#000
style D5 fill:#C8E6C9,stroke:#2E7D32,color:#000
{{< /mermaid >}}

---

## 1) Bernoulli distribution (binary)

Use when:
one trial has two outcomes (success/failure).

Support:
{{< katex >}}x\in\{0,1\}{{< /katex >}}

{{% colour %}}
{{< katex display=true >}}
P(X=x)=p^x(1-p)^{1-x},\quad x\in\{0,1\}
{{< /katex >}}
{{% /colour %}}

Mean and variance:

{{% colour %}}
{{< katex display=true >}}
E(X)=p,\quad V(X)=p(1-p)
{{< /katex >}}
{{% /colour %}}

ML connection:
binary labels, click/no-click, churn/no-churn.

---

## 2) Binomial distribution (number of successes in n trials)

Use when:
- fixed number of independent trials {{< katex >}}n{{< /katex >}}
- constant success probability {{< katex >}}p{{< /katex >}}
- count successes {{< katex >}}X{{< /katex >}}

Support:
{{< katex >}}x=0,1,2,\dots,n{{< /katex >}}

{{% colour %}}
{{< katex display=true >}}
P(X=x)=\binom{n}{x}p^x(1-p)^{n-x}
{{< /katex >}}
{{% /colour %}}

Mean and variance:

{{% colour %}}
{{< katex display=true >}}
E(X)=np,\quad V(X)=np(1-p)
{{< /katex >}}
{{% /colour %}}

ML connection:
how many “positive” outcomes in n repeated trials (quality checks, conversions, etc.).

### 2.1 Binomial Distribution: Understanding

| Phrase in questions | Meaning in maths | Example for a Binomial RV $X$ |
|---|---|---|
| “at least” | $X \ge k$ | $P(X \ge 3)$ |
| “more than” / “greater than” | $X > k$ | $P(X > 3)=P(X \ge 4)$ |
| “fewer than” / “less than” | $X < k$ | $P(X < 3)=P(X \le 2)$ |
| “no more than” / “at most” | $X \le k$ | $P(X \le 3)$ |
| “exactly” | $X = k$ | $P(X = 3)$ |

---

## 3) Poisson distribution (counts over time/space)

Use when:
you count events in a fixed window and events occur independently at an average rate.

Parameter:
{{< katex >}}\lambda>0{{< /katex >}} (often written as {{< katex >}}m{{< /katex >}} in some texts)

Support:
{{< katex >}}x=0,1,2,\dots{{< /katex >}}

{{% colour %}}
{{< katex display=true >}}
P(X=x)=e^{-\lambda}\frac{\lambda^x}{x!}
{{< /katex >}}
{{% /colour %}}

Mean and variance:

{{% colour %}}
{{< katex display=true >}}
E(X)=\lambda,\quad V(X)=\lambda
{{< /katex >}}
{{% /colour %}}

ML connection:
call arrivals per minute, defects per metre, incidents per day.

---

## 4) Normal (Gaussian) distribution (measurement noise)

Use when:
values cluster around a mean with symmetric “bell-shaped” variation.

Parameters:
mean {{< katex >}}\mu{{< /katex >}}, standard deviation {{< katex >}}\sigma{{< /katex >}}

{{% colour %}}
{{< katex display=true >}}
f(x)=\frac{1}{\sqrt{2\pi}\sigma}\exp\left(-\frac{(x-\mu)^2}{2\sigma^2}\right)
{{< /katex >}}
{{% /colour %}}

Standard normal:
{{< katex >}}Z\sim N(0,1){{< /katex >}}.

{{% hint info %}}
Why Gaussian appears everywhere:
Many small independent effects add up to something close to normal.
In ML:
Gaussian noise assumptions lead to squared-error loss.
{{% /hint %}}

---

## 5) t, Chi-square, F (intro)

These distributions matter later for inference, but you should recognise where they come from.

### 5.1 t distribution (small-sample uncertainty in the mean)

When sampling from a normal population with unknown variance, the statistic

{{% colour %}}
{{< katex display=true >}}
T=\frac{\bar{X}-\mu}{S/\sqrt{n}}
{{< /katex >}}
{{% /colour %}}

follows a t distribution with {{< katex >}}n-1{{< /katex >}} degrees of freedom.

Key idea:
t is like the normal distribution but with heavier tails (more uncertainty).

---

### 5.2 Chi-square distribution (variation / sum of squares)

A chi-square distribution has one parameter:
degrees of freedom {{< katex >}}\nu{{< /katex >}}.

It is positive-valued and is built from sums of squared normal variables.

You will see it in:
confidence intervals and tests about variance.

---

### 5.3 F distribution (ratio of variances)

An F distribution is related to a ratio of two scaled chi-square variables.

You will see it in:
ANOVA and comparing two variances.

---

## 6) Quick selection guide

{{< mermaid >}}
flowchart TD
A["What type of data<br/>are you modelling?"] --> B["Binary outcome"]
A --> C["Count of events"]
A --> D["Continuous measurement"]

B --> B1["Bernoulli<br/>(single trial)"]
B --> B2["Binomial<br/>(n trials)"]

C --> C1["Poisson<br/>(rate-based counts)"]

D --> D1["Normal<br/>(measurement noise)"]
D --> D2["t / Chi-square / F<br/>used in inference"]

style A fill:#90CAF9,stroke:#1E88E5,color:#000
style B fill:#CE93D8,stroke:#8E24AA,color:#000
style C fill:#CE93D8,stroke:#8E24AA,color:#000
style D fill:#CE93D8,stroke:#8E24AA,color:#000

style B1 fill:#C8E6C9,stroke:#2E7D32,color:#000
style B2 fill:#C8E6C9,stroke:#2E7D32,color:#000
style C1 fill:#C8E6C9,stroke:#2E7D32,color:#000
style D1 fill:#C8E6C9,stroke:#2E7D32,color:#000
style D2 fill:#C8E6C9,stroke:#2E7D32,color:#000
{{< /mermaid >}}

---

## 7) Summary table

| Distribution | Typical data | Parameters | Support |
|---|---|---|---|
| Bernoulli | yes/no | p | 0, 1 |
| Binomial | successes in n trials | n, p | 0..n |
| Poisson | counts in a fixed window | λ | 0,1,2,… |
| Normal | continuous measurements | μ, σ | all real numbers |
| t | mean inference (small n) | df | all real numbers |
| Chi-square | variance / sums of squares | df | ≥ 0 |
| F | ratio of variances | df1, df2 | ≥ 0 |

---

## References

- Devore:
Ch. 3 (Bernoulli, Binomial, Poisson) and Ch. 4 (Normal, Chi-square) plus later chapters for t and F
- ISM sessions:
use this as the syllabus-aligned guide

---

{{< home-link "Home" >}} | {{< section-index >}}

---
