---
title: "Conditional Probability"
draft: false
tags: ["AI", "Statistics", "Probability"]
categories: ["AI", "Statistics"]
weight: 210
menu: main
---

# Conditional Probability (and Independent Events)

- Prior Probablity: No condition
- Posterior Probability: Same Event, but now with new knowledge i.e. with Condition

### 1.1 Conditional probability

Conditional probability answers:
What is the probability of event {{< katex >}}B{{< /katex >}}, given that {{< katex >}}A{{< /katex >}} has already occurred?

{{% hint info %}}
Key takeaway:
Conditional probability is always
(joint probability) ÷ (probability of the condition).
The condition must not be an impossible event.
{{% /hint %}}

It is the maths behind phrases like:
- “given that…”
- “among those who…”
- “out of the people who…”
- “if it did not fail immediately…”

Notation:
{{< katex >}}P(B\mid A){{< /katex >}} is read as “B given A” (not “B by A”).
{{% colour %}}
{{< katex display=true >}}
P(B \mid A)=\frac{P(A\cap B)}{P(A)},\quad P(A)>0
{{< /katex >}}
{{% /colour %}}

How to think about it:
Meaning:
- Numerator: {{< katex >}}P(A\cap B){{< /katex >}} is the probability that both events happen together (**joint probability**).
- Denominator: Conditioning on {{< katex >}}A{{< /katex >}} means:
we restrict attention to the outcomes where {{< katex >}}A{{< /katex >}} is true.

**Conditioning “shrinks the universe”**: once {{< katex >}}A{{< /katex >}}is known to have happened, we only count outcomes inside {{< katex >}}A{{< /katex >}}.

Conditioning on {{< katex >}}A{{< /katex >}} means we restrict our attention to outcomes inside {{< katex >}}A{{< /katex >}}.

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

### 1.2 Multiplication rule (joint probability)

Start from the definition:

{{% colour %}}
{{< katex display=true >}}
P(B \mid A)=\frac{P(A\cap B)}{P(A)}
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

Rule of thumb:
every time you add a new event,
the condition becomes the intersection of all previous events.

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
Mutually exclusive events cannot be independent unless one of them is impossible (unless one has probability 0).
If two events cannot happen together, learning one occurred forces the other to be false.
{{% /hint %}}

If {{< katex >}}A\cap B = \varnothing{{< /katex >}}, then {{< katex >}}P(A\cap B)=0{{< /katex >}},
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

{{< home-link "Home" >}} | {{< section-index >}}

---
