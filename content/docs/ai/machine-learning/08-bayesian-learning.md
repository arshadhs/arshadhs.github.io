---
title: "Bayesian Learning"
draft: false
tags: ["AI", "ML", "Bayesian Learning", "MLE", "MAP", "Naive Bayes"]
categories: ["AI", "ML"]
weight: 800
menu: main
---

# Bayesian Learning

Bayesian Learning is a probabilistic approach to machine learning.

Instead of only asking, “Which output should the model predict?”, Bayesian Learning asks:

> Given the data we have observed, how likely is each hypothesis, class, or parameter value?

This makes Bayesian Learning useful when uncertainty matters.

It is especially important in classification, probabilistic modelling, generative models, and situations where we want to combine prior knowledge with observed data.

{{% hint info %}}
**Key takeaway:**  
Bayesian Learning updates belief using evidence.

Prior belief plus observed data gives posterior belief.
{{% /hint %}}

- MLE Hypothesis
- MAP Hypothesis
- Bayes Rule
- Optimal Bayes Classifier
- Naive Bayes Classifier
- Probabilistic Generative Classifiers
- Bayesian Linear Regression

---

## Big Picture

Bayesian Learning is built around this idea:

1. Start with a prior belief.
2. Observe data.
3. Compute how likely the data is under each hypothesis.
4. Update the belief.
5. Choose the most probable class or hypothesis.

{{< mermaid >}}
flowchart LR
    A["Prior Knowledge"] --> B["Observed Data"]
    B --> C["Likelihood"]
    C --> D["Posterior"]
    D --> E["Prediction / Classification"]

    style A fill:#E1F5FE,stroke:#5b7db1,color:#222
    style B fill:#C8E6C9,stroke:#5f8f6a,color:#222
    style C fill:#FFF9C4,stroke:#b59b3b,color:#222
    style D fill:#EDE7F6,stroke:#8a6fb3,color:#222
    style E fill:#FDE2E4,stroke:#b85c68,color:#222
{{< /mermaid >}}

---

## Bayes Rule ☆

Bayes Rule tells us how to update probability when new evidence is observed.

In classification, we often want to find the probability of a class after seeing the input features.

{{% colour "green" %}}
{{< katex display=true >}}
P(C \mid X) = \frac{P(X \mid C)P(C)}{P(X)}
{{< /katex >}}
{{% /colour %}}

Where:

| Term | Meaning |
|---|---|
| {{< katex >}} P(C \mid X) {{< /katex >}} | Posterior probability |
| {{< katex >}} P(X \mid C) {{< /katex >}} | Likelihood |
| {{< katex >}} P(C) {{< /katex >}} | Prior probability |
| {{< katex >}} P(X) {{< /katex >}} | Evidence / normalising term |

---

## Bayes Rule in Hypothesis Form

In Bayesian Learning, we may compare different hypotheses.

A hypothesis could be a model, a class, or a parameter setting.

{{% colour "green" %}}
{{< katex display=true >}}
P(h \mid D) = \frac{P(D \mid h)P(h)}{P(D)}
{{< /katex >}}
{{% /colour %}}

Where:

| Term | Meaning |
|---|---|
| {{< katex >}} h {{< /katex >}} | Hypothesis |
| {{< katex >}} D {{< /katex >}} | Observed training data |
| {{< katex >}} P(h) {{< /katex >}} | Prior probability of hypothesis |
| {{< katex >}} P(D \mid h) {{< /katex >}} | Likelihood of data under hypothesis |
| {{< katex >}} P(h \mid D) {{< /katex >}} | Posterior probability of hypothesis after seeing data |

{{% hint info %}}
The lecturer emphasised the interpretation:

- prior: belief before seeing data
- likelihood: probability of data under a hypothesis
- posterior: belief after seeing the evidence
{{% /hint %}}

---

## Prior, Likelihood, Evidence and Posterior ☆

### Prior

The prior is what we believe before seeing the current data.

Example:

Before seeing symptoms, we may know that a disease is rare.

{{% colour "green" %}}
{{< katex display=true >}}
P(C)
{{< /katex >}}
{{% /colour %}}

---

### Likelihood

The likelihood tells us how likely the evidence is if a class or hypothesis is true.

Example:

If the patient has flu, how likely are fever and cough?

