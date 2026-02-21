---
title: "Vector Calculus"
draft: false
tags: ["Machine Learning", "Mathematics", "Linear Algebra"]
categories: ["AI", "ML"]
weight: 1400
bookCollapseSection: true
---
# Vector Calculus

Gradients power learning. This section builds differentiation skills needed for backpropagation.

---

{{< section_tree >}}

---

{{< mermaid >}}
flowchart TD

    %% Core Node
    PD["Partial Derivatives"]

    %% Supporting Concepts
    DQ["Difference Quotient"]
    JH["Jacobian / Hessian"]
    TS["Taylor Series"]

    %% Application Chapters
    CH6["<br/>Probability"]
    CH7["<br/>Optimization"]
    CH9["<br/>Regression"]
    CH10["<br/>Dimensionality Reduction"]
    CH11["<br/>Density Estimation"]
    CH12["<br/>Classification"]

    %% Relationships
    DQ -->|defines| PD
    PD -->|collected in| JH
    JH -->|used in| TS
    JH -->|used in| CH6
	
    PD -->|used in| CH7
    PD -->|used in| CH9
    PD -->|used in| CH10
    PD -->|used in| CH11
    PD -->|used in| CH12

    %% Styling (Your Soft Academic Palette)
    style PD fill:#90CAF9,stroke:#1E88E5,color:#000

    style DQ fill:#CE93D8,stroke:#8E24AA,color:#000
    style JH fill:#CE93D8,stroke:#8E24AA,color:#000
    style TS fill:#CE93D8,stroke:#8E24AA,color:#000
    style CH6 fill:#CE93D8,stroke:#8E24AA,color:#000
	
    style CH7 fill:#C8E6C9,stroke:#2E7D32,color:#000
    style CH9 fill:#C8E6C9,stroke:#2E7D32,color:#000
    style CH10 fill:#C8E6C9,stroke:#2E7D32,color:#000
    style CH11 fill:#C8E6C9,stroke:#2E7D32,color:#000
    style CH12 fill:#C8E6C9,stroke:#2E7D32,color:#000

{{< /mermaid >}}

---

{{< home-link "Home" >}} | {{< section-index >}}
