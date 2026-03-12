---
title: "Conditional Probability"
draft: false
tags: ["AI", "Statistics", "Probability"]
categories: ["AI", "Statistics"]
weight: 210
menu: main
---

# Conditional Probability and Independent Events

In probability, the number you assign to an event can change once you learn new information.

That is exactly what conditional probability captures:
updating probability when a “conditioning event” has already happened.

{{% hint info %}}
Key takeaway:
Conditional probability is always
(joint probability) ÷ (probability of the condition).
The condition must not be an impossible event.
{{% /hint %}}

---

## Why conditional probability exists

In many real situations:

- You assign probabilities before seeing any extra information.
- Later, partial information becomes available.
- You then revise your probability assignment based on that information.

Typical examples:
- Medical diagnosis:
the probability of a disease changes after a test result.
- Quality control:
the probability that an item is acceptable changes once it passes an initial check.
- Reliability / systems:
the probability of failure changes once you know one component has failed or survived.

---

## 1.1 Conditional probability

Conditional probability answers:
What is the probability of event {{< katex >}}B{{< /katex >}}, given that {{< katex >}}A{{< /katex >}} has already occurred?

Notation:
{{< katex >}}P(B\mid A){{< /katex >}} is read as “B given A” (not “B by A”).

Definition:

{{< colour "red" >}}
{{< katex display=true >}}
P(B\mid A)=\frac{P(A\cap B)}{P(A)},\quad P(A)>0
{{< /katex >}}
{{< /colour >}}

Meaning:
- Numerator {{< katex >}}P(A\cap B){{< /katex >}}:
the probability both happen together (joint probability).
- Denominator {{< katex >}}P(A){{< /katex >}}:
the probability of the condition (the “background event”).

A key point from lectures:
the condition must not be impossible.
If {{< katex >}}P(A)=0{{< /katex >}}, then {{< katex >}}P(B\mid A){{< /katex >}} is not defined.

How to think about it:
conditioning “shrinks the universe”.
Once {{< katex >}}A{{< /katex >}} is known to have happened, we only look inside the outcomes where {{< katex >}}A{{< /katex >}} is true.

---

### Conditional probability the other way round

If the event of interest is {{< katex >}}A{{< /katex >}} and the condition is {{< katex >}}B{{< /katex >}}:

{{< colour "red" >}}
{{< katex display=true >}}
P(A\mid B)=\frac{P(A\cap B)}{P(B)},\quad P(B)>0
{{< /katex >}}
{{< /colour >}}

Important:
the joint probability {{< katex >}}P(A\cap B){{< /katex >}} stays the same,
but the denominator changes depending on which event is used as the condition.

---

## 1.2 Multiplication rule (joint probability)

Start from the definition:

{{< colour "red" >}}
{{< katex display=true >}}
P(B\mid A)=\frac{P(A\cap B)}{P(A)}
{{< /katex >}}
{{< /colour >}}

Rearranging gives the multiplication rule:

{{< colour "red" >}}
{{< katex display=true >}}
P(A\cap B)=P(A)\,P(B\mid A)
{{< /katex >}}
{{< /colour >}}

Similarly:

{{< colour "red" >}}
{{< katex display=true >}}
P(A\cap B)=P(B)\,P(A\mid B)
{{< /katex >}}
{{< /colour >}}

Lecture emphasis:
this multiplication rule works for any two events in the sample space:
independent or dependent.

It is different from the special independence rule
{{< katex >}}P(A\cap B)=P(A)P(B){{< /katex >}},
which only applies when events are independent.

---

### Chain form for three or more events

For three events:

{{< colour "red" >}}
{{< katex display=true >}}
P(A\cap B\cap C)=P(A)\,P(B\mid A)\,P(C\mid A\cap B)
{{< /katex >}}
{{< /colour >}}

For four events, the idea continues:

{{< colour "red" >}}
{{< katex display=true >}}
P(A\cap B\cap C\cap D)=P(A)\,P(B\mid A)\,P(C\mid A\cap B)\,P(D\mid A\cap B\cap C)
{{< /katex >}}
{{< /colour >}}

Rule of thumb:
every time you add a new event,
the condition becomes the intersection of all previous events.

{{% hint warning %}}
This is the bridge between:
conditional probability and joint probability.

In ML, this “chain idea” shows up when modelling sequences and joint distributions.
{{% /hint %}}

---

## 1.3 Conditional probability of independent events

Two events {{< katex >}}A{{< /katex >}} and {{< katex >}}B{{< /katex >}} are independent if knowing one does not change the probability of the other.

Equivalent tests:

{{< colour "red" >}}
{{< katex display=true >}}
P(B\mid A)=P(B)
{{< /katex >}}
{{< /colour >}}

{{< colour "red" >}}
{{< katex display=true >}}
P(A\mid B)=P(A)
{{< /katex >}}
{{< /colour >}}