{{% colour "green" %}}
{{< katex display=true >}}
P(X \mid C)
{{< /katex >}}
{{% /colour %}}

---

### Evidence

The evidence is the overall probability of observing the input.

In classification, it acts as a normalising denominator.

{{% colour "green" %}}
{{< katex display=true >}}
P(X)
{{< /katex >}}
{{% /colour %}}

---

### Posterior

The posterior is the updated probability after seeing evidence.

{{% colour "green" %}}
{{< katex display=true >}}
P(C \mid X)
{{< /katex >}}
{{% /colour %}}

---

## MLE Hypothesis ☆

MLE stands for **Maximum Likelihood Estimation**.

MLE chooses the hypothesis or parameter value that makes the observed data most likely.

It focuses only on the likelihood term.

{{% colour "green" %}}
{{< katex display=true >}}
h_{MLE} = \arg\max_h P(D \mid h)
{{< /katex >}}
{{% /colour %}}

If the model has parameters {{< katex >}} \theta {{< /katex >}}, we write:

{{% colour "green" %}}
{{< katex display=true >}}
\theta_{MLE} = \arg\max_{\theta} P(D \mid \theta)
{{< /katex >}}
{{% /colour %}}

---

## MLE Using Log-Likelihood ☆

Products of many probabilities can become very small.

So we usually maximise the log-likelihood instead.

If the data points are independent:

{{% colour "green" %}}
{{< katex display=true >}}
P(D \mid \theta) = \prod_{i=1}^{n} P(x_i \mid \theta)
{{< /katex >}}
{{% /colour %}}

Taking log:

{{% colour "green" %}}
{{< katex display=true >}}
\log P(D \mid \theta) = \sum_{i=1}^{n} \log P(x_i \mid \theta)
{{< /katex >}}
{{% /colour %}}

So:

{{% colour "green" %}}
{{< katex display=true >}}
\theta_{MLE} = \arg\max_{\theta} \sum_{i=1}^{n} \log P(x_i \mid \theta)
{{< /katex >}}
{{% /colour %}}

{{% hint info %}}
MLE is data-driven.

It does not include prior belief.
It only asks which parameter makes the observed data most probable.
{{% /hint %}}

---

## MAP Hypothesis ☆

MAP stands for **Maximum A Posteriori**.

MAP chooses the hypothesis with the highest posterior probability after observing the data.

{{% colour "green" %}}
{{< katex display=true >}}
h_{MAP} = \arg\max_h P(h \mid D)
{{< /katex >}}
{{% /colour %}}

Using Bayes Rule:

{{% colour "green" %}}
{{< katex display=true >}}
h_{MAP} = \arg\max_h \frac{P(D \mid h)P(h)}{P(D)}
{{< /katex >}}
{{% /colour %}}

Since {{< katex >}} P(D) {{< /katex >}} is the same for all hypotheses, we can ignore it when comparing hypotheses.

{{% colour "green" %}}
{{< katex display=true >}}
h_{MAP} = \arg\max_h P(D \mid h)P(h)
{{< /katex >}}
{{% /colour %}}

For parameters:

{{% colour "green" %}}
{{< katex display=true >}}
\theta_{MAP} = \arg\max_{\theta} P(D \mid \theta)P(\theta)
{{< /katex >}}
{{% /colour %}}

---

## MAP Using Log Form

{{% colour "green" %}}
{{< katex display=true >}}
\theta_{MAP} = \arg\max_{\theta} \left[ \log P(D \mid \theta) + \log P(\theta) \right]
{{< /katex >}}
{{% /colour %}}

This shows the difference clearly:

- MLE uses only the data likelihood.
- MAP uses likelihood plus prior.

{{% hint info %}}
MAP can be interpreted as MLE with a prior-based preference.

This is why MAP is often connected to regularisation.
{{% /hint %}}

---

## MLE vs MAP

| Aspect | MLE | MAP |
|---|---|---|
| Full form | Maximum Likelihood Estimation | Maximum A Posteriori |
| Uses data likelihood | Yes | Yes |
| Uses prior belief | No | Yes |
| Main expression | {{< katex >}} P(D \mid h) {{< /katex >}} | {{< katex >}} P(D \mid h)P(h) {{< /katex >}} |
| Useful when | We trust data strongly | We want to include prior knowledge |
| Risk | May overfit with limited data | Depends on quality of prior |

