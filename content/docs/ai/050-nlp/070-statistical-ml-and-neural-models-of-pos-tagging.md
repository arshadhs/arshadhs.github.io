---
title: "Statistical, ML and Neural Models of POS Tagging"
draft: false
tags: ["Natural Language Processing", "NLP", "Hidden Markov Models", "Forward Algorithm", "Viterbi Algorithm", "POS Tagging"]
categories: ["AI", "ML"]
weight: 700
menu: main
---

# Statistical, ML and Neural Models of POS Tagging

'HMM Inference: Forward and Viterbi Algorithms' covers the portion:

- Forward Algorithm
- Viterbi Algorithm
- HMM inference for POS tagging

The complete Topic also includes:

- Maximum Entropy Markov Models
- Bidirectionality
- Neural-network models for POS tagging

# HMM Inference: Forward and Viterbi Algorithms

Hidden Markov Models create two closely related inference problems:

- **Likelihood:** How probable is an observed sequence under the model?
- **Decoding:** Which hidden-state sequence most probably generated the observations?

The Forward Algorithm solves the likelihood problem, while the Viterbi Algorithm solves the decoding problem. Both use dynamic programming and a trellis, but they combine paths differently.

## Learning Objectives

- Distinguish likelihood from decoding.
- Explain why brute-force HMM inference is impractical.
- Calculate forward probabilities using a trellis.
- Calculate Viterbi probabilities and store backpointers.
- Apply Forward and Viterbi inference to Part-of-Speech tagging.

## Big Picture

{{< mermaid >}}
flowchart TD
    A["HMM and Observations"] --> B{"Required Result"}
    B -->|"Sequence likelihood"| C["Forward Algorithm"]
    B -->|"Best hidden path"| D["Viterbi Algorithm"]
    C --> E["Sum over paths"]
    D --> F["Maximise and backtrack"]

    style A fill:#E1F5FE
    style B fill:#FFF9C4
    style C fill:#C8E6C9
    style D fill:#EDE7F6
    style E fill:#E1F5FE
    style F fill:#C8E6C9
{{< /mermaid >}}

## 1. HMM Recap

A Hidden Markov Model is represented as:

{{% colour "red" %}}
{{< katex display=true >}}
\lambda=(A,B,\pi)
{{< /katex >}}
{{% /colour %}}

where:

| Component | Meaning |
|---|---|
| {{< katex >}} A {{< /katex >}} | State-transition probabilities |
| {{< katex >}} B {{< /katex >}} | Observation or emission probabilities |
| {{< katex >}} \pi {{< /katex >}} | Initial-state probabilities |

The state sequence is hidden, but the emitted observation sequence is visible.

For POS tagging:

| HMM concept | POS-tagging interpretation |
|---|---|
| Hidden state | POS tag |
| Observation | Word |
| Transition | Probability of one tag following another |
| Emission | Probability of a word being emitted by a tag |

## 2. Two Fundamental Problems ☆

Let the observation sequence be:

{{% colour "red" %}}
{{< katex display=true >}}
O=o_1,o_2,\ldots,o_T
{{< /katex >}}
{{% /colour %}}

### Likelihood

Given the model and observations, calculate:

{{% colour "red" %}}
{{< katex display=true >}}
P(O\mid\lambda)
{{< /katex >}}
{{% /colour %}}

This asks:

> How likely is the complete observation sequence under this HMM?

### Decoding

Given the model and observations, find the most probable hidden-state sequence:

{{% colour "red" %}}
{{< katex display=true >}}
Q^*=\underset{Q}{\operatorname{argmax}}\;P(Q\mid O,\lambda)
{{< /katex >}}
{{% /colour %}}

This asks:

> Which hidden path most probably generated the observations?

## 3. Why Brute Force Is Impractical

If an HMM has {{< katex >}} N {{< /katex >}} states and {{< katex >}} T {{< /katex >}} observations, then it has:

{{% colour "red" %}}
{{< katex display=true >}}
N^T
{{< /katex >}}
{{% /colour %}}

possible state sequences.

For {{< katex >}} N=20 {{< /katex >}} and {{< katex >}} T=100 {{< /katex >}}, this becomes {{< katex >}} 20^{100} {{< /katex >}}, an astronomically large number.

Many candidate paths share the same partial computations. Dynamic programming stores these intermediate results so they do not need to be recalculated.

## 4. The Trellis

A trellis arranges HMM computation by time and state:

