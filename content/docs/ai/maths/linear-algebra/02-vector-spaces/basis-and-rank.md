---
title: "Basis and Rank"
draft: false
weight: 22
tags: ["Linear Algebra", "Vector Spaces"]
---

# Basis and Rank

A **basis** is a minimal set of linearly independent vectors that spans a space.

The **dimension** of a space is the number of vectors in a basis.

## Rank

The **rank** of a matrix is the dimension of its column space (number of independent columns).

{{< katex display=true >}}
\text{rank}(A) = \dim(\text{Col}(A))
{{< /katex >}}

Rank measures the number of linearly independent columns (or rows) of a matrix.

## Interpretation
- High rank → richer information
- Low rank → redundancy

---

| Condition | Result |
|---------|--------|
| {{< katex >}}\text{rank}(A) < \text{rank}([A \mid \mathbf{b}]){{< /katex >}} | **No solution** (Inconsistent system) |
| {{< katex >}}\text{rank}(A) = \text{rank}([A \mid \mathbf{b}]){{< /katex >}}, no free variables | **Unique solution** |
| {{< katex >}}\text{rank}(A) = \text{rank}([A \mid \mathbf{b}]){{< /katex >}}, free variables exist | **Infinitely many solutions** |

---

{{% hint info %}}
**Key idea:**  
The rank measures the number of independent equations.  
Free variables indicate degrees of freedom in the solution.
{{% /hint %}}

---

{{< home-link "Home" >}} | {{< section-index >}}
