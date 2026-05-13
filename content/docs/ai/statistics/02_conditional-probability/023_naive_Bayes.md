---
title: "Naïve Bayes"
date: 2026-03-12
draft: false
tags: ["AI", "Statistics", "Probability", "Machine Learning"]
categories: ["AI", "Statistics"]
weight: 230
menu: main
---

# Naïve Bayes

Naïve Bayes is a **probabilistic classifier**.

- Supervised Learning Problem
- Binary Classification - final target variable is considered in two classes
- Hypothesis is target which you want to classify
- Total Probability (Prior) of Yes and No is already calculated
- Post / Posterior is when you start studying data
- Based on max probability of hypotheses classify given instance into a class

It predicts a class label by computing:
- a **prior** for each class
- **conditional probabilities** of features given the class
- a **score** for each class (multiply probabilities)
- the class with the **maximum score** is chosen

{{% hint info %}}
Key takeaway:
Naïve Bayes = Bayes + conditional independence.

It works by comparing:

{{< katex >}}P(\text{class}) \times \prod P(\text{feature}\mid \text{class}){{< /katex >}}
{{% /hint %}}

---

## 1) The classification setup

We have:
- feature vector:
{{< katex >}}X=(X_1,X_2,\dots,X_n){{< /katex >}}
- class label:
{{< katex >}}Y \in \{c_1,c_2,\dots,c_K\}{{< /katex >}}

Goal:
given a new instance {{< katex >}}x{{< /katex >}},
predict {{< katex >}}\hat{y}{{< /katex >}}.

Examples:
- Play = Yes/No given weather
- Spam / Not spam given words
- Pass / Fail given attributes

---

## 2) Bayes theorem for a class

For a class {{< katex >}}Y=c_k{{< /katex >}} and observed features {{< katex >}}X=x{{< /katex >}}:

{{< colour "red" >}}
{{< katex display=true >}}
P(Y=c_k\mid X=x)=\frac{P(X=x\mid Y=c_k)\,P(Y=c_k)}{P(X=x)}
{{< /katex >}}
{{< /colour >}}

Meaning:
- {{< katex >}}P(Y=c_k){{< /katex >}}:
prior probability of class {{< katex >}}c_k{{< /katex >}}
- {{< katex >}}P(X=x\mid Y=c_k){{< /katex >}}:
likelihood of observing {{< katex >}}x{{< /katex >}} if the class were {{< katex >}}c_k{{< /katex >}}
- {{< katex >}}P(Y=c_k\mid X=x){{< /katex >}}:
posterior (updated probability after seeing the features)

---

## 3) MAP decision rule (choose the best class)

Since {{< katex >}}P(X=x){{< /katex >}} is the same for all classes,
we can drop it when comparing classes.

{{< colour "red" >}}
{{< katex display=true >}}
\hat{y}=\arg\max_{c_k}\; P(Y=c_k\mid X=x)
=\arg\max_{c_k}\; P(X=x\mid Y=c_k)\,P(Y=c_k)
{{< /katex >}}
{{< /colour >}}

This is a **comparison rule**:
we only need relative scores.

---

## 4) The “naïve” assumption (conditional independence)

Naïve Bayes assumes:
features are **conditionally independent** given the class.

{{< colour "red" >}}
{{< katex display=true >}}
P(X_1,\dots,X_n\mid Y)=\prod_{i=1}^{n} P(X_i\mid Y)
{{< /katex >}}
{{< /colour >}}

Plain meaning:
once the class is fixed,
learning one feature does not change the probability of another feature.

---

## 5) The Naïve Bayes scoring formula

Using conditional independence:

{{< colour "red" >}}
{{< katex display=true >}}
P(Y=c_k\mid X=x)\propto P(Y=c_k)\prod_{i=1}^{n} P(X_i=x_i\mid Y=c_k)
{{< /katex >}}
{{< /colour >}}

So, for each class:
1) compute the prior {{< katex >}}P(Y=c_k){{< /katex >}}
2) compute each conditional probability {{< katex >}}P(X_i=x_i\mid Y=c_k){{< /katex >}}
3) multiply to get a score
4) choose the maximum score

---

## 6) Pipeline

{{< mermaid >}}
%%{init: {'theme':'base','themeVariables': {
  'fontFamily':'Inter, ui-sans-serif, system-ui',
  'primaryColor':'#EAF7F1',
  'primaryTextColor':'#1F2937',
  'primaryBorderColor':'#A7E3C8',
  'lineColor':'#94A3B8',
  'tertiaryColor':'#F8FAFC'
}}}%%
flowchart TD
  A["Training data<br/>features + labels"] --> B["Count frequencies per class"]
  B --> C["Convert counts to probabilities<br/>priors + conditional probs"]
  C --> D["Score a new instance x<br/>P(class) × product P(feature|class)"]
  D --> E{"Pick the max score"}
  E --> F["Predicted class"]
{{< /mermaid >}}

---

## 7) Worked example A: Play Tennis (multiple features)

Feature vector:

{{< katex >}}X=(\text{Outlook},\text{Temp},\text{Humidity},\text{Windy}){{< /katex >}}

Example instance:
- Outlook = Sunny
- Temp = Hot
- Humidity = Normal
- Windy = False

Compute two class scores (Yes/No):

{{< colour "red" >}}
{{< katex display=true >}}
\text{Score(Yes)}=
P(\text{Yes})\,
P(\text{Sunny}\mid \text{Yes})\,
P(\text{Hot}\mid \text{Yes})\,
P(\text{Normal}\mid \text{Yes})\,
P(\text{False}\mid \text{Yes})
{{< /katex >}}
{{< /colour >}}

