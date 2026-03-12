---
title: "Naïve Bayes"
date: 2026-03-11
draft: false
tags: ["AI", "Statistics", "Probability", "Machine Learning"]
categories: ["AI", "Statistics"]
weight: 220
menu: main
---

# Naïve Bayes

Naïve Bayes is a probabilistic classifier built from:
conditional probability and Bayes’ theorem.

- compute posterior probabilities for each class (hypothesis)
- then choose the class with the maximum posterior

---

## 1) What problem does Naïve Bayes solve?

We have:
- features (inputs):
{{< katex >}}X=(X_1,X_2,\dots,X_n){{< /katex >}}
- a target / class label:
{{< katex >}}Y{{< /katex >}}

Goal:
given a new instance {{< katex >}}x{{< /katex >}},
predict its class {{< katex >}}\hat{y}{{< /katex >}}.

Examples:
- Will “Play Tennis” be Yes or No given weather conditions?
- Is a sentence “Sports” or “Not sports”?
- Does a customer “Evade” tax (Yes/No)?

---

## 2) Bayes’ theorem in classification language

Treat each class as a hypothesis.

- hypothesis / class:
{{< katex >}}Y=c_k{{< /katex >}}
- evidence / features:
{{< katex >}}X=x{{< /katex >}}

Bayes’ theorem:

{{< colour "red" >}}
{{< katex display=true >}}
P(Y=c_k\mid X=x)=\frac{P(X=x\mid Y=c_k)\,P(Y=c_k)}{P(X=x)}
{{< /katex >}}
{{< /colour >}}

Meaning:
- {{< katex >}}P(Y=c_k){{< /katex >}}:
prior probability of class {{< katex >}}c_k{{< /katex >}}
- {{< katex >}}P(X=x\mid Y=c_k){{< /katex >}}:
likelihood of the features if the class were {{< katex >}}c_k{{< /katex >}}
- {{< katex >}}P(Y=c_k\mid X=x){{< /katex >}}:
posterior probability (updated belief after seeing the features)

---

## 3) Choosing the class: MAP decision rule

In practice, {{< katex >}}P(X=x){{< /katex >}} is the same for all classes,
so we drop it when comparing classes.

MAP (maximum a-posteriori) classification:

{{< colour "red" >}}
{{< katex display=true >}}
\hat{y}=\arg\max_{c_k}\; P(Y=c_k\mid X=x)
=\arg\max_{c_k}\; P(X=x\mid Y=c_k)\,P(Y=c_k)
{{< /katex >}}
{{< /colour >}}

Interpretation:
compute a score per class,
pick the larger one.

---

## 4) The “Naïve” assumption: conditional independence

Even if features are not independent in real life,
Naïve Bayes assumes:

- features are conditionally independent given the target class

In symbols:

{{< colour "red" >}}
{{< katex display=true >}}
P(X_1,\dots,X_n\mid Y)=\prod_{i=1}^{n} P(X_i\mid Y)
{{< /katex >}}
{{< /colour >}}

This is called conditional independence:
with respect to the condition (the class label),
one feature does not change the probability of another.

---

## 5) Naïve Bayes formula (what you actually compute)

Using conditional independence:

{{< colour "red" >}}
{{< katex display=true >}}
P(Y=c_k\mid X=x)
\propto
P(Y=c_k)\prod_{i=1}^{n} P(X_i=x_i\mid Y=c_k)
{{< /katex >}}
{{< /colour >}}

So the algorithm is:

1) compute priors {{< katex >}}P(Y=c_k){{< /katex >}} from the training data  
2) compute conditional probabilities {{< katex >}}P(X_i=v\mid Y=c_k){{< /katex >}} from frequency tables  
3) multiply them to get a class score  
4) choose the class with the larger score

---

## 6) Workflow (syllabus-aligned steps)

{{< mermaid >}}
flowchart TD
  A["Training data<br/>features + class labels"] --> B["Step 1: Priors<br/>P(Y) from label counts"]
  B --> C["Step 2: Likelihoods<br/>P(Xi|Y) from frequency tables"]
  C --> D["Step 3: Score new x<br/>P(Y)*product P(xi|Y)"]
  D --> E["Step 4: Pick max<br/>argmax over classes"]
{{< /mermaid >}}

---

## 7) Worked example 1: “Play Tennis” with one feature (Weather)

Question (from slides):
If the weather is Sunny, will the player play or not?

Let:
- {{< katex >}}Y{{< /katex >}} = Play Tennis (Yes/No)
- {{< katex >}}X{{< /katex >}} = Weather (Sunny/Overcast/Rainy)

