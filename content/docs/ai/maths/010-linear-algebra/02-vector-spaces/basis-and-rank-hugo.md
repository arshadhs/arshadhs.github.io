---
title: "Basis and Rank"
draft: false
weight: 20
tags: ["Linear Algebra", "Vector Spaces"]
---

# Basis and Rank

A **basis** is a minimal set of linearly independent vectors that spans a space.

The **dimension** of a space is the number of vectors in a basis.

{{% hint info %}}
Key Idea:
Basis = independence + spanning.
Rank tells us how many independent directions exist in a matrix.
{{% /hint %}}

---

# Basis (From Lecture Understanding)

From lecture discussions:

- A basis must satisfy two conditions:
  1. Vectors must be linearly independent  
  2. Vectors must span the space  

This means:

- No redundancy (independence)  
- Full coverage (spanning)  

---

# Formal Definition

{{< katex display=true >}}
\text{Span}(v_1, v_2, \dots, v_k) = V
{{< /katex >}}

{{< katex display=true >}}
c_1 v_1 + \cdots + c_k v_k = 0 \Rightarrow c_i = 0
{{< /katex >}}

---

# Why Basis Matters

- Represents space efficiently  
- Removes redundancy  
- Helps define coordinates  
- Used in ML for feature representation  

---

# Dimension

Dimension is the number of vectors in a basis.

Examples:

- 2D space → dimension = 2  
- 3D space → dimension = 3  

---

# Rank

The **rank** of a matrix is the number of linearly independent columns.

{{< katex display=true >}}
\text{rank}(A) = \dim(\text{Col}(A))
{{< /katex >}}

---

# Rank (From Lecture Insight)

From transcripts:

- Rank = number of pivot elements in REF  
- Pivot columns are independent  
- Non-pivot columns are dependent  

---

# How to Find Rank (Exam Method)

Step-by-step:

1. Convert matrix to REF  
2. Count pivot positions  
3. Number of pivots = rank  

---

# Column Space and Basis

Column space is formed by:

- All linear combinations of columns of A  

Basis of column space:

- Pivot columns of A  

Important:

- Take pivot columns from ORIGINAL matrix  
- Not from REF  

---

# Null Space Connection

{{< katex display=true >}}
A x = 0
{{< /katex >}}

- Solutions form null space  
- Free variables → dimension of null space  

{{< katex display=true >}}
\text{rank}(A) + \text{nullity}(A) = n
{{< /katex >}}

---

# Solution of Linear Systems (Important)

{{< katex display=true >}}
A x = b
{{< /katex >}}

System is consistent if:

{{< katex display=true >}}
\text{rank}(A) = \text{rank}([A \mid b])
{{< /katex >}}

---

# Interpretation of Solutions

| Condition | Result |
|---------|--------|
| {{< katex >}}\text{rank}(A) < \text{rank}([A \mid b]){{< /katex >}} | No solution |
| {{< katex >}}\text{rank}(A) = \text{rank}([A \mid b]){{< /katex >}}, no free variables | Unique solution |
| {{< katex >}}\text{rank}(A) = \text{rank}([A \mid b]){{< /katex >}}, free variables exist | Infinite solutions |

---

# Geometric Interpretation

- Rank = number of independent directions  
- Basis = coordinate system  

Examples:

- Rank 1 → line  
- Rank 2 → plane  
- Rank 3 → space  

---

# Basis from Matrix (Exam Pattern)

Steps:

1. Convert to REF  
2. Identify pivot columns  
3. Select corresponding original columns  

These columns form a basis.

---

# Example

{{< katex display=true >}}
A =
\begin{bmatrix}
1 & 2 \\
2 & 4
\end{bmatrix}
{{< /katex >}}

Second column is multiple of first.

Rank = 1  
Basis = first column  

---

# Important Theorems

1. Pivot columns form a basis  
2. Rank = number of pivots  
3. Rank ≤ min(m, n)  
4. Rank determines dimension  

---

# Hidden Exam Pattern

From lectures:

- Rank, basis, and null space are combined  
- Same matrix used across multiple sub-questions  

---

# Common Mistakes

- Taking pivot columns from REF instead of original  
- Miscounting pivots  
- Ignoring free variables  
- Confusing span with basis  

---

# Strategy to Prepare

1. Practice REF thoroughly  
2. Identify pivots correctly  
3. Link rank with null space  
4. Solve system-based questions  

---

# Quick Summary Table

| Concept | Meaning |
|--------|--------|
| Basis | Independent + spanning |
| Rank | Number of independent columns |
| Pivot columns | Basis vectors |
| Free variables | Null space dimension |

---

# References

- Lecture slides (Linear Systems, Vector Spaces)  
- Webinar explanations on rank and solutions  
- Course handout  

- [My Notes: Linear Independence]({{% relref "linear-independence.md" %}})
- [My Notes: Eigenvalues]({{% relref "eigenvalues.md" %}})

---

{{< home-link "Home" >}} | {{< section-index >}}

---
