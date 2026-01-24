---
title: "Feature Space"
draft: false
tags: ["Machine Learning", "Mathematics", "Linear Algebra"]
categories: ["AI", "ML"]
weight: 1201
bookCollapseSection: true
---

# Feature

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

{{< home-link "Home" >}} | {{< section-index >}}
