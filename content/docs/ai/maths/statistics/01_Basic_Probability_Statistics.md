---
title: "Basic Probability & Statistics"
draft: false
tags: ["AI", "Statistics"]
categories: ["AI", "Statistics"]
weight: 100
menu: main
---

# Basic Probability & Statistics

**Statistics**: describes data (what you *see*).  
**Probability**: models uncertainty (what you *don’t know* yet).

- Summarise a dataset using central tendency and variability.
- Core probability ideas using simple examples.
- Axioms of probability.
- Mutually exclusive vs Independent events.

---

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

## Measures of Central Tendency

Central tendency tells you where the “middle” of the data is.


### Mean (average)

{{< katex display=true >}}
\bar{x}=\frac{x_1+x_2+\dots+x_n}{n}
{{< /katex >}}

- Good for: symmetric data without extreme outliers.
- Sensitive to: outliers.

### Median

The middle value when sorted (or average of the two middle values).

- Good for: skewed data and outliers.
- Often preferred for: income, house prices, anything with long tails.

### Mode

The most frequent value.

- Useful for: categorical data (e.g., favourite colour) or repeated discrete values.

{{% hint info %}}
Rule of thumb:  
If your data is skewed, the **median** usually represents “typical” better than the **mean**.
{{% /hint %}}

---

## Measures of Variability

Variability tells you how spread out the data is.

### Range

{{< katex display=true >}}
\text{Range}=\max(x)-\min(x)
{{< /katex >}}

Simple but fragile (a single outlier can dominate).

### Variance and Standard Deviation

Variance: average squared distance from the mean.

Sample variance:

{{< katex display=true >}}
s^2=\frac{\sum_{i=1}^{n}(x_i-\bar{x})^2}{n-1}
{{< /katex >}}

Standard deviation:

{{< katex display=true >}}
s=\sqrt{s^2}
{{< /katex >}}

- SD is in the same unit as the data.
- Bigger SD means: more spread.

### Interquartile Range (IQR)

{{< katex display=true >}}
\text{IQR}=Q_3-Q_1
{{< /katex >}}

- Robust to outliers.
- Great for: skewed distributions.

### Five-number summary

- Minimum
- Q1
- Median
- Q3
- Maximum

This is the backbone of the box plot.

---

## Basic Probability Concepts

Probability quantifies uncertainty: a number between 0 and 1.

- 0 means: impossible
- 1 means: certain

### Sample space and events

- Sample space \(S\): all possible outcomes  
  Example: dice roll → \(S=\{1,2,3,4,5,6\}\)

- Event \(A\): a subset of outcomes  
  Example: “even number” → \(A=\{2,4,6\}\)

---

## Axioms of Probability

For any event \(A\):

1. **Non-negativity**:

{{< katex display=true >}}
P(A)\ge 0
{{< /katex >}}

2. **Normalisation**:

{{< katex display=true >}}
P(S)=1
{{< /katex >}}

3. **Additivity (for mutually exclusive events)**:  
If \(A\cap B=\emptyset\), then

{{< katex display=true >}}
P(A\cup B)=P(A)+P(B)
{{< /katex >}}

These axioms are the rules of the game: everything else follows from them.

---

## Probability

Two useful interpretations:

### Classical (equally likely outcomes)

If all outcomes are equally likely:

{{< katex display=true >}}
P(A)=\frac{\text{number of outcomes in }A}{\text{number of outcomes in }S}
{{< /katex >}}

Example: rolling a 6 on a fair die:

{{< katex display=true >}}
P(\{6\})=\frac{1}{6}
{{< /katex >}}

### Empirical (long-run frequency)

Probability is what you observe as trials grow large:

{{< katex display=true >}}
P(A)\approx\frac{\text{count of }A}{\text{number of trials}}
{{< /katex >}}

Example: if “heads” appears 498 times in 1000 flips:

{{< katex display=true >}}
P(\text{heads})\approx 0.498
{{< /katex >}}

---

## Mutually Exclusive vs Independent Events

These are often confused: they are different ideas.

### Mutually exclusive (cannot happen together)

Events \(A\) and \(B\) are mutually exclusive if:

{{< katex display=true >}}
A\cap B=\emptyset
{{< /katex >}}

If mutually exclusive:

{{< katex display=true >}}
P(A\cap B)=0
{{< /katex >}}

### Independent (one does not affect the other)

Events \(A\) and \(B\) are independent if:

{{< katex display=true >}}
P(A\cap B)=P(A)\,P(B)
{{< /katex >}}

---

{{< mermaid >}}
flowchart LR
    A["Two events: A and B"] --> B{"Can they happen together?"}
    B -->|No| C["Mutually Exclusive: A ∩ B = ∅"]
    B -->|Yes| D{"Does A change B?"}
    D -->|No| E["Independent: P(A ∩ B)=P(A)P(B)"]
    D -->|Yes| F["Dependent: use conditional probability"]
{{< /mermaid >}}

---

## Mini-check (self-test)

1. If \(P(A)=0.4\) and \(P(B)=0.3\) and \(A,B\) are independent: what is \(P(A\cap B)\)?
2. If \(A,B\) are mutually exclusive: what is \(P(A\cap B)\)?
3. Which measure is more robust to outliers: mean or median?

{{% hint success %}}
Answers:  
1) \(0.4\times 0.3=0.12\)  
2) \(0\)  
3) Median
{{% /hint %}}

---

## What’s next

**Conditional Probability & Bayes’ Theorem**  
This is where “given what I already know…” becomes mathematics, and where Naïve Bayes begins.

---

{{< home-link "Home" >}} | {{< section-index >}}
