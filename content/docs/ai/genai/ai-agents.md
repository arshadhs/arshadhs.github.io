---
title: "AI Agents"
date: 2025-12-15T10:55:52+01:00
draft: false
tags: ["AI", "ANI", "AGI", "ASI"]
categories: ["AI", "ML"]
weight: 480
menu: main
---
# AI Agents

Also referred to as Agentic AI.

AI agents are **intelligent systems** that can **plan, make decisions, and take actions** to achieve goals with **minimal human intervention**.

- A common use case is **task automation**
- for example booking travel based on a user’s request.

- AI agents typically build on **Generative AI** and use **Large Language Models (LLMs)** as the reasoning core.
- Agents often interact with tools (APIs, databases, calendars) to complete multi-step workflows.

---

## How an AI Agent Works

{{< mermaid >}}
flowchart TD
    U[User Request] --> LLM[LLM Reasoning]
    LLM --> P[Plan]
    P --> T[Use Tools]
    T --> O[Observe Results]
    O --> R[Review and Decide Next Step]
    R --> D{Goal Achieved}
    D -->|No| LLM
    D -->|Yes| A[Final Answer or Action]

    style U fill:#C8E6C9
    style LLM fill:#BBDEFB
    style P fill:#90CAF9
    style T fill:#64B5F6
    style O fill:#90CAF9
    style R fill:#BBDEFB
    style D fill:#FFE0B2
    style A fill:#FFCDD2
{{< /mermaid >}}

---

## example: Travel Booking Agent
- User: “Book me a weekend trip to Edinburgh next month within £300.”
- Agent:
  - creates a plan (dates, travel, hotel, budget)
  - searches options using tools/APIs
  - compares results and checks constraints
  - confirms details and completes the booking (if allowed)

---

- **Agents** are goal-driven systems that can run multi-step workflows.
- They combine **LLM reasoning** with **tool use** and **feedback loops**.
- They are widely used for automation and decision support.

---

---
{{< home-link "Home" >}} | {{< section-index >}}  
