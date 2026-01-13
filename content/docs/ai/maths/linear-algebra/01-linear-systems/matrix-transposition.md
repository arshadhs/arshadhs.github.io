---
title: "Matrix Transposition"
draft: false
weight: 1141
---

# Transposition of a Matrix

The **transpose of a matrix** is obtained by **swapping rows and columns**.

If

{{< katex display=true >}}
A = [a_{ij}]
{{< /katex >}}

then the transpose of \( A \), denoted \( A^T \), is:

{{< katex display=true >}}
A^T = [a_{ji}]
{{< /katex >}}

---

## Rules of Matrix Transposition

### 1. Transpose of a Transpose

{{< katex display=true >}}
(A^T)^T = A
{{< /katex >}}

**Intuition**  
Swapping rows and columns twice returns the original matrix.

---

### 2. Transpose of a Sum

{{< katex display=true >}}
(A + B)^T = A^T + B^T
{{< /katex >}}

**Condition**  
Matrices \( A \) and \( B \) must have the same dimensions.

**Intuition**  
Transposition distributes over addition.

---

### 3. Transpose of a Scalar Multiple

{{< katex display=true >}}
(cA)^T = cA^T
{{< /katex >}}

**Intuition**  
Scaling a matrix does not affect how rows and columns are swapped.

---

### 4. Transpose of a Product

{{< katex display=true >}}
(AB)^T = B^T A^T
{{< /katex >}}

**Important**  
The order of multiplication reverses.

**Intuition**  
Matrix multiplication depends on row–column pairing.  
Transposition swaps this pairing, which reverses the order.

---

### 5. Transpose of the Identity Matrix

{{< katex display=true >}}
I^T = I
{{< /katex >}}

**Intuition**  
The identity matrix is symmetric about its main diagonal.

---

### 6. Transpose of a Zero Matrix

{{< katex display=true >}}
0^T = 0
{{< /katex >}}

**Intuition**  
All entries are zero, so swapping rows and columns changes nothing.

---

### 7. Transpose and Inverse (Invertible Matrices)

{{< katex display=true >}}
(A^{-1})^T = (A^T)^{-1}
{{< /katex >}}

**Intuition**  
Inversion and transposition commute for invertible matrices.

---

{{< home-link "Home" >}} | {{< section-index >}}