- each column represents one observation position
- each row represents one possible hidden state
- each cell stores a probability for a partial sequence
- incoming edges represent possible state transitions

{{< mermaid >}}
flowchart LR
    A["Time one"] --> B["Time two"]
    B --> C["Time three"]

    style A fill:#E1F5FE
    style B fill:#C8E6C9
    style C fill:#FFF9C4
{{< /mermaid >}}

The Forward and Viterbi algorithms can use the same trellis structure. What differs is the value stored in each cell.

## 5. The Forward Algorithm ☆

The Forward Algorithm efficiently calculates the likelihood of the observation sequence.

The forward value {{< katex >}} \alpha_t(j) {{< /katex >}} is the total probability of:

- generating observations {{< katex >}} o_1,\ldots,o_t {{< /katex >}}, and
- being in state {{< katex >}} j {{< /katex >}} at time {{< katex >}} t {{< /katex >}}.

{{% colour "red" %}}
{{< katex display=true >}}
\alpha_t(j)=P(o_1,o_2,\ldots,o_t,q_t=j\mid\lambda)
{{< /katex >}}
{{% /colour %}}

### Initialisation

For every state {{< katex >}} j {{< /katex >}}:

{{% colour "red" %}}
{{< katex display=true >}}
\alpha_1(j)=\pi_j b_j(o_1)
{{< /katex >}}
{{% /colour %}}

This combines the probability of starting in state {{< katex >}} j {{< /katex >}} with the probability that it emits the first observation.

### Recursion

For each later time step:

{{% colour "red" %}}
{{< katex display=true >}}
\alpha_t(j)=b_j(o_t)\sum_{i=1}^{N}\alpha_{t-1}(i)a_{ij}
{{< /katex >}}
{{% /colour %}}

The algorithm:

1. Takes every path reaching state {{< katex >}} j {{< /katex >}}.
2. Multiplies the previous forward value by the transition probability.
3. Sums the incoming path probabilities.
4. Multiplies by the emission probability for the current observation.

### Termination

After the final observation, sum the forward values across all possible final states:

{{% colour "red" %}}
{{< katex display=true >}}
P(O\mid\lambda)=\sum_{j=1}^{N}\alpha_T(j)
{{< /katex >}}
{{% /colour %}}

### Complexity

The Forward Algorithm reduces the computation from exponential enumeration to:

{{% colour "red" %}}
{{< katex display=true >}}
O(N^2T)
{{< /katex >}}
{{% /colour %}}

## 6. Forward Algorithm Worked Example

Consider an HMM with two hidden weather states:

- `H` — hot
- `C` — cold

The observed ice-cream sequence is:

```text
3, 1, 3
```

The trellis calculations give:

| Time | Observation | {{< katex >}} \alpha_t(H) {{< /katex >}} | {{< katex >}} \alpha_t(C) {{< /katex >}} |
|---:|---:|---:|---:|
| 1 | 3 | 0.32000 | 0.02000 |
| 2 | 1 | 0.04040 | 0.06900 |
| 3 | 3 | 0.02464 | 0.00478 |

The total likelihood is the sum of the final forward values:

{{% colour "red" %}}
{{< katex display=true >}}
P(3,1,3\mid\lambda)=0.02464+0.00478=0.02942
{{< /katex >}}
{{% /colour %}}

The result includes probability mass from **all possible hidden-state paths** that could have produced the observations.

## 7. The Viterbi Algorithm ☆

The Viterbi Algorithm finds the single most probable hidden-state path.

The Viterbi value {{< katex >}} v_t(j) {{< /katex >}} is the probability of the best partial path that:

- generates observations up to time {{< katex >}} t {{< /katex >}}, and
- finishes in state {{< katex >}} j {{< /katex >}}.

### Initialisation

{{% colour "red" %}}
{{< katex display=true >}}
v_1(j)=\pi_j b_j(o_1)
{{< /katex >}}
{{% /colour %}}

### Recursion

{{% colour "red" %}}
{{< katex display=true >}}
v_t(j)=b_j(o_t)\max_{1\leq i\leq N}\left[v_{t-1}(i)a_{ij}\right]
{{< /katex >}}
{{% /colour %}}

Unlike the Forward Algorithm, Viterbi keeps only the most probable incoming path.

### Backpointers

The state producing the maximum value is recorded:

{{% colour "red" %}}
{{< katex display=true >}}
bp_t(j)=\underset{1\leq i\leq N}{\operatorname{argmax}}\left[v_{t-1}(i)a_{ij}\right]
{{< /katex >}}
{{% /colour %}}

