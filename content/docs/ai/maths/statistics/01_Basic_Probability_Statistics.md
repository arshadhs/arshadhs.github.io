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

Variability tells you **“How spread out is the data?”**.

Two datasets can have the same average, but look completely different if one is tightly packed and the other is widely spread.
That’s why averages alone are never enough.

### Range

Difference between largest and smallest value in the data and.

{{< katex display=true >}}
\text{Range}=\max(x)-\min(x)
{{< /katex >}}

Simple but fragile (a single outlier can dominate):
- it only looks at two values
- One extreme outlier can completely distort it

### Variance

Variance looks at **how far values are from the mean**, on average.

- average squared distance from the mean.

{{< katex display=true >}}
s^2=\frac{\sum_{i=1}^{n}(x_i-\bar{x})^2}{n-1}
{{< /katex >}}

- take each value
- see how far it is from the mean
- square that distance
- then average those squared distances.

Why square them?
- it makes all distances positive
- it penalises large deviations more

Variance tells you: how much the data varies around the mean?
High variance means: data points are spread far apart.
Low variance means: data points are tightly clustered near the mean.
The downside: variance is in squared units, which makes it harder to interpret directly.

### Standard Deviation

Standard deviation is just the **square root of variance**.

SD is the **most commonly used measure of spread**.

{{< katex display=true >}}
s=\sqrt{s^2}
{{< /katex >}}

- SD is in the **same unit as the data**.
- Small SD means: data is consistent and predictable.
- Bigger SD means: data is scattered and uncertain.

Standard deviation tells you the typical distance of data points from the mean.

### Quartile Derivation

Quartile deviation focuses on the **middle half** of the data.

First, you split the data into four equal parts:
- Q1 is the value where 25% of data lies below it
- Q3 is the value where 75% of data lies below it
- The difference between Q3 and Q1 is called the Interquartile Range, or IQR.

Quartile deviation is simply: half of the IQR.
QD tells **how spread out the central 50% of the data is**.
The key advantage: it completely ignores extreme values.

This makes quartile deviation **very useful when data has outliers or is skewed**, like incomes or house prices.

### Interquartile Range (IQR)

{{< katex display=true >}}
\text{IQR}=Q_3-Q_1
{{< /katex >}}

- Robust to outliers.
- Great for: skewed distributions.

{{% hint %}}
Range: “How wide is the data, end to end?”
Variance: “How much does data vary around the mean?”
Standard deviation: “On average, how far are points from the mean?”
Quartile deviation: “How spread out is the middle half of the data?”
{{% /hint %}}

### Five-number summary

The five-number summary is a compact way to describe the shape and spread of a dataset using just five values.

Instead of looking at every data point, we describe the data using:
- where it starts,
- where it ends,
- how it’s distributed in between.

These 5-numbers are:
1. **Minimum**: where the data begins.
2. **Q1**: the middle of the lower half of the data.
3. **Median**: splits the data into two equal halves.
4. **Q3**: middle of the upper half of the data.
5. **Maximum**: where the data ends.

With one simple visual, you can immediately see:
- spread
- skewness
- presence of outliers
- how datasets compare

This is the backbone of the box plot. The five-number summary turns a messy dataset into a clear snapshot of its distribution.

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

### Axioms of Probability

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

### Mutually Exclusive vs Independent Events

These are often confused: they are different ideas.

#### Mutually exclusive (cannot happen together)

Events \(A\) and \(B\) are mutually exclusive if:

{{< katex display=true >}}
A\cap B=\emptyset
{{< /katex >}}

If mutually exclusive:

{{< katex display=true >}}
P(A\cap B)=0
{{< /katex >}}

#### Independent (one does not affect the other)

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

