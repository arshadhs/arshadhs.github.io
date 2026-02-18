---
title: "Linear NN for Regression"
date: 2026-02-15
draft: false
weight: 250
tags: ["Deep Learning", "Regression", "Optimisation"]
categories: ["AI", "Machine Learning"]
---

# Linear Neural Networks for Regression

A **linear neural network for regression** is a model that predicts a **continuous** target by taking a weighted sum of input features and applying the **identity activation** (so the output can be any real number).

## Regression

Regression is a supervised learning task that predicts a continuous-valued output based on input features.

Typical use-cases: 
- house prices prediction
- temperature forecasting
- stock price prediction
- other continuous predictions

## Linear Regression

Linear regression assumes the output is a linear combination of the input features.

{{% hint info %}}
**Linear regression as a single-neuron neural network** with an **identity activation**.  
{{% /hint %}}

## Mathematical Form

{{% hint danger %}}
{{< katex display=true >}}
\hat{y} = w_0 + w_1x_1 + \cdots + w_dx_d
{{< /katex >}}
{{% /hint %}}

where:
- {{< katex >}}x = [x_1, x_2, \dots, x_d]^T{{< /katex >}} are the input features
- {{< katex >}}w = [w_1, w_2, \dots, w_d]^T{{< /katex >}} are the weights
- {{< katex >}}w_0{{< /katex >}} is the bias term

## The Four Components

A complete ML system has four components:
- **Data**: inputs {{< katex >}}X{{< /katex >}} and targets {{< katex >}}y{{< /katex >}}
- **Model**: Single neuron implementing linear function - the function that maps inputs to predictions
- **Objective**: Measures prediction error - a loss that measures error
- **Learning algorithm**: an optimiser (Gradient descent to optimise weights)

{{% hint info %}}
This same template extends directly to multi-layer networks:  
only the model {{< katex >}}f_\theta{{< /katex >}} becomes deeper, and backpropagation computes gradients efficiently.
{{% /hint %}}

## Linear Model Prediction

Model: A single neuron computes a weighted sum and applied identity activation function.

## Identity Activation Function

Properties of Identity Function Output: Continuous, (-\infty, \infty)  
Differentiable everywhere: {{< katex >}}f'(z) = 1{{< /katex >}} Allows output to take any real value  
Gradient flows unchanged through the neuron Perfect for predicting continuous targets  
Why identity for regression?  
Target values are continuous (prices, temperatures, etc.) Need to predict any real number, not just binary {{< katex >}}\{0,1\}{{< /katex >}} Differentiability enables gradient-based optimisation  
No information loss from input to output

## Objective Function: Squared Error Loss

Measure how well our model fits the data.

**Squared error (MSE) objective**

### Why Squared Error?

Advantages of squared error loss:  
Differentiable: Smooth everywhere, easy to optimise Convex: Single global minimum (for linear models) Penalises large errors: Quadratic growth  
Statistical interpretation: Maximum likelihood under Gaussian noise

---

## Gradient Descent Algorithm

Iteratively update weights in the direction of steepest descent

- Cost Function: The curve represents the cost function, labeled {{< katex >}}J(w){{< /katex >}} on the vertical axis. This function measures the "cost" or error of the model's predictions based on a given weight parameter {{< katex >}}w{{< /katex >}} on the horizontal axis. 
- Objective: The goal of the algorithm is to find the parameter {{< katex >}}w{{< /katex >}} that minimises the cost function, reaching the "Global cost minimum", {{< katex >}}J_{\min}(w){{< /katex >}}. 
- Initial Point: The process begins at an "Initial weight" (the black dot), a randomly chosen starting point on the cost function curve. 
- Gradient and Iteration: The "Gradient" (dashed line) represents the slope of the tangent at the current point. The algorithm iteratively updates the weight by moving in the direction opposite to the gradient (down the slope), as indicated by the arrows. This process is repeated until the minimum is reached, effectively minimising the model's error. The update rule is typically {{< katex >}}w = w - \eta \cdot \nabla J(w){{< /katex >}}, where {{< katex >}}\eta{{< /katex >}} is the learning rate and {{< katex >}}\nabla J(w){{< /katex >}} is the gradient. 

### Computing GDA

In a neural network, the gradient is a vector that represents the direction and magnitude of the steepest ascent of the loss function.

It is calculated using the partial derivatives of the loss function with respect to each of the network's parameters (weights and biases).

During training, this information is used to guide the process of gradient descent, which adjusts the parameters in the opposite direction of the gradient to minimise the loss and improve the model's predictions

Both Batch Gradient Descent and Stochastic Gradient Descent are useful optimization algorithms that serve different purposes depending on the problem at hand.

### Variations of GDA

Choosing between the two algorithms depends on factors like the size of the dataset, computational resources and the nature of the error surface.

#### Batch GDA
Uses the entire dataset to calculate the gradient and update parameters in one go.
Batch Gradient Descent is more accurate but slower and computationally expensive. It is ideal when working with small to medium-sized datasets and when high accuracy is required.

#### Stochastic GDA (SGD)
Updates parameters based on one training sample at a time.
Stochastic Gradient Descent, on the other hand, is faster and requires less computational power, making it suitable for large datasets. It can also escape local minima more easily but may converge less accurately.

#### Mini-batch Gradient Descent (Mid)
The most commonly used middle ground, using small random subsets (e.g., 32-512 samples) to update. It balances speed (via GPU optimization) with stability, providing faster convergence than Batch and smoother updates than Stochastic. 

### Which to choose?
- Small/Medium Data: Batch GD is fine.
- Large Data/Deep Learning: Mini-batch is the standard approach.
- Online Learning/Very Fast Updates: Stochastic GD. 

---

## Computing Gradient

This is the forward → loss → gradient → update pipeline:

1. **Forward**: {{< katex >}}\hat{y} = Xw{{< /katex >}}  
2. **Error**: {{< katex >}}e = \hat{y} - y{{< /katex >}}  
3. **Loss**: {{< katex >}}J(w) = \frac{1}{N} e^T e{{< /katex >}}  
4. **Gradient**: {{< katex >}}\nabla J(w) = \frac{2}{N}X^Te{{< /katex >}}  
5. **Update**: {{< katex >}}w \leftarrow w - \eta \nabla J(w){{< /katex >}}

---
...
---

## Summary
- Linear regression can be viewed as a **single-neuron neural network** with **identity activation**.
- The training objective is typically **mean squared error**, which is smooth and convex for linear models.
- **Gradient descent** updates weights by moving downhill on the error surface.
- Evaluation should include **training vs test error**, and metrics such as **MSE** and {{< katex >}}R^2{{< /katex >}}.
- This “data → model → objective → optimiser” pipeline generalises to deep networks.

## Reference
- **DNN Module #3 — Linear Neural Networks for Regression**.
- Zhang, Lipton, Li, Smola — *Dive into Deep Learning* (sections on linear regression).
- Goodfellow, Bengio, Courville — *Deep Learning* (optimisation and regression basics).

- [Why Square the Error?](https://medium.com/@mail.alireza.a/why-square-the-error-in-machine-learning-153067c1ef64)
- [Variations of GDA](https://www.geeksforgeeks.org/machine-learning/difference-between-batch-gradient-descent-and-stochastic-gradient-descent/)

---

{{< home-link "Home" >}} | {{< section-index >}}
