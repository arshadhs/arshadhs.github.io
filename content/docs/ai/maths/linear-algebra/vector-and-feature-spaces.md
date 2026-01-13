---
title: "Vector Spaces and Feature Spaces"
draft: false
tags: ["Machine Learning", "Mathematics", "Linear Algebra"]
categories: ["AI", "ML"]
weight: 1150
---

# Vector Spaces and Feature Spaces

Machine learning operates in **vector spaces**.  
Understanding these spaces is essential for reasoning about dimensionality, structure, and representations.

---

## Feature

### Definition
A **feature** is an individual measurable property or characteristic of a data point used as input to a machine learning model.

Each feature corresponds to **one dimension**.

{{< katex display=true >}}
x_i \in \mathbb{R}
{{< /katex >}}

A data point with \( d \) features is represented as:

{{< katex display=true >}}
\mathbf{x} =
\begin{bmatrix}
x_1 \\
x_2 \\
\vdots \\
x_d
\end{bmatrix}
{{< /katex >}}

### Geometric Intuition
A feature is like **one axis** in a coordinate system.

- One feature → line  
- Two features → plane  
- Three features → 3D space  

Each new feature adds a **new dimension**.

### Why Features Matter
Models learn patterns in features, not raw objects.

Good features:
- Capture relevant information  
- Make patterns easier to detect  

Poor features:
- Add noise  
- Increase dimensionality unnecessarily  

---

## Feature Space

### Definition
A **feature space** is the vector space formed by all features, where each data point is represented as a vector.

{{< katex display=true >}}
\mathbf{x} \in \mathbb{R}^d
{{< /katex >}}

Here, \( d \) is the number of features.

### Geometric Intuition
A feature space is a **coordinate system**:

- 1 feature → line  
- 2 features → plane  
- 3 features → 3D space  
- Many features → high-dimensional space  

Each data point becomes a point (or vector) in this space.

### Relation to Linear Algebra
Feature spaces are:
- Vector spaces  
- Often high-dimensional  
- Manipulated using matrices and vectors  

Operations such as projection, rotation, and dimensionality reduction occur directly in feature space.

---

## Vector Space

### Definition
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


