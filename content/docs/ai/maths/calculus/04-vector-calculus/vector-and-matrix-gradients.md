---
title: "Gradients of Vector-Valued and Matrix Functions"
draft: false
tags: ["Machine Learning", "Mathematics", "Vector Calculus"]
categories: ["AI", "ML"]
weight: 1430
---
# Gradients of Vector-Valued and Matrix Functions

Covers gradients when outputs or parameters are vectors/matrices.

If f: R^n -> R^m, the derivative is the Jacobian.

{{% colour RED %}}
\[
J =
\begin{bmatrix}
\frac{\partial f_1}{\partial x_1} & \dots & \frac{\partial f_1}{\partial x_n} \\
\vdots & \ddots & \vdots \\
\frac{\partial f_m}{\partial x_1} & \dots & \frac{\partial f_m}{\partial x_n}
\end{bmatrix}
\]
{{% /colour %}}

For scalar f(x):

{{% colour RED %}}
\[
H = \nabla^2 f
\]
{{% /colour %}}

Hessian captures curvature.

---

{{< home-link "Home" >}} | {{< section-index >}}
