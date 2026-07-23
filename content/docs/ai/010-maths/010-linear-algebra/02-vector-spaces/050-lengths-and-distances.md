---
title: "Lengths and Distances"
draft: false
weight: 50
tags: ["Linear Algebra", "Vector Spaces"]
---

# Lengths and Distances

The **length** of a vector is given by its norm.

The **distance** between two points (vectors) is the norm of their difference.

Distance quantifies **how far two vectors (data points) are** from each other.

{{< katex display=true >}}
d(\mathbf{x},\mathbf{y}) = \lVert \mathbf{x} - \mathbf{y} \rVert
{{< /katex >}}

{{% hint info %}}
Key Idea:
Length measures size of a single vector.
Distance measures separation between two vectors.
Distance = norm applied to difference.
{{% /hint %}}

## Why it matters
- many ML algorithms depend on distances in feature space

---

# Length of a Vector

From analytic geometry lectures:

The length of a vector is its norm.

For vector:

{{< katex display=true >}}
x = (x_1, x_2, \dots, x_n)
{{< /katex >}}

Length (L2 norm):

{{< katex display=true >}}
\lVert x \rVert = \sqrt{x_1^2 + x_2^2 + \cdots + x_n^2}
{{< /katex >}}

---

# Interpretation of Length

- Measures distance from origin  
- Indicates magnitude of vector  
- Independent of direction  

Examples:

- Small length → close to origin  
- Large length → far from origin  

---

# Distance Between Two Vectors

Distance is defined as:

{{< katex display=true >}}
d(x, y) = \lVert x - y \rVert
{{< /katex >}}

Steps:

1. Subtract vectors  
2. Compute norm  

---

# Expanded Formula

For vectors:

{{< katex display=true >}}
x = (x_1, x_2, \dots, x_n), \quad y = (y_1, y_2, \dots, y_n)
{{< /katex >}}

Distance:

{{< katex display=true >}}
d(x,y) = \sqrt{(x_1 - y_1)^2 + \cdots + (x_n - y_n)^2}
{{< /katex >}}

---

# Geometric Interpretation

- Distance = straight-line (Euclidean) distance  
- Represents shortest path between two points  

In 2D:

- forms a triangle  
- follows Pythagoras theorem  

---

# Connection to Inner Product

From lectures:

{{< katex display=true >}}
\lVert x \rVert = \sqrt{x^T x}
{{< /katex >}}

Distance can be written as:

{{< katex display=true >}}
\lVert x - y \rVert = \sqrt{(x - y)^T (x - y)}
{{< /katex >}}

---

# Important Properties of Distance

## 1. Non-negativity

{{< katex display=true >}}
d(x,y) \geq 0
{{< /katex >}}

---

## 2. Identity

{{< katex display=true >}}
d(x,y) = 0 \iff x = y
{{< /katex >}}

---

## 3. Symmetry

{{< katex display=true >}}
d(x,y) = d(y,x)
{{< /katex >}}

---

## 4. Triangle Inequality

{{< katex display=true >}}
d(x,z) \leq d(x,y) + d(y,z)
{{< /katex >}}

---

# Other Distance Measures

## L1 Distance

{{< katex display=true >}}
d(x,y) = \sum |x_i - y_i|
{{< /katex >}}

---

## Infinity Distance

{{< katex display=true >}}
d(x,y) = \max |x_i - y_i|
{{< /katex >}}

---

# Why Distance Matters (ML Insight)

From lectures and applications:

Distances are used in:

- k-NN (nearest neighbour)
- clustering algorithms
- similarity measures
- recommendation systems

Key idea:

- closer points → more similar  
- farther points → less similar  

---

# Distance and Feature Space

In ML:

- each vector = data point  
- space = feature space  

Distance determines:

- grouping of data  
- classification boundaries  

---

# Example

Given:

{{< katex display=true >}}
x = (1,2), \quad y = (4,6)
{{< /katex >}}

{{< katex display=true >}}
d(x,y) = \sqrt{(1-4)^2 + (2-6)^2}
{{< /katex >}}

{{< katex display=true >}}
= \sqrt{9 + 16} = 5
{{< /katex >}}

---

# Connection to Norms

Key relation:

{{< katex display=true >}}
\text{Length} = \lVert x \rVert
{{< /katex >}}

{{< katex display=true >}}
\text{Distance} = \lVert x - y \rVert
{{< /katex >}}

So distance is derived from norm.

---

# Hidden Exam Pattern

From lectures:

- Distance is combined with:
  - norm  
  - inner product  
  - angles  

👉 rarely asked in isolation  

---

# Common Mistakes

- Forgetting subtraction step  
- Mixing L1 and L2 distance  
- Missing square root  
- Arithmetic errors in expansion  

---

# Strategy to Prepare

1. Practice distance calculations  
2. Understand geometric meaning  
3. Link with norm and inner product  
4. Solve ML-style problems  

---

# Quick Summary Table

| Concept | Formula | Meaning |
|--------|--------|--------|
| Length | \lVert x \rVert | Distance from origin |
| Distance | \lVert x - y \rVert | Distance between points |
| L2 | sqrt(sum of squares) | Euclidean |
| L1 | sum of absolute values | Manhattan |

---

# References

- Lecture slides (Analytic Geometry)  
- Course handout  

---

{{< home-link "Home" >}} | {{< section-index >}}

---