---

## MAP Classification Rule ☆

In classification, the MAP rule chooses the class with the highest posterior probability.

{{% colour "green" %}}
{{< katex display=true >}}
\hat{C} = \arg\max_C P(C \mid X)
{{< /katex >}}
{{% /colour %}}

Using Bayes Rule:

{{% colour "green" %}}
{{< katex display=true >}}
\hat{C} = \arg\max_C P(X \mid C)P(C)
{{< /katex >}}
{{% /colour %}}

The denominator {{< katex >}} P(X) {{< /katex >}} is ignored because it is common for all classes.

---

## Probabilistic Generative Classifiers ☆

A probabilistic generative classifier models how data is generated for each class.

It learns:

- the prior probability of each class
- the likelihood of features given each class

Then it applies Bayes Rule to predict the class.

{{% colour "green" %}}
{{< katex display=true >}}
P(C \mid X) \propto P(X \mid C)P(C)
{{< /katex >}}
{{% /colour %}}

{{< mermaid >}}
flowchart LR
    A["Training Data"] --> B["Estimate Class Prior P(C)"]
    A --> C["Estimate Likelihood P(X | C)"]
    B --> D["Apply Bayes Rule"]
    C --> D
    D --> E["Choose Class with Highest Posterior"]

    style A fill:#E1F5FE,stroke:#5b7db1,color:#222
    style B fill:#C8E6C9,stroke:#5f8f6a,color:#222
    style C fill:#FFF9C4,stroke:#b59b3b,color:#222
    style D fill:#EDE7F6,stroke:#8a6fb3,color:#222
    style E fill:#FDE2E4,stroke:#b85c68,color:#222
{{< /mermaid >}}

---

## Naive Bayes Classifier ☆

Naive Bayes is a probabilistic classifier based on Bayes Rule.

It is called **naive** because it assumes that features are conditionally independent given the class.

This assumption is often not perfectly true in real data, but the method can still work surprisingly well.

---

## Naive Bayes Formula

For features {{< katex >}} X = (x_1, x_2, \ldots, x_n) {{< /katex >}}:

{{% colour "green" %}}
{{< katex display=true >}}
P(C \mid x_1, x_2, \ldots, x_n) = \frac{P(C)P(x_1, x_2, \ldots, x_n \mid C)}{P(x_1, x_2, \ldots, x_n)}
{{< /katex >}}
{{% /colour %}}

Using the naive independence assumption:

{{% colour "green" %}}
{{< katex display=true >}}
P(x_1, x_2, \ldots, x_n \mid C) = \prod_{k=1}^{n} P(x_k \mid C)
{{< /katex >}}
{{% /colour %}}

So the classification rule becomes:

{{% colour "green" %}}
{{< katex display=true >}}
\hat{C} = \arg\max_C P(C) \prod_{k=1}^{n} P(x_k \mid C)
{{< /katex >}}
{{% /colour %}}

---

## Why Naive Bayes Is Useful

Naive Bayes is commonly used for:

- spam detection
- sentiment classification
- document classification
- medical diagnosis support
- text categorisation

It is fast because it only needs to estimate simple probabilities.

It is especially useful when the number of features is large, such as in text data.

---

## Types of Naive Bayes

| Type | Feature Type | Example Use |
|---|---|---|
| Gaussian Naive Bayes | Continuous numeric features | Medical measurements, sensor data |
| Multinomial Naive Bayes | Count features | Word counts in documents |
| Bernoulli Naive Bayes | Binary features | Word present / absent |

---

## Laplace Smoothing ☆

If a feature value never appears with a class in the training data, its probability becomes zero.

That would make the whole product zero.

Laplace smoothing avoids this problem.

{{% colour "green" %}}
{{< katex display=true >}}
P(x_k \mid C) = \frac{\text{count}(x_k, C) + 1}{\text{count}(C) + V}
{{< /katex >}}
{{% /colour %}}

Where {{< katex >}} V {{< /katex >}} is the number of possible feature values.

{{% hint warning %}}
Without smoothing, one unseen feature value can force a class probability to become zero.
{{% /hint %}}

---

## Optimal Bayes Classifier

The Bayes optimal classifier chooses the class with the highest posterior probability.

It gives the best possible classification decision if the true probability distributions are known.

