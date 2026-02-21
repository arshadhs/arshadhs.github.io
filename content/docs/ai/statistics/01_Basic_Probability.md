---
title: "Basic Probability"
draft: false
tags: ["AI", "Statistics"]
categories: ["AI", "Statistics"]
weight: 110
menu: main
---

# Basic Probability

Probability models uncertainty:
what you *don’t know* yet, but want to reason about.

{{% hint info %}}
Key takeaway:
Probability is a number between **0 and 1** that measures how likely an event is.
The whole topic is about defining **events** clearly and applying a few core rules consistently.
{{% /hint %}}

Probability quantifies uncertainty: a number between 0 and 1.

- 0 means: impossible
- 1 means: certain

---

## Terminology

### Random experiment

A random experiment is an action whose outcome is not known in advance.

Examples:
- Flip a coin (head/tail)
- Roll a die (1 to 6)
- Wait time at a bus stop
- Number of customers arriving in an hour
- Pulling a card from a deck

{{% hint warning %}}
A random experiment is not “random because we don’t care”.
It is random because the outcome is **uncertain before performing it**.
{{% /hint %}}

---

### Sample space {{< katex >}}S{{< /katex >}}

Sample space:
the set of **all possible outcomes**.

Examples:
- Die roll:
{{< katex >}}S=\{1,2,3,4,5,6\}{{< /katex >}}
- Quality test (accepted/rejected):
{{< katex >}}S=\{a,r\}{{< /katex >}}

---

### Event {{< katex >}}A{{< /katex >}}

Event:
a **subset of the sample space**.

It can be:
- Empty event:
{{< katex >}}\varnothing{{< /katex >}}
- Certain event:
{{< katex >}}S{{< /katex >}}
- Any meaningful subset of outcomes

Example (die roll):
- Event “even”:
{{< katex >}}A=\{2,4,6\}{{< /katex >}}

---

## Operations on events (set operations)

### Complement

Complement:
event “not {{< katex >}}A{{< /katex >}}”.

{{% colour %}}
{{< katex display=true >}}
A^c = S \setminus A
{{< /katex >}}
{{% /colour %}}

{{% colour %}}
{{< katex display=true >}}
P(A^c)=1-P(A)
{{< /katex >}}
{{% /colour %}}

---

### Union

Union:
{{< katex >}}A\cup B{{< /katex >}} means “{{< katex >}}A{{< /katex >}} OR {{< katex >}}B{{< /katex >}} OR both”.

{{% colour %}}
{{< katex display=true >}}
A \cup B = \{ \omega : \omega \in A \text{ or } \omega \in B \}
{{< /katex >}}
{{% /colour %}}

---

### Intersection

Intersection:
{{< katex >}}A\cap B{{< /katex >}} means “{{< katex >}}A{{< /katex >}} AND {{< katex >}}B{{< /katex >}}”.

{{% colour %}}
{{< katex display=true >}}
A \cap B = \{ \omega : \omega \in A \text{ and } \omega \in B \}
{{< /katex >}}
{{% /colour %}}

---

## Probability as a numerical measure

Probability scale:
- 0:
impossible
- 0.5:
equally likely to happen or not happen
- 1:
certain

{{% hint info %}}
If you ever compute a probability less than 0 or greater than 1, something is wrong.
{{% /hint %}}

---

## Three common “definitions” of probability

### 1) Classical approach (equally likely outcomes)

Used when outcomes are equally likely.

{{% colour %}}
{{< katex display=true >}}
P(A)=\frac{\text{number of favourable outcomes}}{\text{total number of possible outcomes}}
{{< /katex >}}
{{% /colour %}}

Example:
rolling an even number on a fair die.

{{% colour %}}
{{< katex display=true >}}
P(\text{even})=\frac{3}{6}=\frac{1}{2}
{{< /katex >}}
{{% /colour %}}

---

### 2) Empirical approach (relative frequency)

Used when probability is estimated from repeated observations.

{{% colour %}}
{{< katex display=true >}}
P(A)\approx\frac{\text{number of times event occurs}}{\text{total number of trials}}
{{< /katex >}}
{{% /colour %}}

Intuition:
with many trials, empirical probability tends to stabilise.

---

### 3) Axiomatic approach (most general)

Probability is assigned to events in a way that satisfies the axioms below.
This is the approach used throughout probability theory.

---

## Axioms of probability

For any event {{< katex >}}A{{< /katex >}} in sample space {{< katex >}}S{{< /katex >}}:

### 1) Non-negativity

{{% colour %}}
{{< katex display=true >}}
P(A)\ge 0
{{< /katex >}}
{{% /colour %}}

### 2) Normalisation

{{% colour %}}
{{< katex display=true >}}
P(S)=1
{{< /katex >}}
{{% /colour %}}

### 3) Additivity (for mutually exclusive events)

If {{< katex >}}A\cap B=\varnothing{{< /katex >}}, then:

