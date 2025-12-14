---
title: "Foundation Models"
date: 2025-12-14
draft: false
tags: ["AI", "ML", "Foundation Models", "LLM"]
categories: ["AI", "ML"]
weight: 450
menu: main
---
# Foundation Model

AI models trained on massive datasets to perform a wide range of tasks with minimal fine-tuning.

- are large deep learning neural networks

- are large AI models trained on **massive and diverse datasets** (text, images, audio, or multiple modalities).  
- Contain **millions or billions of parameters**.  

- designed to perform a **broad range of general tasks** 
- designed for **general-purpose intelligence**, not a single task.  

- acts as **base models** for building specialised AI applications
- uses **transfer learning**, allowing knowledge learned from one task to be reused for others.  
- can be adapted using **fine-tuning** or **prompting**.  

---

## Traditional ML vs Foundation Models

| Feature | Traditional ML Models | Foundation Models |
|------|----------------------|------------------|
| Training data | Small, task-specific datasets | Massive, diverse datasets |
| Model size | Small to medium | Very large (millions/billions of parameters) |
| Purpose | Single, specific task | General-purpose |
| Reusability | Limited | High |
| Training approach | Train from scratch per task | Pre-train once, adapt many times |
| Transfer learning | Rare or minimal | Core design principle |
| Examples | Linear Regression, SVM, Decision Trees | GPT, BERT, CLIP |

---

## Why Foundation Models Are Different
- Traditional ML models are **built for one problem at a time**.  
- Foundation models learn **general representations** of language, vision, or sound.  
- This enables them to be reused across many applications with minimal additional training.  

---

## Areas of Application
- **Natural Language Processing (NLP)**  
- **Computer Vision**  
- **Speech Recognition**  
- **Multimodal AI** (text + images + audio)  

---

## Common Foundation Model Examples

### GPT (Generative Pre-trained Transformer)
- Focus: **Text generation and understanding**
- Tasks: Chatbots, summarisation, translation, code generation
- Example: ChatGPT

### BERT (Bidirectional Encoder Representations from Transformers)
- Focus: **Language understanding**
- Tasks: Search, question answering, sentiment analysis
- Used heavily in search engines

### CLIP (Contrastive Language–Image Pretraining)
- Focus: **Text + Image understanding**
- Tasks: Image classification using text prompts
- Enables multimodal AI systems

---

## Foundation Models in the AI Stack

{{< mermaid >}}
flowchart TB
    DATA[Massive Datasets]
    FM[Foundation Model]
    TASK1[NLP Tasks]
    TASK2[Vision Tasks]
    TASK3[Speech Tasks]

    DATA --> FM
    FM --> TASK1
    FM --> TASK2
    FM --> TASK3
{{< /mermaid >}}

---

## How Foundation Models Are Used
- Pre-trained once on large datasets  
- Adapted using:
  - **Fine-tuning**
  - **Prompt engineering**
  - **Task-specific heads**
- Serve as the backbone for **modern AI systems**, including **Large Language Models (LLMs)**  

---

## FM Summary
- Foundation models are **general-purpose AI models**.
- They power modern systems like **LLMs and multimodal AI**.
- They reduce cost, time, and complexity in AI development.
- They represent a major shift from task-specific ML to **scalable intelligence**.

---
{{< home-link "Home" >}} | {{< section-index >}}  
