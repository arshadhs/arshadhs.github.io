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

Conditional probability and Bayes’ theorem give a structured way to **update beliefs** using evidence.

{{% hint info %}}
Conditional probability updates probabilities after an event is known.
Bayes’ theorem “reverses” conditioning to estimate the probability of a *cause* given an observed *effect*.
{{% /hint %}}

---

## 1) Why conditional probability exists

In many real situations:
we start with limited information and assign probabilities.  
Later, we observe something (evidence), and we must revise those probabilities.

Example idea:
a medical test is positive (evidence), so our belief about having a disease (cause) changes.
But it rarely becomes 0 or 1 because tests can make mistakes.

---

## 2) Conditional probability

Conditional probability answers:

“How likely is event {{< katex >}}B{{< /katex >}}, given that {{< katex >}}A{{< /katex >}} has already occurred?”

If {{< katex >}}P(A)>0{{< /katex >}}, the conditional probability of {{< katex >}}B{{< /katex >}} given {{< katex >}}A{{< /katex >}} is:

{{% colour %}}
{{< katex display=true >}}
P(B \mid A)=rac{P(A\cap B)}{P(A)}
{{< /katex >}}
{{% /colour %}}

Meaning:
- {{< katex >}}P(A\cap B){{< /katex >}} is the probability that both events happen together (joint probability).
- Conditioning on {{< katex >}}A{{< /katex >}} means:
we restrict attention to the outcomes where {{< katex >}}A{{< /katex >}} is true.

{{% hint info %}}
Conditioning “shrinks the universe”:
once {{< katex >}}A{{< /katex >}} is known to have happened, we only count outcomes inside {{< katex >}}A{{< /katex >}}.
{{% /hint %}}

---

## 3) Multiplication rule (very important)

Start from the definition:

{{% colour %}}
{{< katex display=true >}}
P(B \mid A)=rac{P(A\cap B)}{P(A)}
{{< /katex >}}
{{% /colour %}}

Rearrange to get the multiplication rule:

{{% colour %}}
{{< katex display=true >}}
P(A\cap B)=P(A)\,P(B\mid A)
{{< /katex >}}
{{% /colour %}}

Also:

{{% colour %}}
{{< katex display=true >}}
P(A\cap B)=P(B)\,P(A\mid B)
{{< /katex >}}
{{% /colour %}}

For three events {{< katex >}}A,B,C{{< /katex >}}:

{{% colour %}}
{{< katex display=true >}}
P(A\cap B\cap C)=P(A)\,P(B\mid A)\,P(C\mid A\cap B)
{{< /katex >}}
{{% /colour %}}

{{% hint warning %}}
This is the bridge between:
conditional probability and joint probability.
In ML, this “chain idea” shows up when modelling sequences and joint distributions.
{{% /hint %}}

---

## 4) Independence (how to test it)

Two events {{< katex >}}A{{< /katex >}} and {{< katex >}}B{{< /katex >}} are independent if knowing one does not change the probability of the other.

Any one of these conditions is enough:

{{% colour %}}
{{< katex display=true >}}
P(B\mid A)=P(B)
{{< /katex >}}
{{% /colour %}}

{{% colour %}}
{{< katex display=true >}}
P(A\mid B)=P(A)
{{< /katex >}}
{{% /colour %}}

{{% colour %}}
{{< katex display=true >}}
P(A\cap B)=P(A)\,P(B)
{{< /katex >}}
{{% /colour %}}

{{% hint warning %}}
Common confusion:
Mutually exclusive events cannot be independent (unless one has probability 0).
If {{< katex >}}A\cap B=arnothing{{< /katex >}}, then {{< katex >}}P(A\cap B)=0{{< /katex >}},
but {{< katex >}}P(A)P(B){{< /katex >}} is positive if both events can occur.
{{% /hint %}}

---

## 5) A quick ML-flavoured example (independence assumption)

Let:
- {{< katex >}}A{{< /katex >}} = “word ‘offer’ appears”
- {{< katex >}}B{{< /katex >}} = “suspicious link appears”

Assume independence (this is the “naïve” assumption).

Given:
{{< katex >}}P(A)=0.6{{< /katex >}}, {{< katex >}}P(B)=0.4{{< /katex >}}

Then:

{{% colour %}}
{{< katex display=true >}}
P(A\cap B)=P(A)P(B)=0.6	imes 0.4=0.24
{{< /katex >}}
{{% /colour %}}

And:

{{% colour %}}
{{< katex display=true >}}
P(A\cup B)=P(A)+P(B)-P(A\cap B)=0.6+0.4-0.24=0.76
{{< /katex >}}
{{% /colour %}}

Neither occurs:

{{% colour %}}
{{< katex display=true >}}
P(	ext{neither})=1-P(A\cup B)=1-0.76=0.24
{{< /katex >}}
{{% /colour %}}

---

## 6) Total probability theorem

Sometimes we split the world into cases {{< katex >}}B_1,B_2,\dots,B_k{{< /katex >}} that:
- are mutually exclusive
- cover the whole sample space

Then for event {{< katex >}}A{{< /katex >}}:

{{% colour %}}
{{< katex display=true >}}
P(A)=\sum_{i=1}^{k} P(A\mid B_i)\,P(B_i)
{{< /katex >}}
{{% /colour %}}

Plain meaning:
overall probability is a weighted average of conditional probabilities across cases.

### Tree intuition (Mermaid)

{{< mermaid >}}
flowchart TD
  S[Start] --> B1[Case B1]
  S --> B2[Case B2]
  S --> B3[Case B3]

  B1 --> A1[A happens]
  B2 --> A2[A happens]
  B3 --> A3[A happens]

  A1 --- W1["P(B1) × P(A|B1)"]
  A2 --- W2["P(B2) × P(A|B2)"]
  A3 --- W3["P(B3) × P(A|B3)"]
{{< /mermaid >}}

