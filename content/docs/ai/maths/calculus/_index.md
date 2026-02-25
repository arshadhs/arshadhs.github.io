---
title: "Calculus"
draft: false
tags: ["Mathematics", "Calculus", "Derivatives", "Integration"]
categories: ["AI", "ML"]
weight: 1140
menu: main
bookCollapseSection: true
---

# Calculus

Calculus is:
- the mathematical framework for understanding and controlling how quantities change
- the mathematics of **change** and **accumulation**

It helps answer:
- How fast is something changing **right now**?
- What happens when inputs change **slightly**?
- Where is something **maximum or minimum**?

It answers two big questions:
- **How fast is something changing right now?** → derivatives (differentiation)
- **How much has accumulated over an interval?** → integrals (integration)

---

{{< mermaid >}}
flowchart TD
  A[Calculus] --> B[Limits]
  B --> C[Continuity]
  B --> D[Derivatives]
  B --> E[Integrals]
  D --> F[Optimisation: maxima/minima]
  D --> G[ML: gradients & learning]
  E --> H[Accumulation: area/total change]
{{< /mermaid >}}

---

{{< section_tree >}}

---

{{% steps %}}

1. ## Differential Calculus (Rates of Change)

	Studies **how things change**.

	- How steep is a curve at a point?
	- Is something increasing or decreasing?
	- Where are the maxima and minima?
	- The key idea is the **derivative**.

	A **derivative** measures how a small change in input affects the output.

	Example intuition:
	- Slope of a curve
	- Instantaneous speed
	- Gradient of a loss function

2. ## Integral Calculus (Accumulation)

	Studies **how things add up**.

	- What is the total effect over time?
	- How much area lies under a curve?
	- How do small changes accumulate?

	The key idea is the **integral**.

	Example intuition:
	- Total distance from speed
	- Area under a curve
	- Summing many tiny contributions

{{% /steps %}}

---

## Differentiation

Differentiation is used to calculate **rates of change**.

### Real-life intuition

In mechanics:
- Rate of change of displacement with respect to time → **velocity**
- Rate of change of velocity with respect to time → **acceleration**

---

## Derivative and common notations

If

{{< katex display=true >}}
y=f(x)
{{< /katex >}}

the derivative is the rate of change of \(y\) with respect to \(x\).

Notations:
- **Leibniz notation**: \(\dfrac{dy}{dx}\)
- **Prime notation**: \(f'(x)\)
- **Operator notation**: \(\dfrac{d}{dx}(f(x))\)

Different questions often mean the same thing:
- “Differentiate the function …”
- “Find \(f'(x)\)”
- “Find \(\dfrac{dy}{dx}\)”
- “Find the derivative of …”
- “Calculate the gradient of the tangent to the curve …”
- “Calculate the rate of change of …”

---

## The idea behind the derivative

The derivative at a point measures the slope of the tangent line.

{{% colour colour="indigo" %}}
{{< katex display=true >}}
f'(a)=\lim_{h\to 0}\frac{f(a+h)-f(a)}{h}
{{< /katex >}}
{{% /colour %}}

---

## Basic differentiation rules

### Power rule

If

{{% colour colour="indigo" %}}
{{< katex display=true >}}
f(x)=a x^n
{{< /katex >}}
{{% /colour %}}

then

{{% colour colour="indigo" %}}
{{< katex display=true >}}
f'(x)=na x^{n-1}
{{< /katex >}}
{{% /colour %}}

Key fact:
- bring the power down to the front
- subtract 1 from the power

---

## Trigonometric derivatives

{{% colour colour="indigo" %}}
{{< katex display=true >}}
\frac{d}{dx}(\sin x)=\cos x,\qquad \frac{d}{dx}(\cos x)=-\sin x
{{< /katex >}}
{{% /colour %}}

Key fact:
- these standard trig derivatives assume angles are measured in **radians**

---

## The Chain Rule

The chain rule is used to differentiate **composite functions** (a function inside another function).

If \(y\) depends on \(u\), and \(u\) depends on \(x\), then:

{{% colour colour="indigo" %}}
{{< katex display=true >}}
\frac{dy}{dx}=\frac{dy}{du}\cdot\frac{du}{dx}
{{< /katex >}}
{{% /colour %}}

Intuition:
- differentiate the outside function
- keep the inside
- multiply by the derivative of the inside

---

## Leibniz Notation (why it’s useful)

Leibniz notation is widely used because it clearly shows **which variable** you are differentiating with respect to.

### What it tells you
- **Derivative**: the derivative of $(y$) with respect to \(x\) is $\dfrac{dy}{dx}$
- **Operator**: \(\dfrac{d}{dx}\) means “differentiate with respect to \(x\)”
- **Higher-order derivatives**: \(\dfrac{d^2y}{dx^2}\), \(\dfrac{d^3y}{dx^3}\), etc.
- **Chain rule** is naturally expressed as products like \(\dfrac{dy}{du}\dfrac{du}{dx}\)
- **Integrals** pair \(\int\) with \(dx\), e.g. \(\int f(x)\,dx\)

### Advantages
- Explicit variables: helpful for multivariable work
- Fraction-like behaviour: often behaves like fractions in chain rule and differential equations
- Dimensional analysis: units of \(\dfrac{dy}{dx}\) are “units of \(y\) per unit of \(x\)”

### Comparison to other notations
- **Newton’s notation**: dots (e.g. \(\dot{x}\)) mainly for time derivatives in physics
- **Lagrange’s notation**: primes (e.g. \(f'(x)\)) compact but less explicit about the independent variable

---

## Integration (big picture)

Integration is about **accumulation**.

Typical interpretations:
- area under a curve
- total change built up from a rate

The key link between differentiation and integration is that they are inverse ideas (captured formally by the Fundamental Theorem of Calculus, which you’ll meet later).

---

## Multivariate Calculus

Multivariate calculus deals with functions of **more than one variable**.

Univariate (single variable):

{{< katex display=true >}}
y = f(x)
{{< /katex >}}

Multivariate (many variables):

{{< katex display=true >}}
z = f(x, y)
{{< /katex >}}

{{< katex display=true >}}
L(w_1, w_2, \dots, w_n)
{{< /katex >}}

In machine learning, almost every function is **multivariate**.

---

## Why Multivariate Calculus Matters in Machine Learning

Machine learning models do not learn one parameter at a time.  
They optimise many parameters simultaneously.

Example:

{{< katex display=true >}}
\text{Loss}(w_1, w_2, \dots, w_n)
{{< /katex >}}

Multivariate calculus tells us:
- how changing each parameter affects the output
- which direction reduces the error fastest
- whether a solution is a minimum, maximum, or saddle point

---

## Key Topics (for ML)

- Univariate differentiation (revision)
- Partial derivatives
- Gradients
- Jacobian and Hessian
- Gradients of vectors and matrices
- Useful gradient identities
- Backpropagation (conceptual)
- Automatic differentiation

---

## Focus

- Compute gradients correctly
- Use Hessian intuition for minima/maxima
- Understand Taylor series (multivariate)

---

## ML Connection

- Training neural networks (gradient-based learning)

Most ML training is **optimisation**:
- define a loss function
- compute how it changes (derivatives / gradients)
- update parameters to reduce the loss

This is why derivatives (and later, gradients) are central to learning algorithms like gradient descent.

---

{{< home-link "Home" >}} | {{< section-index >}}
