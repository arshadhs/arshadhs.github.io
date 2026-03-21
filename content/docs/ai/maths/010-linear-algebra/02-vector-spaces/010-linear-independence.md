---
title: "Linear Independence"
draft: false
tags: ["Machine Learning", "Mathematics", "Linear Algebra"]
categories: ["AI", "ML"]
weight: 21
---
# Linear Independence

A set of vectors is **linearly independent** if none of them can be written as a linear combination of the others.

{{< colour "yellow" >}}
{{< katex display=true >}}
c_1\mathbf{v}_1 + \cdots + c_k\mathbf{v}_k = \mathbf{0}
\;\Rightarrow\;
c_1=\cdots=c_k=0
{{< /katex >}}
{{< /colour >}}

Independence means each vector adds **new information**.

## Why it matters
- Detects redundancy
- Connects to rank and basis

{{% hint info %}}
Key Idea:
Linear independence tells us whether vectors carry unique information or are redundant.
If even one vector can be expressed using others, the set is dependent.
{{% /hint %}}

---

## Intuition (From Lectures)

Vectors represent directions in space.

If a vector lies in the span of others, it adds no new direction.

Independent vectors create new dimensions.

---

## Formal Definition

{{< colour "yellow" >}}
{{< katex display=true >}}
c_1 v_1 + c_2 v_2 + \cdots + c_k v_k = 0
{{< /katex >}}
{{< /colour >}}

{{< colour "yellow" >}}
{{< katex display=true >}}
c_1 = c_2 = \cdots = c_k = 0
{{< /katex >}}
{{< /colour >}}

---

# Linear Dependence

{{< colour "yellow" >}}
{{< katex display=true >}}
\exists \; c_i \neq 0 \text{ such that } \sum c_i v_i = 0
{{< /katex >}}
{{< /colour >}}

---

## Matrix Interpretation

{{< colour "yellow" >}}
{{< katex display=true >}}
A = [v_1 \; v_2 \; \cdots \; v_k]
{{< /katex >}}
{{< /colour >}}

Columns independent ⇔ full rank.  

---

## Rank Connection

{{< colour "yellow" >}}
{{< katex display=true >}}
\text{rank}(A) = k \Rightarrow \text{independent}
{{< /katex >}}
{{< /colour >}}

{{< colour "yellow" >}}
{{< katex display=true >}}
\text{rank}(A) < k \Rightarrow \text{dependent}
{{< /katex >}}
{{< /colour >}}

---

## Dimension Relationship (VERY IMPORTANT)

{{% hint info %}}
You cannot have more independent vectors than the dimension.
{{% /hint %}}

### Case 1

{{< colour "yellow" >}}
{{< katex display=true >}}
k > n \Rightarrow \text{always dependent}
{{< /katex >}}
{{< /colour >}}

### Case 2

{{< colour "yellow" >}}
{{< katex display=true >}}
k = n \Rightarrow \text{check independence}
{{< /katex >}}
{{< /colour >}}

### Case 3

{{< colour "yellow" >}}
{{< katex display=true >}}
k < n \Rightarrow \text{can be independent}
{{< /katex >}}
{{< /colour >}}

---

## Visual Intuition

{{< mermaid >}}
flowchart LR
A[2D Space] --> B[2 Independent Vectors]
B --> C[Full Span]
A --> D[3 Vectors]
D --> E[Dependent]
{{< /mermaid >}}

---

## Examples

### Dependent

{{< colour "yellow" >}}
{{< katex display=true >}}
(1,2), (2,4)
{{< /katex >}}
{{< /colour >}}

### Independent

{{< colour "yellow" >}}
{{< katex display=true >}}
(1,0), (0,1)
{{< /katex >}}
{{< /colour >}}

---

## Quick Summary

| Case | Result |
|------|--------|
| k > n | Dependent |
| k = n | Check |
| k < n | Can be independent |

---

## REF Method

1. Convert to REF  
2. Find pivot columns  
3. Check pivots  

---

## Geometric Interpretation

Independent → different directions  
Dependent → same plane/line  

---

## Basis Connection

Basis = independent + spanning

---

## Example

{{< katex display=true >}}
v_1 = (1,2), \quad v_2 = (2,4)
{{< /katex >}}

{{< katex display=true >}}
v_2 = 2 v_1
{{< /katex >}}

Dependent.

---

## Exam Focus

- REF method  
- Rank relation  
- Null space connection  

---

## References

- Lecture slides  
- Course handout  

---

{{< home-link "Home" >}} | {{< section-index >}}

---

