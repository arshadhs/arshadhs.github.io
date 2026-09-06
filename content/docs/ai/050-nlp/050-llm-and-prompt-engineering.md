---
title: "LLM and Prompt Engineering"
draft: false
tags: ["Natural Language Processing", "NLP", "Large Language Models", "LLM", "Prompt Engineering", "Transfer Learning"]
categories: ["AI", "ML"]
weight: 500
menu: main
---

# LLMs and Prompt Engineering

A Large Language Model extends neural language modelling through much larger datasets, many more parameters, broad pretraining and adaptation to many downstream tasks. Its central operation remains next-token prediction.

## Learning Objectives

- Explain how neural language modelling develops into an LLM.
- Describe the meaning of large, general-purpose and pretrained.
- Explain how a prompt guides generation.
- Distinguish zero-shot and few-shot prompting.
- Compare prompting with model adaptation.

## Big Picture

{{< mermaid >}}
flowchart TD
    A["Broad Text Data"] --> B["Large-scale Pretraining"]
    B --> C["General Language Model"]
    C --> D["Prompt or Adaptation"]
    D --> E["Task Output"]

    style A fill:#E1F5FE
    style B fill:#C8E6C9
    style C fill:#FFF9C4
    style D fill:#EDE7F6
    style E fill:#E1F5FE
{{< /mermaid >}}

## 1. From Neural Language Models to LLMs ☆

A neural language model learns a conditional probability for the next token:

{{% colour "red" %}}
{{< katex display=true >}}
P(w_t\mid w_1,w_2,\ldots,w_{t-1})
{{< /katex >}}
{{% /colour %}}

The idea becomes a Large Language Model when it is scaled using:

- much larger training datasets
- many more learned parameters
- general-purpose language modelling
- pretraining followed by adaptation or prompting

{{% hint info %}}
An LLM generates one token at a time. Each generated token becomes part of the context used to predict the next token.
{{% /hint %}}

## 2. Main Characteristics of an LLM

### Large

The model is trained on extensive data and contains a large number of parameters. These parameters encode patterns learned from the training data.

### General-purpose

Training uses broad language data rather than data for only one narrow task. The resulting model can support activities such as:

- text generation
- summarisation
- question answering
- classification
- information extraction
- coding assistance
- interaction with external tools and APIs

### Pretrained

The model first learns general language patterns through pretraining. It can then be guided through prompts or adapted for a particular task or domain.

## 3. The Prompt ☆

A prompt is the information supplied to the model before it generates a response. The visible user instruction may be only one part of the complete context; a system can also add conversation history or other relevant information.

A useful prompt may contain:

| Component | Purpose |
|---|---|
| Task | States what the model should do |
| Context | Supplies relevant background or input data |
| Constraints | Controls length, tone, format or boundaries |
| Examples | Demonstrates the expected input-output pattern |

Example:

```text
Task: Classify the review as positive or negative.
Review: I love this movie.
Output format: Sentiment: <label>
```

The model uses the prompt and its learned parameters to estimate the most probable continuation.

## 4. Prompt Engineering

Prompt engineering is the design of prompts intended to elicit useful model behaviour.

It can help specify:

- the goal
- relevant information
- the desired response format
- constraints or guardrails
- examples of correct behaviour

Changing the prompt changes the immediate context, not the model's learned weights.

{{% hint success %}}
Prompting steers an existing model at use time; it does not retrain the model.
{{% /hint %}}

## 5. Zero-shot Prompting ☆

Zero-shot prompting provides an instruction but no worked examples.

```text
Classify this review as positive or negative.

Review: I love this movie.
Sentiment:
```

The model must infer the required task and output from the instruction and its pretrained knowledge.

## 6. Few-shot Prompting ☆

Few-shot prompting includes a small number of demonstrations.

```text
Review: The story was excellent.
Sentiment: Positive

Review: The film was tedious.
Sentiment: Negative

Review: I love this movie.
Sentiment:
```

The examples help establish the intended mapping and output style. The model adapts its response using the supplied context, while its core parameters remain unchanged.

| Method | Instruction | Examples | Weight update |
|---|---|---:|---:|
| Zero-shot | Yes | None | No |
| Few-shot | Yes | A small number | No |

## 7. Pretraining and Transfer Learning

Training a separate deep model from scratch for every NLP task is difficult because it requires large amounts of data and computation. Labelled data for a target domain may also be limited.

Transfer learning begins with a pretrained model and adapts its learned knowledge to a new task.

{{< mermaid >}}
flowchart TD
    A["Pretrained Model"] --> B["Feature Extraction"]
    A --> C["Partial Fine-tuning"]
    A --> D["Full Fine-tuning"]

    style A fill:#E1F5FE
    style B fill:#C8E6C9
    style C fill:#FFF9C4
    style D fill:#EDE7F6
{{< /mermaid >}}

### Feature Extraction

Most pretrained parameters remain fixed. The model supplies useful representations while only a small task-specific component is trained.

### Partial Fine-tuning

Some pretrained layers or parameters are updated, while others remain fixed.

### Full Fine-tuning

Most or all model parameters may be updated for the target task. This allows stronger adaptation but requires more data and computation.

## 8. Choosing an Adaptation Strategy

The appropriate strategy depends on:

- how different the new task is from pretraining
- how much labelled data is available
- the required specialisation
- available computation
- how much general knowledge should be retained

Updating too many parameters on limited or narrow data can cause **catastrophic forgetting**, where the model loses useful knowledge acquired during pretraining.

| Approach | Changes weights? | Typical effort | Main purpose |
|---|---:|---:|---|
| Prompting | No | Low | Guide a model at use time |
| Feature extraction | Small task head only | Moderate | Reuse learned representations |
| Fine-tuning | Some or many | Higher | Specialise model behaviour |

## 9. Limitations and Care

An LLM predicts likely continuations; likelihood is not a guarantee of factual correctness. Output quality depends on the model, its learned data patterns, the supplied context and the clarity of the task.

The model may also be excessive for a simple task where a smaller classifier or language model would be faster and easier to operate.

## Common Mistakes

{{% hint warning %}}
- Prompting does not modify the model's stored parameters.
- Few-shot prompting means examples are placed in the prompt; it is not the same as training on a small dataset.
- An LLM is more than a large vocabulary: scale also concerns data, parameters and general-purpose pretraining.
- A probable continuation is not automatically a true statement.
{{% /hint %}}

## Practice Questions

1. How does a neural language model develop into an LLM?
2. What four components can make a prompt more precise?
3. Compare zero-shot and few-shot prompting.
4. Why might an organisation adapt a pretrained model instead of training from scratch?
5. What trade-off separates feature extraction from full fine-tuning?

## Key Takeaways

{{% hint success %}}
- An LLM is a large-scale, general-purpose, pretrained neural language model.
- Its core operation is predicting the next token from the available context.
- Prompts can specify a task, context, constraints and examples.
- Zero-shot prompting supplies no examples; few-shot prompting supplies a small number.
- Transfer learning reuses pretrained knowledge, while fine-tuning changes selected model parameters.
{{% /hint %}}

## Checklist

- [ ] I can explain an LLM as a scaled neural language model.
- [ ] I can describe the main components of a prompt.
- [ ] I can distinguish zero-shot from few-shot prompting.
- [ ] I can distinguish prompting from fine-tuning.
- [ ] I can explain catastrophic forgetting.

---
{{< home-link "Home" >}} | {{< section-index >}}
