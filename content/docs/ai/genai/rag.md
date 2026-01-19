---
title: "Retrieval-Augmented Generation (RAG)"
draft: false
tags: ["AI", "Generative AI", "LLM", "RAG"]
categories: ["AI", "ML"]
weight: 320
menu: main
---

# Retrieval-Augmented Generation (RAG)

**Retrieval-Augmented Generation (RAG)** is a system design pattern that improves an LLM’s answers by:
1. **Retrieving** relevant information from an external knowledge source, and then  
2. **Augmenting** the LLM prompt with that retrieved context before generating the final response.

RAG helps an LLM **look things up first**, then **answer using evidence**.

---

## Why RAG is Useful

RAG is commonly used when:

- Your knowledge is in **private documents** (PDFs, policies, internal wiki)
- You need **up-to-date information** (things not in the model’s training data)
- You want fewer **hallucinations** by grounding answers in retrieved sources
- You want **traceability** (show “where the answer came from”)

{{% hint info %}}
RAG does not change the model weights.  
It changes what the model *sees* at inference time by adding retrieved context.
{{% /hint %}}

---

## High-Level Flow

{{< mermaid >}}
flowchart LR
    U[User Question] --> E[Embed Question]
    E --> R[Retriever / Vector Search]
    R --> C[Top-k Relevant Chunks]
    C --> P[Prompt Augmentation]
    P --> L[LLM]
    L --> A[Answer]
{{< /mermaid >}}

---

## Key Building Blocks

### 1. Knowledge Source
Where your information lives:
- Documents (PDFs, Word files)
- Web pages / internal wiki
- Databases
- FAQs / manuals

---

### 2. Chunking (Preparing documents)
Documents are split into smaller pieces (“chunks”) so retrieval is accurate.

Good chunking improves:
- Relevance
- Citation quality
- Answer completeness

---

### 3. Embeddings
Both the user query and the stored chunks are converted into vectors (embeddings).

The retriever uses similarity search to find the best matches.

---

### 4. Retriever (Search)
The retriever returns the top-k most relevant chunks.

Common retrieval methods:
- Similarity search (vector search)
- Hybrid search (vector + keyword)
- Re-ranking (improves relevance)

---

### 5. Prompt Augmentation
The retrieved chunks are inserted into the prompt (context window), usually as:

- “Use the following context…”
- “Cite the sources…”
- “If context is insufficient, say so…”

---

### 6. LLM Generation
The LLM generates the final response using:
- the user query
- the retrieved context

---

## RAG vs Prompting vs Fine-tuning

| Method | What changes? | Best for |
|-------|---------------|----------|
| Prompting | Prompt text | Quick behaviour changes |
| RAG | Adds retrieved context | Private / up-to-date knowledge |
| Fine-tuning | Model weights | Style, format, consistent behaviour |

{{% hint warning %}}
If your goal is “answer from my documents”, RAG is usually the first and best option.  
Fine-tuning is not a substitute for retrieval.
{{% /hint %}}

---

## Common Failure Modes (and fixes)

- **Retriever returns irrelevant chunks**  
  → Improve chunking, embeddings, and use re-ranking

- **Answer ignores context**  
  → Strengthen prompt instructions and formatting

- **Not enough context in top-k results**  
  → Increase k, use hybrid retrieval, improve indexing

- **Confident hallucination when context is missing**  
  → Add a rule: “If context is insufficient, say you don’t know.”

---

{{< home-link "Home" >}} | {{< section-index >}}
