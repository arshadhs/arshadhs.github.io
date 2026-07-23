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

{{< mermaid >}}
flowchart TD

A[Conditional<br/>probability] -->|foundation| B[Bayes<br/>theorem]
D[Independent<br/>events] -->|implies| C[Independence]
C -->|simplifies| A

E[Prior] -->|with likelihood| B
F[Likelihood] -->|updates| H[Posterior]
G[Evidence] -->|normalises| B
B -->|yields| H

I[Naïve<br/>Bayes] -->|uses| B
J[Naïve<br/>assumption] -->|assumes| C
K[Features] -->|given class| J
L[Class] -->|conditions| J
I -->|predicts| M[Classification]
M -->|selects| L

style A fill:#90CAF9,stroke:#1E88E5,color:#000
style B fill:#90CAF9,stroke:#1E88E5,color:#000
style C fill:#90CAF9,stroke:#1E88E5,color:#000

style D fill:#CE93D8,stroke:#8E24AA,color:#000
style E fill:#CE93D8,stroke:#8E24AA,color:#000
style F fill:#CE93D8,stroke:#8E24AA,color:#000
style G fill:#CE93D8,stroke:#8E24AA,color:#000
style J fill:#CE93D8,stroke:#8E24AA,color:#000
style K fill:#CE93D8,stroke:#8E24AA,color:#000
style L fill:#CE93D8,stroke:#8E24AA,color:#000

style H fill:#C8E6C9,stroke:#2E7D32,color:#000
style I fill:#C8E6C9,stroke:#2E7D32,color:#000
style M fill:#C8E6C9,stroke:#2E7D32,color:#000

{{< /mermaid >}}

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

## What’s next

Probability Distributions  
Move from events to random variables and distributions.

---

{{< home-link "Home" >}} | {{< section-index >}}

---
