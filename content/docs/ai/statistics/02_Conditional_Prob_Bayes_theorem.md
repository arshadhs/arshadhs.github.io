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
Conditional probability updates probabilities after observing an event.
Bayes’ theorem lets you estimate a hidden cause from observed evidence.
Naïve Bayes turns Bayes’ theorem into a practical classifier by assuming conditional independence of features given the class.
{{% /hint %}}

---

## Section 1 — Conditional Probability (and Independent Events)

### 1.1 Conditional probability

Conditional probability answers:
What is the probability of event {{< katex >}}B{{< /katex >}}, given that {{< katex >}}A{{< /katex >}} has occurred?

{{% colour %}}
{{< katex display=true >}}
P(B \mid A)=rac{P(A\cap B)}{P(A)},\quad P(A)>0
{{< /katex >}}
{{% /colour %}}

How to think about it:
Meaning:
- {{< katex >}}P(A\cap B){{< /katex >}} is the probability that both events happen together (joint probability).
- Conditioning on {{< katex >}}A{{< /katex >}} means:
we restrict attention to the outcomes where {{< katex >}}A{{< /katex >}} is true.

{{% hint info %}}
Conditioning “shrinks the universe”:
once {{< katex >}}A{{< /katex >}} is known to have happened, we only count outcomes inside {{< katex >}}A{{< /katex >}}.
{{% /hint %}}

Conditioning on {{< katex >}}A{{< /katex >}} means we restrict our attention to outcomes inside {{< katex >}}A{{< /katex >}}.
---

### 1.2 Multiplication rule (joint probability)

Start from the definition:

{{% colour %}}
{{< katex display=true >}}
P(B \mid A)=rac{P(A\cap B)}{P(A)}
{{< /katex >}}
{{% /colour %}}

Rearranging the definition gives the multiplication rule:

{{% colour %}}
{{< katex display=true >}}
P(A\cap B)=P(A)\,P(B\mid A)
{{< /katex >}}
{{% /colour %}}

Similarly:

{{% colour %}}
{{< katex display=true >}}
P(A\cap B)=P(B)\,P(A\mid B)
{{< /katex >}}
{{% /colour %}}

For three events (chain form):

{{% colour %}}
{{< katex display=true >}}
P(A\cap B\cap C)=P(A)\,P(B\mid A)\,P(C\mid A\cap B)
{{< /katex >}}
{{% /colour %}}

This chain form is the backbone of many probability models (including ML sequence models).
{{% hint warning %}}
This is the bridge between:
conditional probability and joint probability.
In ML, this “chain idea” shows up when modelling sequences and joint distributions.
{{% /hint %}}

---

### 1.3 Conditional probability of independent events

Two events {{< katex >}}A{{< /katex >}} and {{< katex >}}B{{< /katex >}} are independent if knowing one does not change the probability of the other.

Equivalent tests:

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
Mutually exclusive events cannot be independent unless one of them is impossible (unless one has probability 0).
If two events cannot happen together, learning one occurred forces the other to be false.
{{% /hint %}}

If {{< katex >}}A\cap B=arnothing{{< /katex >}}, then {{< katex >}}P(A\cap B)=0{{< /katex >}},
but {{< katex >}}P(A)P(B){{< /katex >}} is positive if both events can occur.

---

### 1.4 A quick “independent events” example (engineering style)

If a washer needs service with probability 0.30 and a dryer needs service with probability 0.10, and they are assumed independent, then the probability both need service is found by multiplying the probabilities.  

{{% colour %}}
{{< katex display=true >}}
P(	ext{both})=0.30	imes 0.10=0.03
{{< /katex >}}
{{% /colour %}}

---

### 1.5 When independence fails (why conditional probability matters)

If {{< katex >}}P(B\mid A)
eq P(B){{< /katex >}}, then A changes B and the events are dependent.

{{< mermaid >}}
flowchart LR
  A["Know A happened"] --> B{"Does B change?"}
  B -->|No| C["Independent: use multiplication P(A∩B)=P(A)P(B)"]
  B -->|Yes| D["Dependent: compute P(B|A) using joint/total probability"]
{{< /mermaid >}}

---

## Section 2 — Bayes’ Theorem

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

Bayes’ theorem “reverses” conditioning:

{{% colour %}}
{{< katex display=true >}}
P(A\mid B)=rac{P(B\mid A)\,P(A)}{P(B)},\quad P(B)>0
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

### 2.3 Bayes’ theorem (general partition form)

If {{< katex >}}E_1,\dots,E_k{{< /katex >}} is a partition and B is observed, then:

{{% colour %}}
{{< katex display=true >}}
P(E_j\mid B)=rac{P(B\mid E_j)\,P(E_j)}{\sum_{i=1}^{k} P(B\mid E_i)\,P(E_i)}
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
P(D\mid +)=rac{0.99(0.001)}{0.02097}pprox 0.047
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
P(T1\mid R1)=rac{P(R1\mid T1)P(T1)}{P(R1)}=rac{0.95(0.4)}{0.44}pprox 0.864
{{< /katex >}}
{{% /colour %}}

---

## Section 3 — Naïve Bayes

### 3.1 From Bayes to learning (posterior over classes)

In classification, we want:
class probability given features.

For a class {{< katex >}}Y{{< /katex >}} and observed features {{< katex >}}X{{< /katex >}}:

{{% colour %}}
{{< katex display=true >}}
P(Y\mid X)=rac{P(X\mid Y)\,P(Y)}{P(X)}
{{< /katex >}}
{{% /colour %}}

This is a generative viewpoint:
model {{< katex >}}P(X\mid Y){{< /katex >}} and {{< katex >}}P(Y){{< /katex >}}, then infer {{< katex >}}P(Y\mid X){{< /katex >}}.