---

## 7) Worked example (overall probability using total probability)

Example:
users come from three device types:
- Desktop {{< katex >}}D{{< /katex >}}
- Tablet {{< katex >}}T{{< /katex >}}
- Mobile {{< katex >}}M{{< /katex >}}

Given:
{{< katex >}}P(D)=0.5{{< /katex >}}, {{< katex >}}P(T)=0.2{{< /katex >}}, {{< katex >}}P(M)=0.3{{< /katex >}}  
{{< katex >}}P(C\mid D)=0.04{{< /katex >}}, {{< katex >}}P(C\mid T)=0.06{{< /katex >}}, {{< katex >}}P(C\mid M)=0.10{{< /katex >}}

Overall click probability:

{{% colour %}}
{{< katex display=true >}}
P(C)=P(C\mid D)P(D)+P(C\mid T)P(T)+P(C\mid M)P(M)
{{< /katex >}}
{{% /colour %}}

{{% colour %}}
{{< katex display=true >}}
P(C)=0.5(0.04)+0.2(0.06)+0.3(0.10)=0.062
{{< /katex >}}
{{% /colour %}}

{{% hint info %}}
Even if mobile has the highest click rate, the overall click rate depends on how common each device is.
{{% /hint %}}

---

## 8) Bayes’ theorem

Bayes’ theorem reverses conditioning.

It answers:
“Given evidence {{< katex >}}B{{< /katex >}}, how likely is cause {{< katex >}}A{{< /katex >}}?”

{{% colour %}}
{{< katex display=true >}}
P(A\mid B)=rac{P(B\mid A)\,P(A)}{P(B)}
{{< /katex >}}
{{% /colour %}}

Interpretation:
- Prior: {{< katex >}}P(A){{< /katex >}} (belief before evidence)
- Likelihood: {{< katex >}}P(B\mid A){{< /katex >}} (how evidence behaves if A is true)
- Evidence: {{< katex >}}P(B){{< /katex >}} (overall chance of seeing B)
- Posterior: {{< katex >}}P(A\mid B){{< /katex >}} (updated belief)

{{% hint info %}}
Bayes update:
Prior + Evidence → Posterior
{{% /hint %}}

---

## 9) Bayes with multiple causes (using total probability)

If {{< katex >}}A_1,\dots,A_k{{< /katex >}} are possible causes of the same evidence {{< katex >}}B{{< /katex >}}, then:

{{% colour %}}
{{< katex display=true >}}
P(A_i\mid B)=rac{P(B\mid A_i)\,P(A_i)}{\sum_{j=1}^{k} P(B\mid A_j)\,P(A_j)}
{{< /katex >}}
{{% /colour %}}

This is the form used in:
diagnosis, fault detection, and Naïve Bayes classification.

---

## 10) Worked example (communication channel)

A binary channel:
- {{< katex >}}P(T1)=0.4{{< /katex >}} (a 1 is transmitted 40% of the time)
- A transmitted 0 is correctly received with probability 0.90
- A transmitted 1 is correctly received with probability 0.95

Let:
- {{< katex >}}T1{{< /katex >}} = “1 transmitted”, {{< katex >}}T0{{< /katex >}} = “0 transmitted”
- {{< katex >}}R1{{< /katex >}} = “1 received”

Then:
- {{< katex >}}P(R1\mid T1)=0.95{{< /katex >}}
- {{< katex >}}P(R1\mid T0)=0.10{{< /katex >}} (because 0 flips to 1 with probability 0.10)
- {{< katex >}}P(T0)=0.6{{< /katex >}}

First, probability of receiving a 1:

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

Now, given you received a 1, probability that a 1 was transmitted:

{{% colour %}}
{{< katex display=true >}}
P(T1\mid R1)=rac{P(R1\mid T1)P(T1)}{P(R1)}=rac{0.95(0.4)}{0.44}pprox 0.864
{{< /katex >}}
{{% /colour %}}

{{% hint warning %}}
Even after receiving a 1, there is still a chance it was actually a 0 that flipped during transmission.
{{% /hint %}}

---

## 11) Machine learning connection: Naïve Bayes

Naïve Bayes uses Bayes’ theorem plus a simplifying assumption:
features are conditionally independent given the class.

For class {{< katex >}}Y{{< /katex >}} and features {{< katex >}}X_1,\dots,X_n{{< /katex >}}:

{{% colour %}}
{{< katex display=true >}}
P(Y\mid X_1,\dots,X_n)\propto P(Y)\prod_{i=1}^{n} P(X_i\mid Y)
{{< /katex >}}
{{% /colour %}}

What this gives you:
- fast training and prediction
- a strong baseline for text classification (spam/ham)

{{% hint warning %}}
The “naïve” part:
features are rarely truly independent.
But the model can still work well in practice, especially for high-dimensional sparse data (like text).
{{% /hint %}}

---

## Quick summary

- Conditional probability:
updates probability after an event is known.
- Multiplication rule:
computes joint probability from conditional parts.
- Independence:
tested using {{< katex >}}P(A\cap B)=P(A)P(B){{< /katex >}}.
- Total probability:
breaks a probability into weighted cases.
- Bayes’ theorem:
reverses conditioning to infer causes from evidence.

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

- [Conditional Probability vs Bayes’ Theorem – GeeksforGeeks](https://www.geeksforgeeks.org/maths/conditional-probability-vs-bayes-theorem/)

---

{{< home-link "Home" >}} | {{< section-index >}}

---
