---
title: "Basic Probability & Statistics"
draft: false
tags: ["AI", "Statistics"]
categories: ["AI", "Statistics"]
weight: 1210
menu: main
---
# Basic Probability & Statistics

**Statistics**:
describes data (what you *see*).

**Probability**:
models uncertainty (what you *don’t know* yet).

---

## Learning goals

By the end of this topic you should be able to:
- Summarise a dataset using central tendency and variability.
- Explain core probability ideas using simple examples.
- Apply the axioms of probability.
- Distinguish:
mutually exclusive vs independent events.

---

## 1.1 Measures of Central Tendency

Central tendency tells you:
where the “middle” of the data is.

### Mean (average)

\[
\bar{x}=\frac{x_1+x_2+\dots+x_n}{n}
\]

- Good for:
symmetric data without extreme outliers.
- Sensitive to:
outliers.

### Median

The middle value when sorted (or average of the two middle values).

- Good for:
skewed data and outliers.
- Often preferred for:
income, house prices, anything with long tails.

### Mode

The most frequent value.

- Useful for:
categorical data (e.g., favourite colour) or repeated discrete values.

{{% hint info %}}
Rule of thumb:
If your data is skewed, the **median** usually represents “typical” better than the **mean**.
{{% /hint %}}

---

## 1.2 Measures of Variability

Variability tells you:
how spread out the data is.

### Range

\[
\text{Range}=\max(x)-\min(x)
\]

Simple but fragile (a single outlier can dominate).

### Variance and Standard Deviation

Variance:
average squared distance from the mean.

Sample variance:
\[
s^2=\frac{\sum_{i=1}^{n}(x_i-\bar{x})^2}{n-1}
\]

Standard deviation:
\[
s=\sqrt{s^2}
\]

- SD is in the same unit as the data.
- Bigger SD means:
more spread.

### Interquartile Range (IQR)

\[
\text{IQR}=Q_3-Q_1
\]

- Robust to outliers.
- Great for:
skewed distributions.

### Five-number summary

- Minimum
- Q1
- Median
- Q3
- Maximum

This is the backbone of the box plot.

---

## Quick visual intuition

{{< mermaid >}}
flowchart TD
  A[Dataset] --> B[Central Tendency]
  A --> C[Variability]
  B --> B1[Mean]
  B --> B2[Median]
  B --> B3[Mode]
  C --> C1[Range]
  C --> C2[Variance]
  C --> C3[Standard Deviation]
  C --> C4[IQR]
{{< /mermaid >}}

---

## 1.3 Basic Probability Concepts

Probability quantifies uncertainty:
a number between 0 and 1.

- 0 means:
impossible
- 1 means:
certain

### Sample space and events

- Sample space \(S\):
all possible outcomes  
  Example:
dice roll → \(S=\{1,2,3,4,5,6\}\)

- Event \(A\):
a subset of outcomes  
  Example:
“even number” → \(A=\{2,4,6\}\)

---

## 1.3.1 Axioms of Probability

For any event \(A\):

1. **Non-negativity**:
\[
P(A)\ge 0
\]

2. **Normalisation**:
\[
P(S)=1
\]

3. **Additivity (for mutually exclusive events)**:
If \(A\cap B=\emptyset\), then
\[
P(A\cup B)=P(A)+P(B)
\]

These axioms are the rules of the game:
everything else follows from them.

---

## 1.3.2 Definition of Probability

Two useful interpretations:

### Classical (equally likely outcomes)

If all outcomes are equally likely:
\[
P(A)=\frac{\text{number of outcomes in }A}{\text{number of outcomes in }S}
\]

Example:
rolling a 6 on a fair die:
\[
P(\{6\})=\frac{1}{6}
\]

### Empirical (long-run frequency)

Probability is what you observe as trials grow large:
\[
P(A)\approx\frac{\text{count of }A}{\text{number of trials}}
\]

Example:
if “heads” appears 498 times in 1000 flips:
\[
P(\text{heads})\approx 0.498
\]

---

## 1.3.3 Mutually Exclusive vs Independent Events

These are often confused:
they are different ideas.

### Mutually exclusive (cannot happen together)

Events \(A\) and \(B\) are mutually exclusive if:
\[
A\cap B=\emptyset
\]

Example:
On one die roll:
- \(A\): “roll a 2”
- \(B\): “roll a 5”

They can’t both happen on the same roll.

If mutually exclusive:
\[
P(A\cap B)=0
\]

### Independent (one does not affect the other)

Events \(A\) and \(B\) are independent if:
\[
P(A\cap B)=P(A)\,P(B)
\]

Example:
- \(A\): “first coin flip is heads”
- \(B\): “second coin flip is heads”

The second flip doesn’t care about the first.

---

## A simple comparison diagram

{{< mermaid >}}
flowchart LR
  A[Two events: A and B] --> B{Can they happen together?}
  B -->|No| C[Mutually Exclusive<br/>A ∩ B = ∅]
  B -->|Yes| D{Does A change B?}
  D -->|No| E[Independent<br/>P(A ∩ B)=P(A)P(B)]
  D -->|Yes| F[Dependent<br/>Use conditional probability]
{{< /mermaid >}}

---

## Mini-check (self-test)

1. If \(P(A)=0.4\) and \(P(B)=0.3\) and \(A,B\) are independent:
what is \(P(A\cap B)\)?

2. If \(A,B\) are mutually exclusive:
what is \(P(A\cap B)\)?

3. Which measure is more robust to outliers:
mean or median?

{{% hint success %}}
Answers:
1) \(0.4\times 0.3=0.12\)  
2) \(0\)  
3) Median
{{% /hint %}}

---

## What’s next

Next topic:
**Conditional Probability & Bayes’ Theorem**  
This is where “given what I already know…” becomes mathematics, and where Naïve Bayes begins.

---

{{< home-link "Home" >}} | {{< section-index >}}