---

### 3.2 Conditional independence (the “naïve” assumption)

Naïve Bayes assumes:
features are conditionally independent given the class.

Meaning:
once you know the class, learning one feature does not change the probability of another feature.

{{% hint info %}}
Why it helps:
Without the independence assumption, modelling the full joint probability of many features becomes expensive.
With it, we can multiply simple 1-feature probabilities.
{{% /hint %}}

---

### 3.3 Naïve Bayes formula (multiple features)

For features {{< katex >}}X_1,\dots,X_n{{< /katex >}} and class {{< katex >}}Y{{< /katex >}}:

{{% colour %}}
{{< katex display=true >}}
P(Y\mid X_1,\dots,X_n)\propto P(Y)\prod_{i=1}^{n} P(X_i\mid Y)
{{< /katex >}}
{{% /colour %}}

We use “proportional to” because {{< katex >}}P(X_1,\dots,X_n){{< /katex >}} is the same for all classes when comparing classes.

Decision rule (MAP classification):

{{% colour %}}
{{< katex display=true >}}
\hat{y}=rg\max_{y} \; P(y)\prod_{i=1}^{n} P(x_i\mid y)
{{< /katex >}}
{{% /colour %}}

---

### 3.4 How to build a Naïve Bayes classifier from data

Steps (the same workflow used in the session examples):
1) Collect raw data  
2) Convert to frequency tables  
3) Convert counts to probabilities  
4) Plug into Bayes / Naïve Bayes and compare class scores

---

### 3.5 Worked example A (Play Tennis with one feature: Weather)

Goal:
If today is Sunny, predict Play = Yes or No.

From the frequency table:
- {{< katex >}}P(Yes)=9/14{{< /katex >}}, {{< katex >}}P(No)=5/14{{< /katex >}}
- Weather counts:
Sunny has 3 Yes and 2 No, so:
{{< katex >}}P(Sunny\mid Yes)=3/9{{< /katex >}},
{{< katex >}}P(Sunny\mid No)=2/5{{< /katex >}}

Compute scores:

{{% colour %}}
{{< katex display=true >}}
	ext{Score(Yes)}=P(Yes)\,P(Sunny\mid Yes)=rac{9}{14}\cdotrac{3}{9}
{{< /katex >}}
{{% /colour %}}

{{% colour %}}
{{< katex display=true >}}
	ext{Score(No)}=P(No)\,P(Sunny\mid No)=rac{5}{14}\cdotrac{2}{5}
{{< /katex >}}
{{% /colour %}}

Compare the two scores:
the larger score gives the predicted class.

{{% hint info %}}
You do not need to compute the denominator P(Sunny) when comparing classes.
The denominator is the same for both classes.
{{% /hint %}}

---

### 3.6 Worked example B (Play Tennis with multiple features)

Today:
Outlook = Sunny, Temp = Hot, Humidity = Normal, Windy = False

Use:
- Prior:
{{< katex >}}P(Yes)=9/14{{< /katex >}}, {{< katex >}}P(No)=5/14{{< /katex >}}
- Likelihood terms from the tables, e.g.
{{< katex >}}P(Outlook=Sunny\mid Yes)=3/9{{< /katex >}},
{{< katex >}}P(Temp=Hot\mid Yes)=2/9{{< /katex >}},
{{< katex >}}P(Humidity=Normal\mid Yes)=6/9{{< /katex >}},
{{< katex >}}P(Windy=False\mid Yes)=6/9{{< /katex >}},
and similarly for No.

Score each class:

{{% colour %}}
{{< katex display=true >}}
	ext{Score(Yes)}=P(Yes)\,P(Sunny\mid Yes)\,P(Hot\mid Yes)\,P(Normal\mid Yes)\,P(False\mid Yes)
{{< /katex >}}
{{% /colour %}}

{{% colour %}}
{{< katex display=true >}}
	ext{Score(No)}=P(No)\,P(Sunny\mid No)\,P(Hot\mid No)\,P(Normal\mid No)\,P(False\mid No)
{{< /katex >}}
{{% /colour %}}

Pick the larger score.

{{% hint warning %}}
Zero-frequency problem:
If a feature value never appears with a class in your training data, the probability can become zero and the whole product becomes zero.
In practice, you use smoothing (e.g., Laplace smoothing) to avoid this.
{{% /hint %}}

---

### 3.7 Naïve Bayes pipeline (Mermaid)

{{< mermaid >}}
flowchart TD
  A[Training data] --> B[Count frequencies per class]
  B --> C[Convert to probabilities]
  C --> D[Compute class scores for new X]
  D --> E{Pick max score}
  E --> F[Predicted class]
{{< /mermaid >}}

---

## Mini-check (self-test)

1) If {{< katex >}}P(A)=0.3{{< /katex >}} and {{< katex >}}P(B\mid A)=0.8{{< /katex >}}, find {{< katex >}}P(A\cap B){{< /katex >}}.  
2) If {{< katex >}}P(B\mid A)=P(B){{< /katex >}}, what does that tell you about A and B?  
3) In Bayes’ theorem, what usually has the biggest impact when an event is rare?

{{% hint success %}}
Answers:
1) Multiply base probability by the conditional probability.
2) The events are independent.
3) The prior probability (base rate).
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

## What’s next

Probability Distributions  
Move from events to random variables and distributions.

---

## Reference

- [Conditional Probability vs Bayes’ Theorem – GeeksforGeeks](https://www.geeksforgeeks.org/maths/conditional-probability-vs-bayes-theorem/)

---

{{< home-link "Home" >}} | {{< section-index >}}

---