From the frequency table idea:
- compute {{< katex >}}P(\text{Yes}){{< /katex >}} and {{< katex >}}P(\text{No}){{< /katex >}}
- compute {{< katex >}}P(\text{Sunny}\mid\text{Yes}){{< /katex >}} and {{< katex >}}P(\text{Sunny}\mid\text{No}){{< /katex >}}

Then compare:

{{< colour "red" >}}
{{< katex display=true >}}
P(\text{Yes}\mid \text{Sunny})
\propto
P(\text{Sunny}\mid \text{Yes})P(\text{Yes})
{{< /katex >}}
{{< /colour >}}

{{< colour "red" >}}
{{< katex display=true >}}
P(\text{No}\mid \text{Sunny})
\propto
P(\text{Sunny}\mid \text{No})P(\text{No})
{{< /katex >}}
{{< /colour >}}

Decision:
whichever posterior is larger is your prediction.

---

## 8) Worked example 2: “Play Tennis” with multiple features (Naïve Bayes)

Now the feature vector is:

{{< katex >}}X=(\text{Outlook},\text{Temp},\text{Humidity},\text{Windy}){{< /katex >}}

For “today” (from slides):
- Outlook = Sunny
- Temp = Hot
- Humidity = Normal
- Windy = False

We compare:

{{< colour "red" >}}
{{< katex display=true >}}
P(\text{Yes}\mid X)
\propto
P(\text{Yes})\,
P(\text{Sunny}\mid\text{Yes})\,
P(\text{Hot}\mid\text{Yes})\,
P(\text{Normal}\mid\text{Yes})\,
P(\text{False}\mid\text{Yes})
{{< /katex >}}
{{< /colour >}}

{{< colour "red" >}}
{{< katex display=true >}}
P(\text{No}\mid X)
\propto
P(\text{No})\,
P(\text{Sunny}\mid\text{No})\,
P(\text{Hot}\mid\text{No})\,
P(\text{Normal}\mid\text{No})\,
P(\text{False}\mid\text{No})
{{< /katex >}}
{{< /colour >}}

Lecture note:
even if Temp and Humidity are related in real life,
we assume they are independent with respect to the target.

---

## 9) A big practical issue: zero probability (zero-frequency problem)

If one required conditional probability becomes zero,
the entire product becomes zero,
and the classifier can fail to choose a class.

This happens when:
a feature value never appears with a certain class in training data.

---

## 10) Laplace smoothing (fix for zero probabilities)

Laplace smoothing adds 1 (or a constant {{< katex >}}k{{< /katex >}}) to each count.

For word probabilities in text classification:

{{< colour "red" >}}
{{< katex display=true >}}
P(w\mid c)=\frac{\operatorname{count}(w,c)+k}{\operatorname{count}(c)+k|V|}
{{< /katex >}}
{{< /colour >}}

Where:
- {{< katex >}}|V|{{< /katex >}} is vocabulary size
- {{< katex >}}k=1{{< /katex >}} is common

---

## 11) Worked example 3: text classification (“A very close game”)

Each word is a feature:
{{< katex >}}(a,\;very,\;close,\;game){{< /katex >}}

Compare class scores:

{{< colour "red" >}}
{{< katex display=true >}}
\text{Score(Sports)}
= P(\text{Sports})\prod_{w\in\text{sentence}}P(w\mid \text{Sports})
{{< /katex >}}
{{< /colour >}}

{{< colour "red" >}}
{{< katex display=true >}}
\text{Score(Not sports)}
= P(\text{Not sports})\prod_{w\in\text{sentence}}P(w\mid \text{Not sports})
{{< /katex >}}
{{< /colour >}}

Choose the larger score.

---

## 12) Types of Naïve Bayes models

- Gaussian Naïve Bayes:
continuous features, normal distribution assumption  
- Multinomial Naïve Bayes:
counts (e.g., word counts)  
- Bernoulli Naïve Bayes:
binary features (presence/absence)

---

## 13) Advantages and limitations

Advantages:
- simple and fast
- works well with many features (especially text)
- works with small training sets

Limitations:
- independence assumption may not hold
- needs smoothing to avoid zero-frequency problems

---

## 14) Mini-check (self-test)

1) What does “naïve” mean in Naïve Bayes?  
2) Why do we use Laplace smoothing?

{{% hint success %}}
1) We assume features are conditionally independent given the class.
2) To avoid zero probabilities that kill the product.
{{% /hint %}}

---

{{< home-link "Home" >}} | {{< section-index >}}

---
