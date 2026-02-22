---
title: "Naïve Bayes"
draft: false
tags: ["AI", "Statistics", "Probability"]
categories: ["AI", "Statistics"]
weight: 230
menu: main
---

# Naïve Bayes

### 3.1 From Bayes to learning (posterior over classes)

In classification, we want:
class probability given features.

For a class {{< katex >}}Y{{< /katex >}} and observed features {{< katex >}}X{{< /katex >}}:

{{% colour %}}
{{< katex display=true >}}
P(Y\mid X)=\frac{P(X\mid Y)\,P(Y)}{P(X)}
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
\hat{y}=\arg\max_{y} \; P(y)\prod_{i=1}^{n} P(x_i\mid y)
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
	ext{Score(Yes)}=P(Yes)\,P(Sunny\mid Yes)=\frac{9}{14}\cdot\frac{3}{9}
{{< /katex >}}
{{% /colour %}}

{{% colour %}}
{{< katex display=true >}}
	ext{Score(No)}=P(No)\,P(Sunny\mid No)=\frac{5}{14}\cdot\frac{2}{5}
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

## Types of Naïve Bayes Models

Naïve Bayes has a few common variants, mainly depending on the **type of features** you have.

### 1) Gaussian Naïve Bayes (for continuous features)

Gaussian Naïve Bayes is used when your features are **continuous numbers** (e.g., height, temperature, sensor readings).

It assumes that for each class, the feature values follow a **Normal (Gaussian) distribution** — the familiar bell-shaped curve that is symmetric around the mean.

Typical examples:
- medical measurements
- sensor data
- continuous-valued features in small/medium datasets

---

### 2) Multinomial Naïve Bayes (for counts / frequencies)

Multinomial Naïve Bayes is used when features represent **counts**, most commonly **word frequencies** in a document.

It works well when “how many times a word appears” matters.

Typical examples:
- document classification
- sentiment analysis using word counts
- topic classification using term frequency features

---

### 3) Bernoulli Naïve Bayes (for binary features)

Bernoulli Naïve Bayes is used when features are **binary** (0/1), meaning they represent **presence or absence**.

For text, it treats each word feature as:
- 1 if the word appears in the document
- 0 if the word does not appear

This is useful when “word present vs not present” matters more than how many times it appears.

Typical examples:
- document classification with binary bag-of-words
- spam detection based on keyword presence

---

{{< mermaid >}}
flowchart TD
  A["Naïve Bayes: choose model by feature type"] --> B["Continuous values"]
  A --> C["Counts / frequencies"]
  A --> D["Binary (0/1)"]

  B --> B1["Gaussian NB<br/>Normal distribution per class"]
  C --> C1["Multinomial NB<br/>term counts / word frequencies"]
  D --> D1["Bernoulli NB<br/>presence vs absence"]
{{< /mermaid >}}

---

## Advantages

- Easy to implement and computationally efficient.
- Works well when the number of features is large (common in text data).
- Can perform well even with limited training data.
- Handles categorical features effectively.
- For numerical features, Gaussian NB provides a simple and often useful assumption (Normal distribution).

---

## Disadvantages

- Assumes features are independent given the class, which is often not strictly true in real data.
- Can be affected by irrelevant or noisy features.
- Can assign zero probability to unseen events (unless smoothing is used), which may reduce generalisation.

---

## Applications

- Spam email filtering:
classifies emails as spam or not spam.
- Text classification:
sentiment analysis, document categorisation, topic classification.
- Medical diagnosis:
estimates likelihood of disease given symptoms or test results.
- Credit scoring:
supports decisions about loan approval based on applicant features.
- Weather prediction:
classifies weather conditions from observed factors.

---


## Mini-check (self-test)

1) If {{< katex >}}P(A)=0.3{{< /katex >}} and {{< katex >}}P(B\mid A)=0.8{{< /katex >}}, find {{< katex >}}P(A\cap B){{< /katex >}}.  
2) If {{< katex >}}P(B\mid A)=P(B){{< /katex >}}, what does that tell you about A and B?  
3) In Bayes’ theorem, what usually has the biggest impact when an event is rare?

Answers:
1) Multiply base probability by the conditional probability.
2) The events are independent.
3) The prior probability (base rate).

---

## Practice prompts

1) Write {{< katex >}}P(A\cap B\cap C\cap D){{< /katex >}} using the multiplication rule.  
2) If {{< katex >}}P(A)=0.3{{< /katex >}} and {{< katex >}}P(B\mid A)=0.8{{< /katex >}}, find {{< katex >}}P(A\cap B){{< /katex >}}.  
3) Create a 3-branch total probability tree from your work (e.g., device type, customer segment, failure mode), and compute an overall probability.

Quick answer for 2):
{{< katex >}}P(A\cap B)=0.3\times 0.8=0.24{{< /katex >}}

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