{{< colour "red" >}}
{{< katex display=true >}}
\text{Score(No)}=
P(\text{No})\,
P(\text{Sunny}\mid \text{No})\,
P(\text{Hot}\mid \text{No})\,
P(\text{Normal}\mid \text{No})\,
P(\text{False}\mid \text{No})
{{< /katex >}}
{{< /colour >}}

Decision:
- if Score(Yes) > Score(No) → predict Yes
- otherwise → predict No

{{% hint info %}}
Tip:
You do not need to divide by {{< katex >}}P(X){{< /katex >}} to choose the class,
because it is common to both classes.
{{% /hint %}}

---

## 8) Worked example B: Binary attributes (Pass/Fail style)

Suppose the class is:
{{< katex >}}Y\in\{\text{Pass},\text{Fail}\}{{< /katex >}}

Attributes:
- Confident = Yes/No
- Sick = Yes/No

For a new instance:
Confident = Yes, Sick = No

Compute the score for each class:

{{< colour "red" >}}
{{< katex display=true >}}
\text{Score(Pass)}=
P(\text{Pass})\,
P(\text{Confident=Yes}\mid \text{Pass})\,
P(\text{Sick=No}\mid \text{Pass})
{{< /katex >}}
{{< /colour >}}

{{< colour "red" >}}
{{< katex display=true >}}
\text{Score(Fail)}=
P(\text{Fail})\,
P(\text{Confident=Yes}\mid \text{Fail})\,
P(\text{Sick=No}\mid \text{Fail})
{{< /katex >}}
{{< /colour >}}

Pick the larger score.

---

## 9) Practical issue: zero-frequency problem

If any conditional probability becomes 0,
the entire product becomes 0.

{{% hint warning %}}
Zero-frequency problem:
A single zero can kill the entire score.
This is why smoothing is used.
{{% /hint %}}

---

## 10) Laplace smoothing

Laplace smoothing adds 1 (or a constant {{< katex >}}k{{< /katex >}}) to every count.

{{< colour "red" >}}
{{< katex display=true >}}
P(w\mid c)=\frac{\operatorname{count}(w,c)+k}{\operatorname{count}(c)+k|V|}
{{< /katex >}}
{{< /colour >}}

Where:
- {{< katex >}}|V|{{< /katex >}} is vocabulary size
- {{< katex >}}k=1{{< /katex >}} is common

---

## 11) Worked example C: text classification with smoothing

Classes:
- Sports
- Not sports

Sentence:
“A very close game”

Words are features:
{{< katex >}}(\text{a},\text{very},\text{close},\text{game}){{< /katex >}}

Score each class:

{{< colour "red" >}}
{{< katex display=true >}}
\text{Score(Sports)}=
P(\text{Sports})\prod_{w\in\text{sentence}} P(w\mid \text{Sports})
{{< /katex >}}
{{< /colour >}}

{{< colour "red" >}}
{{< katex display=true >}}
\text{Score(Not sports)}=
P(\text{Not sports})\prod_{w\in\text{sentence}} P(w\mid \text{Not sports})
{{< /katex >}}
{{< /colour >}}

---

## 12) Variants (choose by feature type)

{{< mermaid >}}
%%{init: {'theme':'base','themeVariables': {
  'fontFamily':'Inter, ui-sans-serif, system-ui',
  'primaryColor':'#FFF3E6',
  'primaryTextColor':'#1F2937',
  'primaryBorderColor':'#FFD6A5',
  'lineColor':'#94A3B8',
  'tertiaryColor':'#F8FAFC'
}}}%%
flowchart TD
  A["Naïve Bayes: choose by feature type"] --> B["Continuous values"]
  A --> C["Counts / frequencies"]
  A --> D["Binary (0/1)"]

  B --> B1["Gaussian NB<br/>Normal distribution per class"]
  C --> C1["Multinomial NB<br/>term counts / word frequencies"]
  D --> D1["Bernoulli NB<br/>presence vs absence"]
{{< /mermaid >}}

### Gaussian Naïve Bayes

Continuous features.
Assumes a Normal distribution per class.

### Multinomial Naïve Bayes

Count features.
Used for word counts / term frequencies.

### Bernoulli Naïve Bayes

Binary features (0/1).
Used for presence vs absence.

---

## 13) Advantages, disadvantages, applications

Advantages:
- easy to implement and computationally efficient
- works well with many features (especially text)
- performs well even with limited training data

Disadvantages:
- conditional independence assumption may not be true
- can be influenced by irrelevant attributes
- without smoothing, unseen events can cause zero probabilities

Applications:
- spam email filtering
- sentiment analysis and document categorisation
- medical diagnosis support
- credit scoring

---

## Mini-check (self-test)

1) What does “naïve” mean here?  
2) Why can we skip dividing by {{< katex >}}P(X){{< /katex >}} when classifying?  
3) What problem does Laplace smoothing fix?

{{% hint success %}}
Answers:
1) Features are assumed conditionally independent given the class.  
2) {{< katex >}}P(X){{< /katex >}} is common to all classes, so it cancels when comparing.  
3) Zero-frequency problem (a probability becomes 0).
{{% /hint %}}

---

## What’s next

Probability Distributions  
Move from events to random variables and distributions.

---

## Reference

- [Naive Bayes](https://www.geeksforgeeks.org/machine-learning/naive-bayes-classifiers/)

---
{{< home-link "Home" >}} | {{< section-index >}}

---