{{< colour "red" >}}
{{< katex display=true >}}
P(A\cap B)=P(A)\,P(B)
{{< /katex >}}
{{< /colour >}}

---

### Do not confuse: mutually exclusive vs independent

Mutually exclusive means they cannot happen together:

{{< colour "red" >}}
{{< katex display=true >}}
A\cap B=\varnothing
{{< /katex >}}
{{< /colour >}}

So:

{{< colour "red" >}}
{{< katex display=true >}}
P(A\cap B)=0
{{< /katex >}}
{{< /colour >}}

{{% hint warning %}}
Common confusion:
Mutually exclusive events cannot be independent unless one of them has probability 0.

If {{< katex >}}A\cap B=\varnothing{{< /katex >}} and both are possible,
then {{< katex >}}P(A)P(B)>0{{< /katex >}} but {{< katex >}}P(A\cap B)=0{{< /katex >}}.
So independence cannot hold.
{{% /hint %}}

---

## 1.4 Quick independent-events example (engineering style)

If a washer needs service with probability 0.30 and a dryer needs service with probability 0.10, and they are assumed independent, then:

{{< colour "red" >}}
{{< katex display=true >}}
P(\text{both})=0.30\times 0.10=0.03
{{< /katex >}}
{{< /colour >}}

---

## 1.5 A practical conditional probability example (flights)

Given:
- 90% of flights depart on time.
- 80% of flights arrive on time.
- 75% of flights both depart on time and arrive on time.

Let:
- {{< katex >}}D{{< /katex >}}:
depart on time
- {{< katex >}}A{{< /katex >}}:
arrive on time

Then:

{{< colour "red" >}}
{{< katex display=true >}}
P(A\mid D)=\frac{P(A\cap D)}{P(D)}=\frac{0.75}{0.90}=0.8333\ldots
{{< /katex >}}
{{< /colour >}}

Interpretation:
if you know the flight departed on time,
the probability it arrives on time becomes about 0.833.

You can also reverse it:

{{< colour "red" >}}
{{< katex display=true >}}
P(D\mid A)=\frac{P(A\cap D)}{P(A)}=\frac{0.75}{0.80}=0.9375
{{< /katex >}}
{{< /colour >}}

This is a good exam-style reminder:
conditional probabilities change when you reverse the condition.

---

## 1.6 When independence fails (why conditional probability matters)

If:

{{< colour "red" >}}
{{< katex display=true >}}
P(B\mid A)\neq P(B)
{{< /katex >}}
{{< /colour >}}

then {{< katex >}}A{{< /katex >}} changes {{< katex >}}B{{< /katex >}},
and the events are dependent.

{{< mermaid >}}
flowchart LR
  A["We know A happened"] --> B{"Does probability of B change?"}
  B -->|No| C["Independent: use P(A and B)=P(A)P(B)"]
  B -->|Yes| D["Dependent: compute P(B|A) using joint probability"]
{{< /mermaid >}}

---

## 1.7 Two common exam traps

### Trap 1: Adding conditional probabilities

In general:

{{< katex >}}P(C\mid B)+P(C\mid B^c){{< /katex >}}
is not equal to 1.

What is true is the weighted version (law of total probability):

{{< colour "red" >}}
{{< katex display=true >}}
P(C)=P(C\mid B)P(B)+P(C\mid B^c)P(B^c)
{{< /katex >}}
{{< /colour >}}

---

### Trap 2: Complements of independent events

If {{< katex >}}A{{< /katex >}} and {{< katex >}}B{{< /katex >}} are independent, then:
{{< katex >}}A^c{{< /katex >}} and {{< katex >}}B{{< /katex >}} are also independent.

One way to see it quickly is:

{{< colour "red" >}}
{{< katex display=true >}}
P(B\mid A)=P(B)\ \Rightarrow\ P(B\mid A^c)=P(B)
{{< /katex >}}
{{< /colour >}}

This is a common multiple-choice statement.

---

## Mini-check (self-test)

1) What must be true about the condition event for {{< katex >}}P(B\mid A){{< /katex >}} to be defined?  
2) Write {{< katex >}}P(A\cap B){{< /katex >}} using {{< katex >}}P(B\mid A){{< /katex >}} and {{< katex >}}P(A){{< /katex >}}.  
3) If events are independent, what happens to {{< katex >}}P(B\mid A){{< /katex >}}?

{{% hint success %}}
1) The condition must not be impossible: {{< katex >}}P(A)>0{{< /katex >}}.  
2) {{< katex >}}P(A\cap B)=P(A)\,P(B\mid A){{< /katex >}}.  
3) It stays the same: {{< katex >}}P(B\mid A)=P(B){{< /katex >}}.
{{% /hint %}}

---

## What’s next

Total probability and Bayes’ theorem  
This is where you combine conditional probability with a partition of the sample space and “reverse” the conditioning.

---

{{< home-link "Home" >}} | {{< section-index >}}

---
