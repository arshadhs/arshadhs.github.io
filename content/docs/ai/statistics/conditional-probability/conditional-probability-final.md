---
title: "Conditional Probability"
date: 2026-03-12
draft: false
tags: ["AI", "Statistics", "Probability"]
categories: ["AI", "Statistics"]
weight: 210
menu: main
---

# Conditional Probability

Conditional probability updates the probability of an event when new information is available.

It shows up whenever a question says:
- “given that…”
- “among those who…”
- “out of the items that…”
- “if it does not fail immediately…”

{{% hint info %}}
Key takeaway:
Conditional probability is always:

joint probability ÷ probability of the condition.

The condition must not be an impossible event.
{{% /hint %}}

---

## Prior vs posterior

- Prior probability:
probability with no condition (before new information)

- Posterior probability:
probability of the same event after new information (with a condition)

---

## 1) Conditioning changes the sample space

Before you condition, your “universe” is the sample space {{< katex >}}S{{< /katex >}}.

After you condition on {{< katex >}}B{{< /katex >}}, your “universe” becomes:
only the outcomes where {{< katex >}}B{{< /katex >}} is true.

So:
- {{< katex >}}P(A){{< /katex >}}:
probability of {{< katex >}}A{{< /katex >}} in the full sample space
- {{< katex >}}P(A\mid B){{< /katex >}}:
probability of {{< katex >}}A{{< /katex >}} inside {{< katex >}}B{{< /katex >}}

Conditioning “shrinks the universe”.

---

## 2) Definition of conditional probability

For events {{< katex >}}A{{< /katex >}} and {{< katex >}}B{{< /katex >}}, with {{< katex >}}P(B)>0{{< /katex >}}:

{{< colour "red" >}}
{{< katex display=true >}}
P(A\mid B)=\frac{P(A\cap B)}{P(B)}
{{< /katex >}}
{{< /colour >}}

Meaning:
- Numerator {{< katex >}}P(A\cap B){{< /katex >}}:
joint probability (“A and B together”)
- Denominator {{< katex >}}P(B){{< /katex >}}:
probability of the condition (“B happened”)

Important:
you cannot condition on an impossible event.
If {{< katex >}}P(B)=0{{< /katex >}}, then {{< katex >}}P(A\mid B){{< /katex >}} is not defined.

---

## 3) Multiplication rule (joint probability)

Start from the definition and rearrange:

{{< colour "red" >}}
{{< katex display=true >}}
P(A\cap B)=P(B)\,P(A\mid B)
{{< /katex >}}
{{< /colour >}}

Also:

{{< colour "red" >}}
{{< katex display=true >}}
P(A\cap B)=P(A)\,P(B\mid A)
{{< /katex >}}
{{< /colour >}}

This multiplication rule is valid for any two events:
independent or dependent.

---

## 4) Chain rule for three events

For {{< katex >}}A,B,C{{< /katex >}}:

{{< colour "red" >}}
{{< katex display=true >}}
P(A\cap B\cap C)=P(A)\,P(B\mid A)\,P(C\mid A\cap B)
{{< /katex >}}
{{< /colour >}}

Rule of thumb:
each new event is conditioned on everything that happened before it.

---

## 5) Independence as a special case

Two events are independent if knowing one does not change the probability of the other.

Equivalent tests:

{{< colour "red" >}}
{{< katex display=true >}}
P(A\mid B)=P(A)
{{< /katex >}}
{{< /colour >}}

{{< colour "red" >}}
{{< katex display=true >}}
P(B\mid A)=P(B)
{{< /katex >}}
{{< /colour >}}

{{< colour "red" >}}
{{< katex display=true >}}
P(A\cap B)=P(A)\,P(B)
{{< /katex >}}
{{< /colour >}}

---

## 6) Do not confuse: mutually exclusive vs independent

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
Mutually exclusive events are almost never independent.

If {{< katex >}}A\cap B=\varnothing{{< /katex >}} and both events are possible,
then {{< katex >}}P(A)P(B)>0{{< /katex >}} but {{< katex >}}P(A\cap B)=0{{< /katex >}}.
So they cannot be equal.
{{% /hint %}}

---

## 7) Visual guide: what relationship do events have?

{{< mermaid >}}
%%{init: {'theme':'base','themeVariables': {
  'fontFamily':'Inter, ui-sans-serif, system-ui',
  'primaryColor':'#E8F1FF',
  'primaryTextColor':'#1F2937',
  'primaryBorderColor':'#A7C7FF',
  'lineColor':'#94A3B8',
  'tertiaryColor':'#F8FAFC'
}}}%%
flowchart LR
  A["Two events A and B"] --> Q{"Can A and B happen together?"}
  Q -->|No| M["Mutually exclusive<br/>A ∩ B = ∅"]
  Q -->|Yes| R{"Does knowing A change B?"}
  R -->|No| I["Independent<br/>P(A ∩ B)=P(A)P(B)"]
  R -->|Yes| D["Dependent<br/>Use conditional probability"]
{{< /mermaid >}}

