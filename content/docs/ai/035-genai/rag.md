---
title: "Retrieval-Augmented Generation (RAG)"
draft: false
tags: ["AI", "Generative AI", "LLM", "RAG"]
categories: ["AI", "ML"]
weight: 490
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
flowchart TD
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

### 4. Knowledge Base

- Smart Database (Supports CRUD Operations on Vector Data - Create, Read, Update, and Delete)

Stores:
- Embedding
- Searching Algorithm
- Text & Metadata

---

### 5. Retriever (Search)
The retriever returns the top-k most relevant chunks.

Common retrieval methods:
- Similarity search (vector search)
- Hybrid search (vector + keyword)
- Re-ranking (improves relevance)

---

### 6. Prompt Augmentation
The retrieved chunks are inserted into the prompt (context window), usually as:

- “Use the following context…”
- “Cite the sources…”
- “If context is insufficient, say so…”

---

### 7. LLM Generation
The LLM generates the final response using:
- the user query
- the retrieved context

---

## RAG Pipeline

{{< mermaid >}}
graph TB
    %% Vertical RAG Pipeline Diagram
    subgraph "RAG Pipeline"
        
        %% 1. Data Indexing Phase (Top Stack)
        subgraph Data_Indexing ["1. Data Indexing Phase"]
            direction LR
            DI1[Raw Documents] -->|Loading| DI2[Text Chunks]
            DI2 -->|Splitting| DI3[Vector Embeddings]
            DI3 -->|Embedding| DI4[(Vector DB)]
        end

        %% Explicit vertical flow constraint to force the next block down
        Data_Indexing --> Data_Retrieval_Generation

        %% 2. Retrieval & Generation Phase (Bottom Stack)
        subgraph Data_Retrieval_Generation ["2. Retrieval & Generation Phase"]
            direction LR
            DR1[User Query] -->|Embedding Search| DR3[(Vector DB)]
            DR3 -->|Retrieves Context| DR4[Top-K Chunks]
            
            DR1 -->|Prompt| DR5[LLM Engine]
            DR4 -->|Context| DR5
            DR5 -->|Output| DR6[Final Response]
        end
    end

    %% Visual Styling
    style Data_Indexing fill:#ffffff,stroke:#475569,stroke-width:1px,stroke-dasharray: 5 5
    style Data_Retrieval_Generation fill:#ffffff,stroke:#475569,stroke-width:1px,stroke-dasharray: 5 5
    
    style DI1 fill:#E2E8F0,stroke:#334155,stroke-width:1px
    style DI2 fill:#BAE6FD,stroke:#0284C7,stroke-width:1px
    style DI3 fill:#BBF7D0,stroke:#16A34A,stroke-width:1px
    style DI4 fill:#FEF08A,stroke:#CA8A04,stroke-width:2px
    
    style DR1 fill:#BAE6FD,stroke:#0284C7,stroke-width:2px
    style DR3 fill:#FEF08A,stroke:#CA8A04,stroke-width:2px
    style DR4 fill:#FCA5A5,stroke:#DC2626,stroke-width:1px
    style DR5 fill:#E9D5FF,stroke:#9333EA,stroke-width:2px
    style DR6 fill:#99F6E4,stroke:#0D9488,stroke-width:2px
{{< /mermaid >}}

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

## Core Scenarios to Implement RAG

An organisation should deploy a RAG system under the following conditions:

• **You have vast, frequently changing internal data**: If your company relies on data that updates daily or hourly (e.g., inventory levels, live ticket tracking, or market data), conventional LLMs will be outdated. RAG fetches information on demand and in real-time.

• **You need absolute accuracy and auditability**: General LLMs are prone to "hallucinations". RAG forces the model to ground its answers in your verified documentation and can provide direct citations and audit logs, making it crucial for legal, financial, or medical compliance.

• **You must protect strict data permissions**: Fine-tuning a model bakes data directly into the AI, meaning anyone using it might see sensitive info. RAG keeps data outside the model. It allows you to maintain access control, ensuring an employee only retrieves files they have explicit permission to see.

• **You want to optimize operational costs**: Training a proprietary LLM from scratch or constantly fine-tuning it is computationally expensive. RAG provides a highly cost-effective scalability model, allowing you to easily swap or upgrade the underlying LLM without losing access to your knowledge base.

---

{{< home-link "Home" >}} | {{< section-index >}}
