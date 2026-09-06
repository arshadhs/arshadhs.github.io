---
title: "N-gram Language Modelling"
draft: false
tags: ["Natural Language Processing", "NLP", "Language Modelling", "N-grams", "Smoothing", "Perplexity"]
categories: ["AI", "ML"]
weight: 300
menu: main
---

# N-gram Language Modelling

A language model assigns probabilities to sequences of words. It can compare complete sentences or predict which word is likely to come next.

Key ideas include:

- word prediction and sequence probability
- the chain rule and Markov assumption
- unigram, bigram and trigram models
- Maximum Likelihood Estimation
- unseen sequences and smoothing
- interpolation and backoff
- intrinsic and extrinsic evaluation
- perplexity

## Learning Objectives

- Explain what a language model represents.
- Calculate simple unigram and bigram probabilities.
- Explain why unseen N-grams create zero probabilities.
- Distinguish smoothing, interpolation and backoff.
- Interpret perplexity correctly.

## Big Picture

{{< mermaid >}}
flowchart TD
    A["Training Corpus"] --> B["Count N-grams"]
    B --> C["Estimate Probabilities"]
    C --> D["Handle Unseen Events"]
    D --> E["Score Word Sequences"]
    E --> F["Evaluate Model"]

    style A fill:#E1F5FE
    style B fill:#C8E6C9
    style C fill:#FFF9C4
    style D fill:#EDE7F6
    style E fill:#E1F5FE
    style F fill:#C8E6C9
{{< /mermaid >}}

## 1. What Is a Language Model? ☆

A language model estimates how probable a sequence of words is.

For example, a model should normally assign a higher probability to:

```text
I want to eat Chinese food.
```

than to:

```text
I want Chinese to eat food.
```

It can also predict a missing or next word:

```text
Please turn off the ____.
```

Words such as `light` or `computer` should be more probable than an unrelated word such as `banana`.

Language modelling supports tasks such as:

- speech recognition
- spelling correction
- machine translation
- text generation
- predictive typing

{{% hint info %}}
A language model does not decide whether a sentence is absolutely correct. It assigns relative probabilities based on patterns found in its training data.
{{% /hint %}}

## 2. The Chain Rule ☆

The probability of a sentence can be decomposed into a sequence of conditional probabilities.

{{% colour "red" %}}
{{< katex display=true >}}
P(w_1,w_2,\ldots,w_n)=\prod_{i=1}^{n}P(w_i\mid w_1,\ldots,w_{i-1})
{{< /katex >}}
{{% /colour %}}

For three words:

{{% colour "red" %}}
{{< katex display=true >}}
P(w_1,w_2,w_3)=P(w_1)P(w_2\mid w_1)P(w_3\mid w_1,w_2)
{{< /katex >}}
{{% /colour %}}

The chain rule is exact, but estimating a probability from every possible preceding word is impractical. Most histories are rare or absent even in a large corpus.

## 3. The Markov Assumption

An N-gram model simplifies the chain rule by using only a limited amount of recent context.

A bigram model assumes that the next word depends only on the previous word:

{{% colour "red" %}}
{{< katex display=true >}}
P(w_i\mid w_1,\ldots,w_{i-1})\approx P(w_i\mid w_{i-1})
{{< /katex >}}
{{% /colour %}}

A trigram model uses the previous two words:

{{% colour "red" %}}
{{< katex display=true >}}
P(w_i\mid w_1,\ldots,w_{i-1})\approx P(w_i\mid w_{i-2},w_{i-1})
{{< /katex >}}
{{% /colour %}}

This is the **Markov assumption**: a limited recent history is treated as sufficient for prediction.

## 4. N-grams

An N-gram is a sequence of consecutive tokens.

For the sentence:

```text
I love natural language processing
```

| Model | Example units | Context used for prediction |
|---|---|---|
| Unigram | `I`, `love`, `natural` | No previous word |
| Bigram | `I love`, `love natural` | One previous word |
| Trigram | `I love natural` | Two previous words |

Larger N-grams provide more context, but they also create more possible combinations. This increases sparsity: many valid combinations may never occur in the training data.

Sentence boundaries are usually represented with tokens such as `<s>` and `</s>`. These allow the model to learn which words commonly begin or end sentences.

## 5. Maximum Likelihood Estimation ☆

Maximum Likelihood Estimation uses observed corpus counts to estimate probabilities.

For a bigram:

{{% colour "red" %}}
{{< katex display=true >}}
P(w_i\mid w_{i-1})=\frac{C(w_{i-1},w_i)}{C(w_{i-1})}
{{< /katex >}}
{{% /colour %}}

Here:

- {{< katex >}} C(w_{i-1},w_i) {{< /katex >}} is the number of times the two words occur together.
- {{< katex >}} C(w_{i-1}) {{< /katex >}} is the number of times the context word occurs.

### Worked Example

Suppose `I want` occurs 40 times and `I want Chinese` occurs 8 times.

{{% colour "red" %}}
{{< katex display=true >}}
P(\text{Chinese}\mid\text{I want})=\frac{8}{40}=0.2
{{< /katex >}}
{{% /colour %}}

For a bigram model, the sentence probability is the product of its bigram probabilities.

{{% colour "red" %}}
{{< katex display=true >}}
P(w_1,\ldots,w_n)\approx P(w_1)\prod_{i=2}^{n}P(w_i\mid w_{i-1})
{{< /katex >}}
{{% /colour %}}

Because multiplying many small probabilities can underflow numerically, implementations commonly add log probabilities instead.

## 6. Generalisation and Zero Probabilities ☆

An N-gram model can overfit its training corpus. A perfectly reasonable N-gram may be absent from the observed data.

If one bigram has probability zero, the probability of the entire sentence becomes zero:

