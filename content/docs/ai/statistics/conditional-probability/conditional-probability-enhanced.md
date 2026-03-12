---
title: "Conditional Probability"
draft: false
tags: ["AI", "Statistics", "Probability"]
categories: ["AI", "Statistics"]
weight: 210
menu: main
---

# Conditional Probability and Independent Events

Conditional probability answers:
“How likely is one event, given that another event has already happened?”

It is the maths behind phrases like:
- “given that…”
- “among those who…”
- “out of the people who…”
- “if it did not fail immediately…”

---

## 1.1 Conditional probability

Conditional probability answers:
What is the probability of event $B$, given that $A$ has occurred?

{{< colour "red" >}}
$$
P(B\mid A)=\frac{P(A\cap B)}{P(A)},\quad P(A)>0
$$
{{< /colour >}}

Meaning:
- $P(A\cap B)$ is the probability that both events happen together (joint probability).
- Conditioning on $A$ means:
we restrict attention to outcomes where $A$ is true.

**Conditioning “shrinks the universe”**:
once $A$ is known to have happened, we only count outcomes inside $A$.

A useful way to remember:
- Unconditional probability:
“How likely is $B$ overall?”
- Conditional probability:
“How likely is $B$ inside the subset where $A$ is true?”

---

## 1.2 Multiplication rule (joint probability)

Start from the definition:

{{< colour "red" >}}
$$
P(B\mid A)=\frac{P(A\cap B)}{P(A)}
$$
{{< /colour >}}

Rearranging gives the multiplication rule:

{{< colour "red" >}}
$$
P(A\cap B)=P(A)\,P(B\mid A)
$$
{{< /colour >}}

Similarly:

{{< colour "red" >}}
$$
P(A\cap B)=P(B)\,P(A\mid B)
$$
{{< /colour >}}

For three events (chain form):

{{< colour "red" >}}
$$
P(A\cap B\cap C)=P(A)\,P(B\mid A)\,P(C\mid A\cap B)
$$
{{< /colour >}}

This chain form is the backbone of many probability models.

{{% hint warning %}}
This is the bridge between:
conditional probability and joint probability.

In ML, this “chain idea” shows up when modelling sequences and joint distributions.
{{% /hint %}}

---

## 1.3 Conditional probability of independent events

Two events $A$ and $B$ are independent if knowing one does not change the probability of the other.

Equivalent tests:

{{< colour "red" >}}
$$
P(B\mid A)=P(B)
$$
{{< /colour >}}

{{< colour "red" >}}
$$
P(A\mid B)=P(A)
$$
{{< /colour >}}

{{< colour "red" >}}
$$
P(A\cap B)=P(A)\,P(B)
$$
{{< /colour >}}

{{% hint warning %}}
Common confusion:
Mutually exclusive events cannot be independent unless one has probability 0.

If $A\cap B=\varnothing$, then $P(A\cap B)=0$,
but $P(A)P(B)$ is positive if both events can occur.
So they cannot be equal.
{{% /hint %}}

---

## 1.4 A quick independent-events example

If a washer needs service with probability 0.30 and a dryer needs service with probability 0.10, and they are assumed independent, then:

{{< colour "red" >}}
$$
P(\text{both})=0.30\times 0.10=0.03
$$
{{< /colour >}}

Interpretation:
independence allows multiplication.

---

## 1.5 When independence fails (why conditional probability matters)

If $P(B\mid A)\neq P(B)$, then $A$ changes $B$ and the events are dependent.

{{< mermaid >}}
flowchart LR
  A["Know A happened"] --> B{"Does B change?"}
  B -->|No| C["Independent: use product rule"]
  B -->|Yes| D["Dependent: compute using conditional probability"]
{{< /mermaid >}}

---

## 1.6 Quiz-style examples (how to spot conditional probability fast)

### Example A: “eligible given female”

A company interview has 1000 aspirants, 500 females.
25% of females are eligible.

Question:
Probability that the aspirant is eligible given the aspirant is female.

{{< colour "red" >}}
$$
P(\text{Eligible}\mid \text{Female})=\frac{\text{eligible females}}{\text{total females}}
=\frac{0.25\times 500}{500}=0.25=\frac{1}{4}
$$
{{< /colour >}}

Key reading skill:
“given female” means the sample space becomes “females only”.

---

### Example B: “fraction of those who passed first also passed second”

Given:
- 35% passed both exams: $P(A\cap B)=0.35$
- 42% passed the first exam: $P(A)=0.42$

Question:
Fraction of those who passed the first who also passed the second.

That is $P(B\mid A)$:

{{< colour "red" >}}
$$
P(B\mid A)=\frac{P(A\cap B)}{P(A)}=\frac{0.35}{0.42}=\frac{35}{42}=\frac{5}{6}
$$
{{< /colour >}}

Key reading skill:
“of those who passed the first” means we are conditioning on “passed the first”.

---

### Example C: “does not fail immediately”

Bulbs:
7 fail immediately,
8 fail after a couple of hours,
24 acceptable.

Given:
it does not fail immediately.

Then the possible bulbs are:
partially defective or acceptable ($8+24=32$).

So:

{{< colour "red" >}}
$$
P(\text{acceptable}\mid \text{not immediate fail})=\frac{24}{24+8}=\frac{24}{32}=0.75
$$
{{< /colour >}}

Key reading skill:
“given it does not fail immediately” removes the “fails immediately” bulbs from the sample space.

---

## 1.7 Two common exam traps (learned from quizzes)

### Trap 1: “$P(C\mid B)+P(C\mid B^c)=1$”

This is not true in general.

What is true is the total probability form:

{{< colour "red" >}}
$$
P(C)=P(C\mid B)P(B)+P(C\mid B^c)P(B^c)
$$
{{< /colour >}}

So $P(C\mid B)$ and $P(C\mid B^c)$ only “add nicely” when weighted by $P(B)$ and $P(B^c)$.

---

### Trap 2: Independence and complements

If $A$ and $B$ are independent, then complements are also independent:

- $A^c$ and $B$ are independent
- $A$ and $B^c$ are independent
- $A^c$ and $B^c$ are independent

One quick check (example form):

{{< colour "red" >}}
$$
P(B\mid A)=P(B)\ \Rightarrow\ P(B\mid A^c)=P(B)
$$
{{< /colour >}}

---

## Mini-check (self-test)

1) If $P(A)=0.4$ and $P(B)=0.3$ and $A,B$ are independent, find $P(A\cap B)$.  
2) If $P(A\cap B)=0.35$ and $P(A)=0.42$, find $P(B\mid A)$.  
3) If a statement says “given that $A$ happened”, what becomes the new sample space?

Answers:
1) $0.4\times 0.3=0.12$  
2) $\frac{0.35}{0.42}=\frac{5}{6}$  
3) The set of outcomes where $A$ is true.

---

{{< home-link "Home" >}} | {{< section-index >}}

---
