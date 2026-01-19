---
title: "Generative AI"
draft: false
tags: ["AI", "Generative AI", "GenAI", "LLM"]
categories: ["AI"]
weight: 400
menu: main
bookCollapseSection: true
---

# Generative AI

**Generative Artificial Intelligence (GenAI)** refers to a class of AI systems that can **generate new content** such as text, images, audio, video, or code, rather than only making predictions or classifications.

GenAI systems learn **patterns and representations from large datasets** and use them to produce **novel outputs** that resemble the data they were trained on.

---

## How Generative AI Differs from Traditional AI

| Traditional AI | Generative AI |
|----------------|---------------|
| Predicts or classifies | Generates new content |
| Task-specific models | General-purpose models |
| Fixed outputs | Open-ended outputs |
| Often rule-based | Data-driven and probabilistic |

---

## Core Idea of Generative AI

> **Instead of learning “what label to assign”, Generative AI learns “how data is structured” and then creates new data following that structure.**

---

## High-Level View

{{< mermaid >}}
flowchart LR
    DATA[Large Datasets]
    FM[Foundation Models]
    GEN[Generated Content]

    DATA -->|Training| FM
    FM -->|Generation| GEN
{{< /mermaid >}}

---

## Key Characteristics of Generative AI

- Trained on **large-scale, diverse datasets**
- Uses **deep learning models**, especially transformers
- Produces **probabilistic outputs**
- Can be adapted to many tasks using:
  - Prompting
  - Fine-tuning
  - Retrieval-Augmented Generation (RAG)

---

## Types of Generative AI Models

- **Text generation**  
  (e.g. chatbots, summarisation, translation)

- **Image generation**  
  (e.g. image synthesis, style transfer)

- **Audio generation**  
  (e.g. speech synthesis, music generation)

- **Code generation**  
  (e.g. programming assistants)

- **Multimodal generation**  
  (combining text, images, audio)

---

## Relationship to Other AI Concepts

- Built on **Deep Learning**
- Implemented using **Foundation Models**
- Includes **Large Language Models (LLMs)** as a major subset
- Often combined with **AI Agents** for goal-driven behaviour

---

## Where Generative AI Is Used

- Conversational assistants
- Content creation
- Search and information retrieval
- Education and tutoring
- Software development support

---

## Learning Path Within This Section

- **Foundation Models** – the base models behind GenAI  
- **LLM – Model** – language-focused generative models  
- **Transformer** – the core architecture enabling GenAI  
- **AI Agents** – systems that use GenAI for reasoning and action  

---

> **Generative AI focuses on creating new content by learning the underlying structure of data, enabled by large foundation models and deep learning.**

---

{{< home-link "Home" >}} | {{< section-index >}}
