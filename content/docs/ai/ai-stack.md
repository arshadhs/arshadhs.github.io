---
title: "AI Stack"
draft: false
tags: ["AI"]
categories: ["AI"]
weight: 200
menu: main
---
# AI Stack

The **AI Stack** describes the **layers required to build an end-to-end AI system**, from infrastructure at the bottom to user-facing applications at the top.

Different organisations represent the AI stack differently; this is a simplified conceptual view for learning.

Each layer depends on the one below it.

---
{{< mermaid >}}
graph TB

    subgraph APP["Applications"]
        A[User Interfaces & Integrations]
    end

    subgraph ORCH["Orchestration"]
        O[Workflows • Agents • Control Logic]
    end

    subgraph DATA["Data"]
        D[Data Sources • Pipelines • Vector DBs]
    end

    subgraph MODEL["Models"]
        M[ML • DL • Foundation Models • LLMs]
    end

    subgraph INFRA["Infrastructure"]
        I[Cloud • On-prem • GPUs • Storage]
    end

    %% Styling
    style APP fill:#FFCCBC
    style ORCH fill:#90CAF9
    style DATA fill:#BBDEFB
    style MODEL fill:#C8E6C9
    style INFRA fill:#E1F5FE

    style A fill:#FFE0B2
    style O fill:#B3E5FC
    style D fill:#E3F2FD
    style M fill:#DCEDC8
    style I fill:#E1F5FE
{{< /mermaid >}}


---

## 1. Infrastructure
The foundation that provides **compute and storage**.

- Cloud (AWS, Azure, GCP)
- On-premise servers
- Local machines (laptops, edge devices)
- CPUs, GPUs, TPUs
- Networking and storage

> Without infrastructure, AI cannot run or scale.

---

## 2. Models
The **intelligence layer** of the system.

- Open-source or proprietary models
- Small models or large models (LLMs)
- General-purpose or specialised models
- Examples:
  - Classical ML models
  - Neural Networks
  - Deep Learning models
  - Foundation Models
  - LLMs

> Models transform data into predictions or generated content.

---

## 3. Data
The **fuel** for AI systems.

- Data sources (databases, APIs, files, sensors)
- Data pipelines (ingestion, cleaning, transformation)
- Structured and unstructured data
- Vector databases for embeddings

> Better data usually matters more than bigger models.

---

## 4. Orchestration
The **control layer** that manages AI behaviour.

- Decides **when** and **how** models are used
- Combines:
  - Thinking
  - Execution
  - Review and feedback
- Handles workflows, retries, and tool usage

> This is where modern AI systems become *intelligent systems*, not just models.

---

## 5. Applications
The **user-facing layer**.

- Interfaces:
  - Text
  - Images
  - Audio
  - Numerical data
- Integrations:
  - Inputs (users, systems)
  - Outputs (dashboards, APIs, actions)

> This is the only layer users usually see.

---

## One-Line Summary
- **Infrastructure** runs everything  
- **Models** provide intelligence  
- **Data** feeds the models  
- **Orchestration** coordinates behaviour  
- **Applications** deliver value to users  

---

{{< mermaid >}}
flowchart TB
    INFRA[Infrastructure]
    MODEL[Models]
    DATA[Data]
    ORCH[Orchestration]
    APP[Applications]

    INFRA --> MODEL
    MODEL --> DATA
    DATA --> ORCH
    ORCH --> APP
{{< /mermaid >}}

---

{{< home-link "Home" >}} | {{< section-index >}}
