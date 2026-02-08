---
title: "Conditional Probability & Bayes’ Theorem"
draft: false
tags: ["AI", "Statistics", "Probability"]
categories: ["AI", "Statistics"]
weight: 200
menu: main
---

# Conditional Probability & Bayes’ Theorem

Probability often changes when we **learn new information**.

Conditional Probability and Bayes’ Theorem provide the mathematical tools to **update beliefs** based on evidence — a core idea behind many machine learning algorithms.

---

## Conditional Probability

**Conditional probability** measures the probability of an event occurring **given that another event has already occurred**.

In simple terms:

> *How likely is A, if I already know B has happened?*

---

For two events \(A\) and \(B\), with \(P(B) > 0\), the conditional probability of \(A\) given \(B\) is defined as:

{{% hint danger %}}
{{< katex display=true >}}
P(A \mid B) = \frac{P(A \cap B)}{P(B)}
{{< /katex >}}
{{% /hint %}}

- \(P(A \cap B)\): probability that **both** A and B occur  
- \(P(B)\): probability that B occurs  
- Conditioning **restricts the sample space** to cases where B is true

{{% hint info %}}
Conditional probability **redefines the universe** of outcomes based on known information.
{{% /hint %}}

---

### Example

- \(A\): “It is raining”
- \(B\): “The sky is cloudy”

Then:

- \(P(A)\): probability of rain on any day
- \(P(A \mid B)\): probability of rain **given** that the sky is cloudy

---

## Bayes’ Theorem

**Bayes’ Theorem** allows us to **reverse conditional probabilities**.

It answers questions like:

> *Given the evidence, how likely is my hypothesis?*

---

### Bayes’ Formula

{{% hint danger %}}
{{< katex display=true >}}
P(A \mid B)
=
\frac{P(B \mid A)\,P(A)}{P(B)}
{{< /katex >}}
{{% /hint %}}

- **Posterior** \(P(A \mid B)\)  
  → Updated belief after seeing evidence  

- **Likelihood** \(P(B \mid A)\)  
  → How likely the evidence is if A were true  

- **Prior** \(P(A)\)  
  → Initial belief before seeing evidence  

- **Evidence** \(P(B)\)  
  → Overall probability of observing B  

{{% hint info %}}
Bayes’ Theorem = **Prior belief + Evidence → Updated belief**
{{% /hint %}}

---

## Why Bayes’ Theorem Works

Bayes’ Theorem is derived directly from conditional probability:

{{< katex >}}
P(A \cap B) = P(A \mid B)P(B) = P(B \mid A)P(A)
{{< /katex >}}

Rearranging gives Bayes’ formula.

---

## Intuition with an Example

Suppose:
- A disease is rare → low prior \(P(A)\)
- A test is accurate → high \(P(B \mid A)\)

Even with a positive test:
- The posterior \(P(A \mid B)\) may still be low

{{% hint warning %}}
This explains why **false positives** occur in medical tests.
{{% /hint %}}

---

## Conditional Probability vs Bayes’ Theorem

| Concept | Focus |
|------|------|
| Conditional Probability | Probability **given known information** |
| Bayes’ Theorem | **Updating beliefs** using evidence |

---

## Machine Learning Connection

Bayes’ Theorem is foundational to:

- **Naïve Bayes classifiers**
- Probabilistic reasoning
- Spam detection
- Medical diagnosis models
- Bayesian inference

{{% hint info %}}
Naïve Bayes applies Bayes’ Theorem with an independence assumption between features.
{{% /hint %}}

---

## Key Takeaways

- Conditional probability restricts outcomes based on known events
- Bayes’ Theorem reverses conditioning
- Learning from data is fundamentally **Bayesian updating**
- Many ML models are probabilistic at their core

---

## Reference

- [Conditional Probability vs Bayes’ Theorem – GeeksforGeeks](https://www.geeksforgeeks.org/maths/conditional-probability-vs-bayes-theorem/)

---

{{< home-link "Home" >}} | {{< section-index >}}