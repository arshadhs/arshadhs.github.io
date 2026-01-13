---
title: "Special Matrices"
draft: false
weight: 1126
---

## Special Matrices

Certain types of matrices have special structural properties that are widely used in linear algebra and machine learning.

---

### Symmetric Matrix

A matrix is **symmetric** if:

{{< katex display=true >}}
A^T = A
{{< /katex >}}

**Intuition**  
The matrix is symmetric about its main diagonal.

**Used in**
- Covariance matrices
- Optimisation problems
- Quadratic forms

---

### Skew-Symmetric Matrix

A matrix is **skew-symmetric** if:

{{< katex display=true >}}
A^T = -A
{{< /katex >}}

This implies all diagonal elements are zero.

**Intuition**  
The matrix is antisymmetric across the main diagonal.

**Used in**
- Rotations
- Cross-product representations
- Physics and control systems

---

### Diagonal Matrix

A **diagonal matrix** has non-zero elements only on its main diagonal.

{{< katex display=true >}}
A =
\begin{bmatrix}
a_1 & 0 & 0 \\
0 & a_2 & 0 \\
0 & 0 & a_3
\end{bmatrix}
{{< /katex >}}

**Intuition**  
Each dimension is scaled independently.

**Used in**
- Feature scaling
- Eigenvalue matrices
- Simplified computations

---

### Upper Triangular Matrix

An **upper triangular matrix** has all elements below the main diagonal equal to zero.

{{< katex display=true >}}
a_{ij} = 0 \quad \text{for } i > j
{{< /katex >}}

---

### Lower Triangular Matrix

A **lower triangular matrix** has all elements above the main diagonal equal to zero.

{{< katex display=true >}}
a_{ij} = 0 \quad \text{for } i < j
{{< /katex >}}

**Intuition (Triangular Matrices)**  
Dependencies flow in one direction.

**Used in**
- Solving linear systems
- Cholesky decomposition
- LU decomposition

---

### Sparse Matrix

A **sparse matrix** contains mostly zero elements.

**Intuition**  
Only a small number of values carry meaningful information.

**Used in**
- Recommender systems
- Large-scale machine learning
- Graph representations
- Natural language processing

Sparse matrices reduce:
- Memory usage
- Computational cost

---

{{< home-link "Home" >}} | {{< section-index >}}

