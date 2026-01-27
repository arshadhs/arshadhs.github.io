---
title: "Basic Probability"
draft: false
tags: ["AI", "Statistics"]
categories: ["AI", "Statistics"]
weight: 110
menu: main
---

# Basic Probability

**Probability**: models uncertainty (what you *don’t know* yet).

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
