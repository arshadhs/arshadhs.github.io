---
title: 'Instance-based Learning'
draft: false
tags: ["AI", "ML"]
categories: ["AI", "ML"]
weight: 600
---
# Instance-based Learning 

## k-Nearest Neighbor Learning  

### Distance / similarity choices

#### Cosine similarity vs Euclidean distance

Cosine similarity compares direction (angle), not magnitude:

{{< colour "blue" >}}
{{< katex display=true >}}
\cos(\theta)=\frac{x\cdot y}{\|x\|\|y\|}
{{< /katex >}}
{{< /colour >}}

When to use cosine:
- text/document vectors (high-dimensional, sparse)
- when vector length should not dominate similarity

Euclidean distance measures straight-line distance:

{{< colour "blue" >}}
{{< katex display=true >}}
d(x,y)=\|x-y\|
{{< /katex >}}
{{< /colour >}}

When to use Euclidean:
- continuous features on comparable scales (often after normalisation)

---

## Locally Weighted Regression (LWR) Learning  

## Radial Basis Functions  

---

{{< home-link "Home" >}} | {{< section-index >}}

