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

### Sample space

Sample space $S$:

the set of **all possible outcomes**.

Examples:
- Die roll:
  $S=\{1,2,3,4,5,6\}$
- Quality test (accepted/rejected):
  $S=\{a,r\}$

---

### Event

Event $A$:
a **subset of the sample space**.

It can be:
- Empty event:
  $\varnothing$
- Certain event:
  $S$
- Any meaningful subset of outcomes

Example (die roll):
- Event “even”:
  $A=\{2,4,6\}$

---

## Operations on events (set operations)

### Complement

Complement:
event “not $A$”.

{{< colour "red" >}}
$$
A^c = S \setminus A
$$
{{< /colour >}}

{{< colour "red" >}}
$$
P(A^c)=1-P(A)
$$
{{< /colour >}}

---

### Union

Union:
$A\cup B$ means “$A$ OR $B$ OR both”.

{{< colour "red" >}}
$$
A \cup B = \{ \omega : \omega \in A \text{ or } \omega \in B \}
$$
{{< /colour >}}

---

### Intersection

Intersection:
$A\cap B$ means “$A$ AND $B$”.

{{< colour "red" >}}
$$
A \cap B = \{ \omega : \omega \in A \text{ and } \omega \in B \}
$$
{{< /colour >}}

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

{{< colour "red" >}}
$$
P(A)=\frac{\text{number of favourable outcomes}}{\text{total number of possible outcomes}}
$$
{{< /colour >}}

Example:
rolling an even number on a fair die.

{{< colour "red" >}}
$$
P(\text{even})=\frac{3}{6}=\frac{1}{2}
$$
{{< /colour >}}

---

### 2) Empirical approach (relative frequency)

Used when probability is estimated from repeated observations.

{{< colour "red" >}}
$$
P(A)\approx\frac{\text{number of times event occurs}}{\text{total number of trials}}
$$
{{< /colour >}}

Intuition:
with many trials, empirical probability tends to stabilise.

---

### 3) Axiomatic approach (most general)

Probability is assigned to events in a way that satisfies the axioms below.
This is the approach used throughout probability theory.

---

## Axioms of probability

For any event $A$ in sample space $S$:

### 1) Non-negativity

{{< colour "red" >}}
$$
P(A)\ge 0
$$
{{< /colour >}}

### 2) Normalisation

{{< colour "red" >}}
$$
P(S)=1
$$
{{< /colour >}}

### 3) Additivity (for mutually exclusive events)

If $A\cap B=\varnothing$, then:

{{< colour "red" >}}
$$
P(A\cup B)=P(A)+P(B)
$$
{{< /colour >}}

{{% hint info %}}
These axioms are “rules of the game”.
Most probability formulas you use later are consequences of these.
{{% /hint %}}

---

## The Addition Rule (general case)

Even if $A$ and $B$ overlap, the union probability is:

{{< colour "red" >}}
$$
P(A\cup B)=P(A)+P(B)-P(A\cap B)
$$
{{< /colour >}}

Why we subtract:
the overlap $A\cap B$ gets counted twice if we only add $P(A)$ and $P(B)$.

---

## Mutually exclusive vs independent (do not confuse)

### Mutually exclusive (disjoint)

Cannot happen together.

{{< colour "red" >}}
$$
A\cap B=\varnothing
$$
{{< /colour >}}

So:

{{< colour "red" >}}
$$
P(A\cap B)=0
$$
{{< /colour >}}

Example (one die roll):
- $A$:
roll a 2
- $B$:
roll a 5  
They cannot occur together.

#### Collectively exhaustive

Two events A and B are Mutually Exclusive, but other than A and B there is nothing left in the Sample Space.

Collectively exhaustive means:
together, the events cover the entire sample space.

If $A$ and $B$ are collectively exhaustive:

{{< colour "red" >}}
$$
A\cup B = S
$$
{{< /colour >}}

If they are both mutually exclusive and collectively exhaustive, then:

{{< colour "red" >}}
$$
P(A)+P(B)=1
$$
{{< /colour >}}
---

### Independent

One event happening does not change the probability of the other.

{{< colour "red" >}}
$$
P(A\cap B)=P(A)\,P(B)
$$
{{< /colour >}}

Example:
- First coin toss is heads
- Second coin toss is heads  
These are independent.

{{% hint warning %}}
Mutually exclusive events are usually not independent unless one event has probability 0.
Intuition:
if two events cannot happen together, then learning one occurred forces the other to be false.
{{% /hint %}}

---

{{< mermaid >}}
flowchart LR
A["Two events: A and B"] --> B{"Can A and B happen together?"}
B -->|No| C["Mutually exclusive<br/>Intersection is empty"]
B -->|Yes| D{"Does knowing A change B?"}
D -->|No| E["Independent<br/>Use product rule"]
D -->|Yes| F["Dependent<br/>Use conditional probability next"]

style A fill:#90CAF9,stroke:#1E88E5,color:#000

style B fill:#CE93D8,stroke:#8E24AA,color:#000
style D fill:#CE93D8,stroke:#8E24AA,color:#000

style C fill:#C8E6C9,stroke:#2E7D32,color:#000
style E fill:#C8E6C9,stroke:#2E7D32,color:#000
style F fill:#C8E6C9,stroke:#2E7D32,color:#000
{{< /mermaid >}}


---

## Worked patterns you should recognise

### Pattern 1: Valid probability assignment

If outcomes $A,B,C,\dots$ are mutually exclusive and exhaustive, then a valid assignment must satisfy:
- Each probability is between 0 and 1
- Total must sum to 1

Checklist:
1) Are all $P(\cdot)\ge 0$?
2) Is $\sum P(\text{outcomes})=1$?
If both yes, it is permissible.

---

### Pattern 2: “At least one” trick

“At least one of X” is often easiest using complement.

Example idea:
“At least one desktop”  
= $1 - P(\text{no desktop})$  
= $1 - P(\text{both are laptops})$

{{% hint info %}}
Whenever you see:
“At least one…”
try the complement first.
{{% /hint %}}

---

## Mini-check (self-test)

1) If $P(A)=0.4$ and $P(B)=0.3$ and $A,B$ are independent, find $P(A\cap B)$.  
2) If $A,B$ are mutually exclusive, what is $P(A\cap B)$?  
3) If $P(A)=0.7$, what is $P(A^c)$?  
4) If $P(A)=0.6$, $P(B)=0.5$, and $P(A\cap B)=0.2$, find $P(A\cup B)$.

Answers:
1) $0.4\times 0.3=0.12$  
2) $0$  
3) $1-0.7=0.3$  
4) $0.6+0.5-0.2=0.9$

---

## What’s next

**Conditional Probability & Bayes’ Theorem**  
This is where “given what I already know…” becomes mathematics, and where Naïve Bayes begins.

---

{{< home-link "Home" >}} | {{< section-index >}}

---
