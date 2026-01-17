---
title: "Systems of Linear Equations"
draft: false
tags: ["Machine Learning", "Mathematics", "Linear Algebra"]
categories: ["AI", "ML"]
weight: 1130
---
# Systems of Linear Equations

A linear system can be written compactly as:

{{< katex display=true >}}
Ax=b
{{< /katex >}}

# Consistency of Linear Systems

Consider a system of linear equations written in matrix form:

{{< katex display=true >}}
A\mathbf{x} = \mathbf{b}
{{< /katex >}}

- \(A\) → **Coefficient matrix**
- \([A | b]\) → **Augmented matrix**
---

## Summary Table

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


## What you should know
- Consistent vs inconsistent systems
- Unique vs infinitely many solutions
- Geometric interpretation (lines/planes)

---

{{< home-link "Home" >}} | {{< section-index >}}
