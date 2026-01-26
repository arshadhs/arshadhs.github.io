---
title: "LLM - Model"
draft: false
tags: ["AI", "ML", "Deep Learning", "LLM"]
categories: ["AI", "ML"]
weight: 460
menu: main
---

# LLM – Large Language Model

Large Language Models (LLMs) are **advanced AI systems** designed to process, understand, and generate **human-like text**.

They learn language by analysing **massive amounts of text data**, discovering patterns in:
- grammar
- meaning
- context
- relationships between words and sentences

- Built on **Deep Learning**
- Implemented using **Neural Networks**
- Based on **Transformers**
- Often combined with tools like:
  - Retrieval (RAG)
  - Agents
  - External APIs
  - Memory systems

---

## What makes an LLM special?

- Built using **deep neural networks**
- Trained on **very large datasets** (books, articles, code, web text)
- Can perform many tasks **without task-specific training**
- General-purpose language understanding, not single-task models

---

## Foundation: Transformer Architecture

LLMs are based on the **[Transformer Architecture](/ai/deep-learning/transformer/)**, which allows models to understand **context and long-range dependencies** in text.

Instead of reading text one word at a time, Transformers:
- Look at **all words in a sentence at once**
- Decide which words are **important to each other**
- Use **attention mechanisms** to build meaning

---

{{< mermaid >}}
flowchart LR
    A[Input Text] --> B[Tokenizer]
    B --> C[Transformer Layers]
    C --> D[Contextual Representation]
    D --> E[Next Token Prediction]
    E --> F[Generated Text]
{{< /mermaid >}}

- Text is converted into tokens
- Tokens pass through multiple Transformer layers
- Each layer refines understanding of context
- Model predicts the most likely next token
- Repeating this generates fluent text

---

## Popular LLMs

- GPT family (OpenAI)
- BERT (Google)
- LLaMA (Meta)
- PaLM / Gemini (Google)
- Claude (Anthropic)

*(Exact implementation details differ, but the core ideas remain the same.)*

---

## What Can LLMs Do?

- Text generation and summarisation
- Question answering
- Chatbots and virtual assistants
- Code generation and explanation
- Translation
- Document analysis
- Search and retrieval augmentation (RAG)

---

## Why LLMs Are Powerful

- Understand context, not just keywords
- Can generalise across many tasks
- Reduce need for task-specific models
- Learn representations automatically
- Scale well with more data and compute

---

## Limitations & Challenges

- Very large computational and memory cost
- Can produce confident but incorrect answers (hallucinations)
- Lack true understanding or reasoning
- Sensitive to training data quality
- Hard to interpret internally
- Ethical and bias concerns

{{% hint warning %}}
LLMs **do not understand meaning like humans**.  
They predict the next most likely token based on patterns in data.
{{% /hint %}}

---

> **An LLM is a very large neural network trained to predict the next word — and becomes powerful by doing this extremely well.**

---

{{< home-link "Home" >}} | {{< section-index >}}
