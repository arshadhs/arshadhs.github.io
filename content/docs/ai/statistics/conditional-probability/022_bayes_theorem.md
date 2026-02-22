---
title: "Bayes’ Theorem"
draft: false
tags: ["AI", "Statistics", "Probability"]
categories: ["AI", "Statistics"]
weight: 220
menu: main
---

# Bayes’ Theorem

### 2.1 Total probability (needed for Bayes)

Often we split the world into cases {{< katex >}}E_1,E_2,\dots,E_k{{< /katex >}} that:
- are mutually exclusive
- cover the whole sample space

Then for any event {{< katex >}}A{{< /katex >}}:

{{% colour %}}
{{< katex display=true >}}
P(A)=\sum_{i=1}^{k} P(A\mid E_i)\,P(E_i)
{{< /katex >}}
{{% /colour %}}

Tree intuition:

{{< mermaid >}}
flowchart TD
  S[Start] --> E1[Case E1]
  S --> E2[Case E2]
  S --> E3[Case E3]
  E1 --> A1["A happens"]
  E2 --> A2["A happens"]
  E3 --> A3["A happens"]
{{< /mermaid >}}

---

### 2.2 Bayes’ theorem (two-event form)

Bayes' Theorem is a mathematical formula used to determine the **conditional probability of an event based on prior knowledge and new evidence**.

It adjusts probabilities when new information comes in and helps make better decisions in uncertain situations.

Bayes' theorem (also known as the Bayes Rule or Bayes Law) is used to determine the conditional probability of event {{< katex >}}A{{< /katex >}} when event {{< katex >}}B{{< /katex >}} has already occurred.

The general statement of Bayes’ theorem is:
The conditional probability of an event {{< katex >}}A{{< /katex >}}, given the occurrence of another event {{< katex >}}B{{< /katex >}}, is equal to the product of the probability of {{< katex >}}B{{< /katex >}} given {{< katex >}}A{{< /katex >}} and the probability of {{< katex >}}A{{< /katex >}} divided by the probability of event {{< katex >}}B{{< /katex >}}.

Bayes’ theorem “reverses” conditioning:

{{% colour %}}
{{< katex display=true >}}
P(A\mid B)=\frac{P(B\mid A)\,P(A)}{P(B)},\quad P(B)>0
{{< /katex >}}
{{% /colour %}}

Language you will hear often:
- Prior:
{{< katex >}}P(A){{< /katex >}}
- Likelihood:
{{< katex >}}P(B\mid A){{< /katex >}}
- Evidence:
{{< katex >}}P(B){{< /katex >}}
- Posterior:
{{< katex >}}P(A\mid B){{< /katex >}}

---

### Terms related to Bayes' Theorem

#### Hypotheses

Hypotheses refer to possible events or outcomes in the sample space; they are denoted as {{< katex >}}E_1,E_2,\dots,E_n{{< /katex >}}.

Each hypothesis represents a distinct scenario that could explain an observed event.

#### Priori Probability

Priori probability {{< katex >}}P(E_i){{< /katex >}} is the initial probability of an event occurring before any new data is taken into account.

It reflects existing knowledge or assumptions about the event.

Example:
The probability of a person having a disease before taking a test.

#### Posterior Probability

Posterior probability {{< katex >}}P(E_i\mid A){{< /katex >}} is the updated probability of an event after considering new information.

It is derived using Bayes’ Theorem.

Example:
The probability of having a disease given a positive test result.

#### Conditional Probability

The probability of an event {{< katex >}}A{{< /katex >}} based on the occurrence of another event {{< katex >}}B{{< /katex >}} is termed conditional probability.

It is denoted as {{< katex >}}P(A\mid B){{< /katex >}} and represents the probability of {{< katex >}}A{{< /katex >}} when event {{< katex >}}B{{< /katex >}} has already happened.

#### Joint Probability

When the probability of two or more events occurring together and at the same time is measured, it is called joint probability.

For two events {{< katex >}}A{{< /katex >}} and {{< katex >}}B{{< /katex >}}, it is denoted by {{< katex >}}P(A\cap B){{< /katex >}}.

#### Random Variables

Real-valued variables whose possible values are determined by random experiments are called random variables.

The probability of finding such variables is the experimental probability.

---

### Difference between Conditional Probability and Bayes Theorem

