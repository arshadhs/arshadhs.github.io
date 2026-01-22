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

- Summarise a dataset using central tendency and variability
- Explain core probability ideas using simple examples
- Apply the axioms of probability
- Distinguish mutually exclusive vs independent events

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

{{% hint danger %}}
{{< katex display=true >}}
\bar{x}=\frac{x_1+x_2+\dots+x_n}{n}
{{< /katex >}}
{{% /hint %}}

- Good for: symmetric data without extreme outliers
- Sensitive to: outliers

### Median

The middle value when sorted (or the average of the two middle values).

- Good for: skewed data and outliers
- Often preferred for: income, house prices, anything with long tails

### Mode

The most frequent value.

- Useful for: categorical data (e.g., favourite colour) or repeated discrete values

{{% hint info %}}
Rule of thumb:  
If your data is skewed, the **median** usually represents “typical” better than the **mean**.
{{% /hint %}}

---

## Measures of Variability

Variability tells you: **How spread out is the data?**

Two datasets can have the same mean, but look completely different if one is tightly packed and the other is widely spread.  
That’s why averages alone are never enough.

{{% hint %}}
- Range: “How wide is the data, end to end?”  
- Variance: “How much does data vary around the mean?”  
- Standard deviation: “Typical distance from the mean?”  
- Quartile deviation: “How spread out is the middle 50%?”  
{{% /hint %}}

### Range

Difference between the largest and smallest value.

{{% hint danger %}}
{{< katex display=true >}}
\text{Range}=\max(x)-\min(x)
{{< /katex >}}
{{% /hint %}}

Simple but fragile (a single outlier can dominate):

- It only uses two values
- One extreme outlier can distort it

### Variance

Variance measures **how far values are from the mean**, on average (squared).

{{% hint danger %}}
{{< katex display=true >}}
s^2=\frac{\sum_{i=1}^{n}(x_i-\bar{x})^2}{n-1}
{{< /katex >}}
{{% /hint %}}

- Take each value and subtract the mean
- Square the distance (removes negatives and penalises big deviations)
- Average the squared distances

- High variance → data points are spread far apart.
- Low variance → data points are tightly clustered near the mean.

Variance is in **squared units**, so it can be harder to interpret directly.

### Standard Deviation

Standard deviation is the square root of variance.

{{% hint danger %}}
{{< katex display=true >}}
s=\sqrt{s^2}
{{< /katex >}}
{{% /hint %}}

- Same units as the data
- Small SD → data is consistent
- Large SD → data is spread out

### Quartiles, IQR, and Quartile Deviation

Quartiles split sorted data into four equal parts:

- **Q1**: 25% of data lies below it
- **Q3**: 75% of data lies below it

Interquartile Range (IQR):

{{% hint danger %}}
{{< katex display=true >}}
\text{IQR}=Q_3-Q_1
{{< /katex >}}
{{% /hint %}}

Quartile Deviation (QD) is half the IQR:

{{% hint danger %}}
{{< katex display=true >}}
\text{QD}=\frac{\text{IQR}}{2}
{{< /katex >}}
{{% /hint %}}

- Robust to (not affected by) outliers
- Great for skewed distributions (e.g., income, house prices)

---

## Five-number summary

A compact description of a dataset using five values:

1. **Minimum** → where the data begins.
2. **Q1** → the middle of the lower half of the data.
3. **Median** → splits the data into two equal halves.
4. **Q3** → middle of the upper half of the data.
5. **Maximum** → where the data ends.

With one simple visual, you can immediately see:

- Spread
- Skewness
- Outliers (potentially)
- How datasets compare

This is the backbone of the **box-and-whisker plot**.

---

## Box-and-Whisker Plot (Concept)

This diagram shows the *structure* of a box plot using the five-number summary.

{{< mermaid >}}
flowchart LR
    MIN[Minimum] --- Q1[Q1] --- MED[Median] --- Q3[Q3] --- MAX[Maximum]

    %% Box region
    Q1 --- BOX1[ ]
    BOX1 --- MED
    MED --- BOX2[ ]
    BOX2 --- Q3

    %% Styling (soft)
    style MIN fill:#E1F5FE,stroke:#333
    style Q1 fill:#C8E6C9,stroke:#333
    style MED fill:#BBDEFB,stroke:#333
    style Q3 fill:#C8E6C9,stroke:#333
    style MAX fill:#E1F5FE,stroke:#333
    style BOX1 fill:#FFF9C4,stroke:#333
    style BOX2 fill:#FFF9C4,stroke:#333
{{< /mermaid >}}

{{% hint info %}}
In a real box plot:
- The **box** spans from **Q1 to Q3**
- The **line inside** the box is the **median**
- The **whiskers** extend towards the minimum and maximum (or to non-outlier limits)
{{% /hint %}}

---

## Probability

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

1. **Non-negativity**

{{% hint danger %}}
{{< katex display=true >}}
P(A)\ge 0
{{< /katex >}}
{{% /hint %}}

2. **Normalisation**

{{% hint danger %}}
{{< katex display=true >}}
P(S)=1
{{< /katex >}}
{{% /hint %}}

3. **Additivity (mutually exclusive events)**  
If \(A\cap B=\emptyset\), then

{{% hint danger %}}
{{< katex display=true >}}
P(A\cup B)=P(A)+P(B)
{{< /katex >}}
{{% /hint %}}

These axioms are the rules of the game: everything else follows from them.

---

## Definition of Probability

Two useful interpretations:

### Classical (equally likely outcomes)

{{% hint danger %}}
{{< katex display=true >}}
P(A)=\frac{\text{number of outcomes in }A}{\text{number of outcomes in }S}
{{< /katex >}}
{{% /hint %}}

Example: rolling a 6 on a fair die

{{% hint danger %}}
{{< katex display=true >}}
P(\{6\})=\frac{1}{6}
{{< /katex >}}
{{% /hint %}}

### Empirical (long-run frequency)

{{% hint danger %}}
{{< katex display=true >}}
P(A)\approx\frac{\text{count of }A}{\text{number of trials}}
{{< /katex >}}
{{% /hint %}}

Example: if “heads” appears 498 times in 1000 flips

{{% hint danger %}}
{{< katex display=true >}}
P(\text{heads})\approx 0.498
{{< /katex >}}
{{% /hint %}}

---

## Mutually Exclusive vs Independent Events

These are often confused: they are different ideas.

### Mutually exclusive (cannot happen together)

Events \(A\) and \(B\) are mutually exclusive if:

{{% hint danger %}}
{{< katex display=true >}}
A\cap B=\emptyset
{{< /katex >}}
{{% /hint %}}

If mutually exclusive:

{{% hint danger %}}
{{< katex display=true >}}
P(A\cap B)=0
{{< /katex >}}
{{% /hint %}}

### Independent (one does not affect the other)

Events \(A\) and \(B\) are independent if:

{{% hint danger %}}
{{< katex display=true >}}
P(A\cap B)=P(A)\,P(B)
{{< /katex >}}
{{% /hint %}}

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