{{% colour %}}
{{< katex display=true >}}
P(A\cup B)=P(A)+P(B)
{{< /katex >}}
{{% /colour %}}

{{% hint info %}}
These axioms are “rules of the game”.
Most probability formulas you use later are consequences of these.
{{% /hint %}}

---

## The Addition Rule (general case)

Even if {{< katex >}}A{{< /katex >}} and {{< katex >}}B{{< /katex >}} overlap, the union probability is:

{{% colour %}}
{{< katex display=true >}}
P(A\cup B)=P(A)+P(B)-P(A\cap B)
{{< /katex >}}
{{% /colour %}}

Why we subtract:
the overlap {{< katex >}}A\cap B{{< /katex >}} gets counted twice if we only add {{< katex >}}P(A){{< /katex >}} and {{< katex >}}P(B){{< /katex >}}.

---

## Mutually exclusive vs independent (do not confuse)

### Mutually exclusive (disjoint)

Cannot happen together.

{{% colour %}}
{{< katex display=true >}}
A\cap B=\varnothing
{{< /katex >}}
{{% /colour %}}

So:

{{% colour %}}
{{< katex display=true >}}
P(A\cap B)=0
{{< /katex >}}
{{% /colour %}}

Example (one die roll):
- {{< katex >}}A{{< /katex >}}: roll a 2
- {{< katex >}}B{{< /katex >}}: roll a 5  
They cannot occur together.

#### Collective Exaustive

Two events A and B are Mutually Exclusive, but other than A and B there is nothing left.

---

### Independent

One event happening does not change the probability of the other.

{{% colour %}}
{{< katex display=true >}}
P(A\cap B)=P(A)\,P(B)
{{< /katex >}}
{{% /colour %}}

Example:
- First coin toss is heads
- Second coin toss is heads  
These are independent.

{{% hint warning %}}
Mutually exclusive events are usually NOT independent (unless one event has probability 0).
If {{< katex >}}A\cap B=\varnothing{{< /katex >}} and both have positive probability, then {{< katex >}}P(A\cap B)=0{{< /katex >}} but {{< katex >}}P(A)P(B)>0{{< /katex >}}.
So they cannot be equal.
{{% /hint %}}

---

## Visual: deciding the relationship between events

{{< mermaid >}}
flowchart LR
  A["Two events: A and B"] --> B{"Can A and B happen together?"}
  B -->|No| C["Mutually exclusive\nA ∩ B = ∅"]
  B -->|Yes| D{"Does knowing A change B?"}
  D -->|No| E["Independent\nP(A ∩ B)=P(A)P(B)"]
  D -->|Yes| F["Dependent\nUse conditional probability next"]
{{< /mermaid >}}

---

## Worked patterns you should recognise

### Pattern 1: Valid probability assignment

If outcomes {{< katex >}}A,B,C,\dots{{< /katex >}} are mutually exclusive and exhaustive, then a valid assignment must satisfy:
- Each probability is between 0 and 1
- Total must sum to 1

{{% hint success %}}
Checklist:
1) Are all {{< katex >}}P(\cdot)\ge 0{{< /katex >}}?
2) Is {{< katex >}}\sum P(\text{outcomes})=1{{< /katex >}}?
If both yes, it is permissible.
{{% /hint %}}

---

### Pattern 2: “At least one” trick

“At least one of X” is often easiest using complement.

Example idea:
“At least one desktop”  
= 1 − P(no desktop)  
= 1 − P(both are laptops)

{{% hint info %}}
Whenever you see:
“At least one…”
try the complement first.
{{% /hint %}}

---

## Mini-check (self-test)

1) If {{< katex >}}P(A)=0.4{{< /katex >}} and {{< katex >}}P(B)=0.3{{< /katex >}} and {{< katex >}}A,B{{< /katex >}} are independent, find {{< katex >}}P(A\cap B){{< /katex >}}.  
2) If {{< katex >}}A,B{{< /katex >}} are mutually exclusive, what is {{< katex >}}P(A\cap B){{< /katex >}}?  
3) If {{< katex >}}P(A)=0.7{{< /katex >}}, what is {{< katex >}}P(A^c){{< /katex >}}?  
4) If {{< katex >}}P(A)=0.6{{< /katex >}}, {{< katex >}}P(B)=0.5{{< /katex >}}, and {{< katex >}}P(A\cap B)=0.2{{< /katex >}}, find {{< katex >}}P(A\cup B){{< /katex >}}.

{{% hint success %}}
Answers:
1) {{< katex >}}0.4\times 0.3=0.12{{< /katex >}}  
2) {{< katex >}}0{{< /katex >}}  
3) {{< katex >}}1-0.7=0.3{{< /katex >}}  
4) {{< katex >}}0.6+0.5-0.2=0.9{{< /katex >}}
{{% /hint %}}

---

## What’s next

**Conditional Probability & Bayes’ Theorem**  
This is where “given what I already know…” becomes mathematics, and where Naïve Bayes begins.

---

{{< home-link "Home" >}} | {{< section-index >}}

---