{{% colour "red" %}}
{{< katex display=true >}}
P(w_1,\ldots,w_n)=\cdots\times 0\times\cdots=0
{{< /katex >}}
{{% /colour %}}

This does not necessarily mean that the sentence is impossible. It may simply mean that the training corpus was limited.

Unknown words create a related problem. In an open-vocabulary system, infrequent training words can be mapped to an `<UNK>` token so the model learns a probability for unseen vocabulary.

## 7. Laplace Smoothing

Laplace smoothing, or add-one smoothing, adds one to every possible N-gram count.

For a bigram model:

{{% colour "red" %}}
{{< katex display=true >}}
P_{\text{add-1}}(w_i\mid w_{i-1})=\frac{C(w_{i-1},w_i)+1}{C(w_{i-1})+|V|}
{{< /katex >}}
{{% /colour %}}

The vocabulary size {{< katex >}} |V| {{< /katex >}} is added to the denominator because one count has been added for every possible next word.

Laplace smoothing prevents zero probabilities, but it is a blunt method: it transfers too much probability mass from observed events to unseen events. It is useful for understanding smoothing, but is generally not preferred for serious N-gram language models.

## 8. Interpolation and Backoff

Both methods use shorter histories when a longer N-gram is unreliable.

### Linear Interpolation

Interpolation combines probabilities from several N-gram orders:

{{% colour "red" %}}
{{< katex display=true >}}
\hat{P}(w_i\mid w_{i-2},w_{i-1})=\lambda_3P(w_i\mid w_{i-2},w_{i-1})+\lambda_2P(w_i\mid w_{i-1})+\lambda_1P(w_i)
{{< /katex >}}
{{% /colour %}}

The weights satisfy:

{{% colour "red" %}}
{{< katex display=true >}}
\lambda_1+\lambda_2+\lambda_3=1
{{< /katex >}}
{{% /colour %}}

The weights can be chosen using held-out data.

### Backoff

Backoff uses the highest-order N-gram when it has adequate evidence. If it does not, the model falls back to a shorter context.

```text
Trigram available? → use trigram
Otherwise          → try bigram
Still unavailable? → use unigram
```

**Stupid Backoff** is designed for very large web-scale N-gram collections. It applies a fixed penalty when moving to a lower-order model and is useful for ranking, even though its scores are not normalised probabilities.

## 9. Evaluating Language Models ☆

Data should be separated into:

- **training set** — estimates model parameters
- **development set** — selects settings and compares model variants
- **test set** — provides the final evaluation on unseen data

The same test set should not be repeatedly used to tune a model, because the model-selection process would indirectly overfit it.

### Extrinsic Evaluation

Extrinsic evaluation places the language model inside a real application, such as speech recognition or machine translation, and measures the application's performance.

It is the most direct test of usefulness, but can be slow and expensive.

### Intrinsic Evaluation

Intrinsic evaluation measures the language model directly on held-out text. The principal metric is perplexity.

## 10. Perplexity ☆

Perplexity measures how surprised a model is by a word sequence.

For a sequence of {{< katex >}} N {{< /katex >}} words:

{{% colour "red" %}}
{{< katex display=true >}}
PP(W)=P(w_1,w_2,\ldots,w_N)^{-\frac{1}{N}}
{{< /katex >}}
{{% /colour %}}

For a bigram model:

{{% colour "red" %}}
{{< katex display=true >}}
PP(W)=\left(\prod_{i=1}^{N}P(w_i\mid w_{i-1})\right)^{-\frac{1}{N}}
{{< /katex >}}
{{% /colour %}}

{{% hint success %}}
**Lower perplexity means better next-word prediction**, provided the models use the same vocabulary and are evaluated on the same test data.
{{% /hint %}}

Perplexity can also be understood as an average branching factor. A perplexity of 10 suggests that the model behaves as though it is choosing among roughly 10 plausible next words at each step.

## N-gram Trade-offs

| Property | Smaller N | Larger N |
|---|---|---|
| Context | Less | More |
| Data required | Lower | Higher |
| Sparsity | Lower | Higher |
| Local fluency | Weaker | Better when observed |
| Long-distance dependencies | Poor | Still limited |

## Common Mistakes

{{% hint warning %}}
- An unseen N-gram is not necessarily an impossible phrase.
- A larger value of N does not automatically produce a better model; data sparsity also increases.
- Lower perplexity is meaningful only under a fair comparison using the same evaluation conditions.
- Add-one smoothing prevents zeros but is not usually the strongest practical smoothing method.
{{% /hint %}}

## Practice Questions

1. Why is the exact chain-rule model difficult to estimate from a corpus?
2. Calculate {{< katex >}} P(\text{food}\mid\text{Chinese}) {{< /katex >}} if `Chinese food` occurs 25 times and `Chinese` occurs 100 times.
3. Why can one unseen bigram make a whole sentence probability zero?
4. Compare interpolation with backoff.
5. What does a lower perplexity indicate?

## Key Takeaways

{{% hint success %}}
- A language model assigns probabilities to word sequences and predicts likely words.
- N-gram models approximate the full history with a short recent context.
- Corpus counts provide Maximum Likelihood Estimates of N-gram probabilities.
- Smoothing, interpolation and backoff help the model handle sparse or unseen sequences.
- Perplexity measures predictive uncertainty; lower is better under comparable conditions.
{{% /hint %}}

## Checklist

- [ ] I can explain the chain rule and Markov assumption.
- [ ] I can calculate a bigram probability from counts.
- [ ] I can explain why smoothing is needed.
- [ ] I can distinguish interpolation from backoff.
- [ ] I can interpret perplexity.

---
{{< home-link "Home" >}} | {{< section-index >}}