---

## 8) Worked patterns

### Pattern A: “fraction of those who passed first also passed second”

Let:
- {{< katex >}}A{{< /katex >}}:
passed exam 1
- {{< katex >}}B{{< /katex >}}:
passed exam 2

If you are given {{< katex >}}P(A\cap B){{< /katex >}} and {{< katex >}}P(A){{< /katex >}}:

{{< colour "red" >}}
{{< katex display=true >}}
P(B\mid A)=\frac{P(A\cap B)}{P(A)}
{{< /katex >}}
{{< /colour >}}

Example values:
if {{< katex >}}P(A\cap B)=0.35{{< /katex >}} and {{< katex >}}P(A)=0.42{{< /katex >}},

{{< colour "red" >}}
{{< katex display=true >}}
P(B\mid A)=\frac{0.35}{0.42}=\frac{35}{42}=\frac{5}{6}
{{< /katex >}}
{{< /colour >}}

---

### Pattern B: “given it does not fail immediately”

Conditioning removes some outcomes.

Example structure:
if “fails immediately” bulbs are excluded by the condition,
then the sample space becomes only:
(partially defective + acceptable)

{{< colour "red" >}}
{{< katex display=true >}}
P(\text{acceptable}\mid \text{not immediate fail})
=\frac{\text{acceptable}}{\text{acceptable}+\text{partial}}
{{< /katex >}}
{{< /colour >}}

---

### Pattern C: table-based conditional probability (counts)

If you have a two-way table (like Age group vs Default Yes/No),
then:

- joint count:
a cell in the table
- marginal count:
row total or column total

Example structure:

{{< colour "red" >}}
{{< katex display=true >}}
P(\text{No default}\mid \text{Middle-aged})
=\frac{\text{No default and Middle-aged}}{\text{Middle-aged total}}
{{< /katex >}}
{{< /colour >}}

Reverse conditioning changes the denominator:

{{< colour "red" >}}
{{< katex display=true >}}
P(\text{Middle-aged}\mid \text{No default})
=\frac{\text{No default and Middle-aged}}{\text{No default total}}
{{< /katex >}}
{{< /colour >}}

---

## 9) Common traps

### Trap 1: mixing up “A given B” and “B given A”

They are usually different.

{{< colour "red" >}}
{{< katex display=true >}}
P(A\mid B)=\frac{P(A\cap B)}{P(B)}
\qquad
P(B\mid A)=\frac{P(A\cap B)}{P(A)}
{{< /katex >}}
{{< /colour >}}

Same joint probability:
different denominators.

---

### Trap 2: adding conditional probabilities

In general:
{{< katex >}}P(C\mid B)+P(C\mid B^c){{< /katex >}}
is not equal to 1.

What is true is the weighted version:

{{< colour "red" >}}
{{< katex display=true >}}
P(C)=P(C\mid B)P(B)+P(C\mid B^c)P(B^c)
{{< /katex >}}
{{< /colour >}}

---

### Trap 3: complements of independent events

If {{< katex >}}A{{< /katex >}} and {{< katex >}}B{{< /katex >}} are independent, then complements remain independent:
{{< katex >}}A^c{{< /katex >}} and {{< katex >}}B{{< /katex >}} are also independent.

A quick check form:

{{< colour "red" >}}
{{< katex display=true >}}
P(B\mid A)=P(B)\ \Rightarrow\ P(B\mid A^c)=P(B)
{{< /katex >}}
{{< /colour >}}

---

## Mini-check (self-test)

1) What must be true for {{< katex >}}P(A\mid B){{< /katex >}} to be defined?  
2) Rewrite {{< katex >}}P(A\cap B){{< /katex >}} using conditional probability.  
3) If {{< katex >}}A{{< /katex >}} and {{< katex >}}B{{< /katex >}} are independent, what is {{< katex >}}P(A\mid B){{< /katex >}}?  
4) If {{< katex >}}A{{< /katex >}} and {{< katex >}}B{{< /katex >}} are mutually exclusive and both possible, are they independent?

{{% hint success %}}
Answers:
1) {{< katex >}}P(B)>0{{< /katex >}}  
2) {{< katex >}}P(A\cap B)=P(B)P(A\mid B){{< /katex >}}  
3) {{< katex >}}P(A\mid B)=P(A){{< /katex >}}  
4) No
{{% /hint %}}

---

## What’s next

Total probability and Bayes’ theorem

---

{{< home-link "Home" >}} | {{< section-index >}}

---