| Topic | Bayes Theorem | Conditional Probability |
|---|---|---|
| Meaning | Derived using conditional probability and used to find the “reverse” probability. | Probability of event {{< katex >}}A{{< /katex >}} when event {{< katex >}}B{{< /katex >}} has already occurred. |
| Formula | {{< katex >}}P(A\mid B)=\frac{P(B\mid A)P(A)}{P(B)}{{< /katex >}} | {{< katex >}}P(A\mid B)=\frac{P(A\cap B)}{P(B)}{{< /katex >}} |
| Purpose | Update the probability of an event based on new evidence. | Find the probability of one event based on the occurrence of another. |
| Focus | Uses prior knowledge and evidence to compute a revised probability. | Direct relationship between two events. |

---

### 2.3 Bayes’ theorem (general partition form)

If {{< katex >}}E_1,\dots,E_k{{< /katex >}} is a partition and B is observed, then:

{{% colour %}}
{{< katex display=true >}}
P(E_j\mid B)=\frac{P(B\mid E_j)\,P(E_j)}{\sum_{i=1}^{k} P(B\mid E_i)\,P(E_i)}
{{< /katex >}}
{{% /colour %}}

This form is ideal for:
diagnosis, fault detection, and “which cause is most likely?” problems.

---

### 2.4 Worked example (rare disease, why Bayes can surprise)

Suppose:
- Disease rate is 1 in 1000:
{{< katex >}}P(D)=0.001{{< /katex >}}
- Test sensitivity 99%:
{{< katex >}}P(+\mid D)=0.99{{< /katex >}}
- False positive rate 2%:
{{< katex >}}P(+\mid D^c)=0.02{{< /katex >}}

First compute evidence:

{{% colour %}}
{{< katex display=true >}}
P(+)=P(+\mid D)P(D)+P(+\mid D^c)P(D^c)
{{< /katex >}}
{{% /colour %}}

{{% colour %}}
{{< katex display=true >}}
P(+)=0.99(0.001)+0.02(0.999)=0.02097
{{< /katex >}}
{{% /colour %}}

Now apply Bayes:

{{% colour %}}
{{< katex display=true >}}
P(D\mid +)=\frac{0.99(0.001)}{0.02097}\approx 0.047
{{< /katex >}}
{{% /colour %}}

{{% hint warning %}}
Why this matters:
Even a very accurate test can produce many false positives when the condition is rare.
The prior probability (base rate) strongly affects the posterior.
{{% /hint %}}

---

### 2.5 Worked example (communication channel)

A binary channel:
- 1 is transmitted 40% of the time:
{{< katex >}}P(T1)=0.4{{< /katex >}}
- 0 is correctly received with probability 0.90
- 1 is correctly received with probability 0.95

Let:
{{< katex >}}R1{{< /katex >}} = “1 received”.

Then:
{{< katex >}}P(R1\mid T1)=0.95{{< /katex >}},
{{< katex >}}P(R1\mid T0)=0.10{{< /katex >}},
{{< katex >}}P(T0)=0.6{{< /katex >}}.

Total probability:

{{% colour %}}
{{< katex display=true >}}
P(R1)=P(R1\mid T1)P(T1)+P(R1\mid T0)P(T0)
{{< /katex >}}
{{% /colour %}}

{{% colour %}}
{{< katex display=true >}}
P(R1)=0.95(0.4)+0.10(0.6)=0.44
{{< /katex >}}
{{% /colour %}}

Bayes:

{{% colour %}}
{{< katex display=true >}}
P(T1\mid R1)=\frac{P(R1\mid T1)P(T1)}{P(R1)}=\frac{0.95(0.4)}{0.44}\approx 0.864
{{< /katex >}}
{{% /colour %}}

---

## Practice prompts

1) Write {{< katex >}}P(A\cap B\cap C\cap D){{< /katex >}} using the multiplication rule.  
2) If {{< katex >}}P(A)=0.3{{< /katex >}} and {{< katex >}}P(B\mid A)=0.8{{< /katex >}}, find {{< katex >}}P(A\cap B){{< /katex >}}.  
3) Create a 3-branch total probability tree from your work (e.g., device type, customer segment, failure mode), and compute an overall probability.

{{% hint success %}}
Quick answer for 2):
{{< katex >}}P(A\cap B)=0.3	imes 0.8=0.24{{< /katex >}}
{{% /hint %}}

---

## Reference

- [Bayes' Theorem](https://www.geeksforgeeks.org/maths/bayes-theorem/)
- [Conditional Probability vs Bayes’ Theorem – GeeksforGeeks](https://www.geeksforgeeks.org/maths/conditional-probability-vs-bayes-theorem/)
- [Bayes' theorem in Artificial intelligence](https://www.geeksforgeeks.org/artificial-intelligence/bayes-theorem-in-artificial-intelligence/)

---

{{< home-link "Home" >}} | {{< section-index >}}

---
