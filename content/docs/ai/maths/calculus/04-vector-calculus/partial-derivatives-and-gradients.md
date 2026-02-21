---
title: "Partial Differentiation and Gradients"
draft: false
tags: ["Machine Learning", "Mathematics", "Vector Calculus"]
categories: ["AI", "ML"]
weight: 1420
---
# Partial Differentiation and Gradients

For f(x1, x2, ..., xn):

{{% colour RED %}}
\[
\frac{\partial f}{\partial x_i}
\]
{{% /colour %}}

Gradient vector:

{{% colour RED %}}
\[
\nabla f =
\begin{bmatrix}
\frac{\partial f}{\partial x_1} \\
\vdots \\
\frac{\partial f}{\partial x_n}
\end{bmatrix}
\]
{{% /colour %}}

Gradient points in direction of steepest ascent.

{{< mermaid >}}
flowchart LR
    Input --> Function
    Function --> Gradient
    Gradient --> Optimisation
{{< /mermaid >}}

---

{{< home-link "Home" >}} | {{< section-index >}}