{{% colour "green" %}}
{{< katex display=true >}}
\hat{C}_{Bayes} = \arg\max_C P(C \mid X)
{{< /katex >}}
{{% /colour %}}

In practice, the true distributions are unknown, so methods such as Naive Bayes estimate them from data.

---

## Bayesian Linear Regression

In ordinary linear regression, we estimate a single best set of weights.

In Bayesian Linear Regression, the weights are treated as random variables.

We place a prior over the weights and update it using observed data.

{{% colour "green" %}}
{{< katex display=true >}}
y = w^T x + \epsilon
{{< /katex >}}
{{% /colour %}}

A common assumption is Gaussian noise:

{{% colour "green" %}}
{{< katex display=true >}}
\epsilon \sim \mathcal{N}(0, \sigma^2)
{{< /katex >}}
{{% /colour %}}

The likelihood becomes:

{{% colour "green" %}}
{{< katex display=true >}}
P(y \mid X, w) = \mathcal{N}(y \mid Xw, \sigma^2 I)
{{< /katex >}}
{{% /colour %}}

If we use a Gaussian prior over weights:

{{% colour "green" %}}
{{< katex display=true >}}
P(w) = \mathcal{N}(w \mid 0, \alpha^{-1}I)
{{< /katex >}}
{{% /colour %}}

Then Bayes Rule gives the posterior over weights:

{{% colour "green" %}}
{{< katex display=true >}}
P(w \mid X,y) \propto P(y \mid X,w)P(w)
{{< /katex >}}
{{% /colour %}}

---

## Bayesian Linear Regression Intuition

Ordinary regression says:

> These are the best weights.

Bayesian regression says:

> These weights are more likely, but there is uncertainty.

This is useful because it can give uncertainty around predictions.

---

## Worked Mini Example: MAP Classification

Suppose we want to classify an email as spam or not spam.

Let the evidence be:

- contains the word “offer”
- contains the word “free”

We compare:

{{% colour "green" %}}
{{< katex display=true >}}
P(Spam \mid X) \propto P(X \mid Spam)P(Spam)
{{< /katex >}}
{{% /colour %}}

{{% colour "green" %}}
{{< katex display=true >}}
P(NotSpam \mid X) \propto P(X \mid NotSpam)P(NotSpam)
{{< /katex >}}
{{% /colour %}}

The predicted class is whichever value is larger.

---

## Notes ☆

You should be comfortable with:

1. Identifying prior, likelihood, evidence and posterior from Bayes Rule.
2. Explaining the difference between MLE and MAP.
3. Writing the MAP classification rule.
4. Applying Naive Bayes using class priors and conditional probabilities.
5. Explaining why Naive Bayes assumes conditional independence.
6. Explaining why Laplace smoothing is needed.
7. Comparing discriminative and generative classifiers.

---

## Common Mistakes

| Mistake | Correction |
|---|---|
| Confusing likelihood and posterior | Likelihood is {{< katex >}} P(X \mid C) {{< /katex >}}, posterior is {{< katex >}} P(C \mid X) {{< /katex >}} |
| Forgetting the prior in MAP | MAP uses both likelihood and prior |
| Treating MLE and MAP as the same | MLE ignores the prior; MAP includes it |
| Forgetting Naive Bayes independence assumption | Naive Bayes multiplies individual feature likelihoods |
| Not using smoothing | Zero probabilities can destroy the whole product |

---

## Revision

| Concept | Core Idea | Formula |
|---|---|---|
| Bayes Rule | Update belief using evidence | {{< katex >}} P(C \mid X) = \frac{P(X \mid C)P(C)}{P(X)} {{< /katex >}} |
| MLE | Choose hypothesis that makes data most likely | {{< katex >}} \arg\max_h P(D \mid h) {{< /katex >}} |
| MAP | Choose hypothesis with highest posterior | {{< katex >}} \arg\max_h P(D \mid h)P(h) {{< /katex >}} |
| Naive Bayes | Apply Bayes Rule with independent features | {{< katex >}} \arg\max_C P(C)\prod_k P(x_k \mid C) {{< /katex >}} |
| Bayes Optimal | Choose class with highest true posterior | {{< katex >}} \arg\max_C P(C \mid X) {{< /katex >}} |

---

{{< home-link "Home" >}} | {{< section-index >}}
