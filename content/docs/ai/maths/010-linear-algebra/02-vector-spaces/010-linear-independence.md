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

If one vector can already be formed using others, it does not add anything new.  
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
From lecture explanation:
- Think of vectors as “features”  
- Independent vectors → new feature  
- Dependent vectors → repeated feature  

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

This means:
- only trivial solution exists  
- no vector depends on others  
---

# Linear Dependence

{{< colour "yellow" >}}
{{< katex display=true >}}
\exists \; c_i \neq 0 \text{ such that } \sum c_i v_i = 0
{{< /katex >}}
{{< /colour >}}
In lectures, this was linked to solving:

{{< katex display=true >}}
A x = 0
{{< /katex >}}

If non-trivial solution exists → dependent  

---

## Matrix Interpretation

{{< colour "yellow" >}}
{{< katex display=true >}}
A = [v_1 \; v_2 \; \cdots \; v_k]
{{< /katex >}}
{{< /colour >}}

Columns independent ⇔ full rank.  

From lecture:
- pivot columns → independent  
- non-pivot columns → dependent  

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

Lecture insight:
Rank tells how many “useful” columns exist.  
Anything beyond rank is redundant.  
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

Lecture intuition:
Space has limited directions.  
Extra vectors must reuse existing ones.  

---
### Case 2

{{< colour "yellow" >}}
{{< katex display=true >}}
k = n \Rightarrow \text{check independence}
{{< /katex >}}
{{< /colour >}}

Check using:
- determinant  
- rank  

---

### Case 3

{{< colour "yellow" >}}
{{< katex display=true >}}
k < n \Rightarrow \text{can be independent}
{{< /katex >}}
{{< /colour >}}

But still check:
- vectors might lie in same direction  

---

## Visual Intuition

{{< mermaid >}}
flowchart LR
A[2D Space] --> B[2 Independent Vectors]
B --> C[Full Span]
A --> D[3 Vectors]
D --> E[Dependent]
{{< /mermaid >}}

Interpretation:
- 2 vectors → enough for plane  
- extra vector → redundant  

---

## Examples

### Dependent

{{< colour "yellow" >}}
{{< katex display=true >}}
(1,2), (2,4)
{{< /katex >}}
{{< /colour >}}

Second vector is multiple → no new direction  

---

### Independent

{{< colour "green" >}}
{{< katex display=true >}}
(1,0), (0,1)
{{< /katex >}}
{{< /colour >}}

Both directions unique  

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