These backpointers are essential because the final maximum gives only the best ending state. Backtracking through the pointers reconstructs the complete path.

### Termination and Backtracking

Choose the best final state:

{{% colour "red" %}}
{{< katex display=true >}}
q_T^*=\underset{j}{\operatorname{argmax}}\;v_T(j)
{{< /katex >}}
{{% /colour %}}

Then follow the stored backpointers from time {{< katex >}} T {{< /katex >}} to time 1.

## 8. Viterbi Worked Example

Using the same two-state HMM and observations `3, 1, 3`:

| Time | Observation | {{< katex >}} v_t(H) {{< /katex >}} | {{< katex >}} v_t(C) {{< /katex >}} |
|---:|---:|---:|---:|
| 1 | 3 | 0.320 | 0.020 |
| 2 | 1 | 0.038 | 0.064 |
| 3 | 3 | 0.013 | 0.003 |

The best final state is `H`. Following the backpointers gives the most probable hidden-state path:

```text
H → C → H
```

This is different from the Forward result. Forward returns the combined likelihood of all paths; Viterbi returns the best individual path and its probability.

## 9. Forward and Viterbi Compared ☆

| Feature | Forward Algorithm | Viterbi Algorithm |
|---|---|---|
| Problem | Likelihood | Decoding |
| Output | Probability of observations | Most likely hidden path |
| Incoming paths | Adds all | Keeps maximum |
| Core operation | Sum | Max |
| Backpointers | Not required | Required |
| Complexity | {{< katex >}} O(N^2T) {{< /katex >}} | {{< katex >}} O(N^2T) {{< /katex >}} |

{{% hint success %}}
**Forward asks “How likely are these observations?” Viterbi asks “Which hidden path best explains them?”**
{{% /hint %}}

## 10. Application to POS Tagging

In HMM-based POS tagging:

```text
Observed sequence: Janet would back the bill
Hidden sequence:   NNP   MD    VB   DT  NN
```

The Forward Algorithm can calculate the likelihood of a word sequence under the tag model.

The Viterbi Algorithm decodes the word sequence to find its highest-probability tag sequence using:

- initial tag probabilities
- transition probabilities between tags
- emission probabilities connecting tags to words

The trellis has:

- one column for each word
- one row for each possible POS tag
- a Viterbi score and backpointer in each cell

## 11. Scope Beyond HMMs

HMM-based POS tagging has limitations:

- strong independence assumptions
- limited context
- difficulty incorporating varied features such as capitalisation, prefixes, suffixes, neighbouring words, word shape, digits and hyphens

Models such as Maximum Entropy Markov Models and neural POS taggers are designed to incorporate richer evidence. These approaches form a later extension and are not developed here.

## Common Mistakes

{{% hint warning %}}
- Forward sums the probabilities of all paths; Viterbi does not.
- Viterbi needs backpointers to recover the complete best path.
- The most probable path is not the same quantity as the total probability of the observations.
- In POS tagging, words are observations and tags are hidden states.
- Dynamic programming avoids recomputing shared subproblems; it does not enumerate every full path.
{{% /hint %}}

## Practice Questions

1. Distinguish the HMM likelihood and decoding problems.
2. Why does brute-force inference require {{< katex >}} N^T {{< /katex >}} paths?
3. What does {{< katex >}} \alpha_t(j) {{< /katex >}} represent?
4. Why does Forward use a sum while Viterbi uses a maximum?
5. What information is stored in a Viterbi backpointer?
6. Map HMM states, observations, transitions and emissions to POS tagging.

## Key Takeaways

{{% hint success %}}
- HMM likelihood and decoding are different inference problems.
- The Forward Algorithm sums over all possible hidden paths to calculate observation likelihood.
- The Viterbi Algorithm keeps the best partial path and uses backpointers to recover the best complete path.
- Both algorithms use a trellis and reduce exponential search to dynamic programming.
- In HMM-based POS tagging, Viterbi finds the most probable tag sequence for the observed words.
{{% /hint %}}

## Checklist

- [ ] I can distinguish likelihood from decoding.
- [ ] I can explain a trellis.
- [ ] I can perform Forward initialisation, recursion and termination.
- [ ] I can perform Viterbi recursion and backtracking.
- [ ] I can explain the difference between summing and maximising paths.
- [ ] I can apply both algorithms conceptually to POS tagging.

---
{{< home-link "Home" >}} | {{< section-index >}}
