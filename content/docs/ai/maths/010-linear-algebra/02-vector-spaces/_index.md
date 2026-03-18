---
title: "Vector Spaces"
draft: false
tags: ["Machine Learning", "Mathematics", "Linear Algebra"]
categories: ["AI", "ML"]
weight: 1200
bookCollapseSection: true
---
# Vector Spaces

A vector space is the mathematical “home” where vectors live and where addition and scaling are valid operations.

- A vector space is a set closed under vector addition and scalar multiplication.
- Machine learning operates in vector spaces.

- covers independence, bases, rank, and geometric tools like norms and inner products that are used to measure length, distance, and angles.

A **vector space** is a set of vectors that follows **ten axioms**, defined under two operations:
- Vector addition  
- Scalar multiplication  

These axioms ensure consistent linear behaviour.

It also:
- Contains a zero vector  
- Contains additive inverses  

### Geometric Intuition
A vector space is the **entire space where vectors live**.

Examples:
- A line through the origin  
- A plane through the origin  
- Higher-dimensional spaces  

### In Machine Learning
Feature spaces and embedding spaces are vector spaces.

---

{{< section_tree >}}

---

{{< mermaid >}}
flowchart TD
  VS[Vector Spaces] --> LI[Linear Independence]
  VS --> BR[Basis & Rank]
  VS --> N[Norms]
  VS --> IP[Inner Products]
  VS --> LD[Lengths & Distances]
  VS --> AO[Angles & Orthogonality]
  VS --> ONB[Orthonormal Basis]
{{< /mermaid >}}

---

## Key components
- Zero vector
- Additive inverse
- Closure under operations

# Vector Spaces and Feature Spaces

Machine learning operates in **vector spaces**.  
Understanding these spaces is essential for reasoning about dimensionality, structure, and representations.

---

## Vector Subspace

### Definition
A **vector subspace** is a subset of a vector space that satisfies all vector space conditions.

A subspace must:
- Contain the zero vector
- Be closed under addition
- Be closed under scalar multiplication

### Geometric Intuition
A subspace is a **smaller space inside a larger space** that still behaves like a full space.

Examples:
- A line inside a plane  
- A plane inside 3D space  

### In Machine Learning
Subspaces capture **lower-dimensional structure** in data, such as the space spanned by principal components.

---

## Axioms of a Vector Space

### Properties of Vector Addition

- **Closure** {{< katex display=true >}}\mathbf{u}, \mathbf{v} \in V \Rightarrow \mathbf{u} + \mathbf{v} \in V{{< /katex >}}
- **Commutativity**  {{< katex display=true >}}\mathbf{u} + \mathbf{v} = \mathbf{v} + \mathbf{u}{{< /katex >}}
- **Associativity**  
{{< katex display=true >}}
(\mathbf{u} + \mathbf{v}) + \mathbf{w} = \mathbf{u} + (\mathbf{v} + \mathbf{w})
{{< /katex >}}
- **Additive Identity**  
{{< katex display=true >}}
\mathbf{u} + \mathbf{0} = \mathbf{u}
{{< /katex >}}
- **Additive Inverse**  
{{< katex display=true >}}
\mathbf{u} + (-\mathbf{u}) = \mathbf{0}
{{< /katex >}}

---

### Properties of Scalar Multiplication

- **Distributivity over Vector Addition**  
{{< katex display=true >}}
c(\mathbf{u} + \mathbf{v}) = c\mathbf{u} + c\mathbf{v}
{{< /katex >}}
- **Distributivity over Scalar Addition**  
{{< katex display=true >}}
(c + d)\mathbf{u} = c\mathbf{u} + d\mathbf{u}
{{< /katex >}}
- **Associativity of Scalars**  
{{< katex display=true >}}
c(d\mathbf{u}) = (cd)\mathbf{u}
{{< /katex >}}
- **Multiplicative Identity**  
{{< katex display=true >}}
1\mathbf{u} = \mathbf{u}
{{< /katex >}}
- **Zero Property**  
{{< katex display=true >}}
0\mathbf{u} = \mathbf{0}, \quad c\mathbf{0} = \mathbf{0}
{{< /katex >}}

---

## Why This Matters in ML

- Data lives in vector spaces  
- Models manipulate these spaces  
- Learning often discovers meaningful subspaces  

---

{{< home-link "Home" >}} | {{< section-index >}}
