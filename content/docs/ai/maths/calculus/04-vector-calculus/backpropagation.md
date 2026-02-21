---
title: "Backpropagation and Automatic Differentiation"
draft: false
tags: ["Machine Learning", "Mathematics", "Vector Calculus"]
categories: ["AI", "ML"]
weight: 1450
---
# Backpropagation and Automatic Differentiation

Backpropagation applies the chain rule:
- efficiently across a computational graph.
- repeatedly.

Chain rule:

{{% colour RED %}}
\[
\frac{dL}{dx} = \frac{dL}{dy} \cdot \frac{dy}{dx}
\]
{{% /colour %}}

{{< mermaid >}}
flowchart LR
    x --> y
    y --> L
{{< /mermaid >}}

Automatic differentiation computes exact derivatives efficiently using computational graphs.

---

{{< home-link "Home" >}} | {{< section-index >}}
