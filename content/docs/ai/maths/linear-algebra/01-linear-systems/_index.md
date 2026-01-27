---
title: "Linear Systems"
draft: false
tags: ["Machine Learning", "Mathematics", "Linear Algebra"]
categories: ["AI", "ML"]
weight: 1120
bookCollapseSection: true
---
# Linear Systems

How systems of linear equations are represented and solved using matrices.

-  the study of vectors and rules to manipulate
vectors
- describe multiple linear equations solved simultaneously
- connect algebraic equations with matrix representations

---
![Matrix](/images/ai/matrix_vector_operations.png)

---

## Idea of Closure

-  performing a specific operation (like addition or multiplication) on members of a set always produces a result that belongs to the same set

- idea of closure is fundamental to defining a **[Vector space]**(/docs/ai/linear-algebra/01-linear-systems) because it ensures that performing arithmetic operations (addition and scalar multiplication) on vectors within a set does not produce a new element outside that set. 

---

## Machine Learning View

Many machine learning models can be abstracted as:

{{< katex display=true >}}
y = Wx + b
{{< /katex >}}

x → input data

W → weight, how model transforms it

𝑏 → bias adjusts the result

y → is the output

> **Models transform input vectors using matrices.**

---

{{< home-link "Home" >}} | {{< section-index >}}
