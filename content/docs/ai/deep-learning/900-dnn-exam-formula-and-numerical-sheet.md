---
title: "DNN Formula and Numerical Sheet"
draft: false
tags: ["AI", "Deep Learning", "DNN", "Exam", "Formula Sheet", "Numericals"]
categories: ["AI", "Deep Learning"]
weight: 9000
menu: main
---

# DNN Formula and Numerical Sheet

This page consolidates the most useful Deep Neural Networks formulas and numerical patterns for revision.

It is designed for preparation and should be used together with the topic pages.

{{% hint info %}}
**Revision strategy:**  
Do not only memorise formulas.

For each formula, know:

1. what each symbol means
2. when to apply it
3. how to substitute values carefully
4. what the output shape or answer represents
{{% /hint %}}

---

# 1. Artificial Neuron

## Weighted Sum ☆

{{% colour "blue" %}}
{{< katex display=true >}}
z = \sum_{i=1}^{n} w_i x_i + b
{{< /katex >}}
{{% /colour %}}

Vector form:

{{% colour "blue" %}}
{{< katex display=true >}}
z = w^T x + b
{{< /katex >}}
{{% /colour %}}

## Activation

{{% colour "blue" %}}
{{< katex display=true >}}
\hat{y}=f(z)
{{< /katex >}}
{{% /colour %}}

---

# 2. Regression with Single Neuron

## Linear Prediction ☆

{{% colour "blue" %}}
{{< katex display=true >}}
\hat{y}=w^T x
{{< /katex >}}
{{% /colour %}}

## Mean Squared Error

{{% colour "blue" %}}
{{< katex display=true >}}
J(w)=\frac{1}{N}\sum_{i=1}^{N}\frac{1}{2}(w^T x^{(i)}-y^{(i)})^2
{{< /katex >}}
{{% /colour %}}

## Gradient of Squared Error

{{% colour "blue" %}}
{{< katex display=true >}}
\nabla J(w)=\frac{1}{N}X^T(Xw-y)
{{< /katex >}}
{{% /colour %}}

## Gradient Descent Update

{{% colour "blue" %}}
{{< katex display=true >}}
w^{(t+1)}=w^{(t)}-\eta \nabla J(w^{(t)})
{{< /katex >}}
{{% /colour %}}

---

# 3. Binary Classification

## Sigmoid ☆

{{% colour "blue" %}}
{{< katex display=true >}}
\sigma(z)=\frac{1}{1+e^{-z}}
{{< /katex >}}
{{% /colour %}}

## Sigmoid Derivative

{{% colour "blue" %}}
{{< katex display=true >}}
\sigma'(z)=\sigma(z)(1-\sigma(z))
{{< /katex >}}
{{% /colour %}}

## Prediction Rule

{{% colour "blue" %}}
{{< katex display=true >}}
\hat{y} = \begin{cases}1, & \sigma(z) \ge 0.5 \\ 0, & \sigma(z) < 0.5\end{cases}
{{< /katex >}}
{{% /colour %}}

## Binary Cross-Entropy

{{% colour "blue" %}}
{{< katex display=true >}}
\ell(\hat{y},y)= -y\log(\hat{y})-(1-y)\log(1-\hat{y})
{{< /katex >}}
{{% /colour %}}

---

# 4. Multi-Class Classification

## Softmax ☆

{{% colour "blue" %}}
{{< katex display=true >}}
\hat{y}_k = \frac{e^{z_k}}{\sum_{j=1}^{K}e^{z_j}}
{{< /katex >}}
{{% /colour %}}

## Categorical Cross-Entropy

{{% colour "blue" %}}
{{< katex display=true >}}
\ell(\hat{y},y)= -\sum_{k=1}^{K} y_k \log(\hat{y}_k)
{{< /katex >}}
{{% /colour %}}

## Useful Shortcut

For softmax with cross-entropy:

{{% colour "blue" %}}
{{< katex display=true >}}
\frac{\partial \ell}{\partial z_k}=\hat{y}_k-y_k
{{< /katex >}}
{{% /colour %}}

---

# 5. DFNN / MLP Shapes

## Layer-wise Computation ☆

For layer {{< katex >}} l {{< /katex >}}:

{{% colour "blue" %}}
{{< katex display=true >}}
z^{(l)} = h^{(l-1)}W^{(l)} + b^{(l)}
{{< /katex >}}
{{% /colour %}}

{{% colour "blue" %}}
{{< katex display=true >}}
h^{(l)} = \sigma^{(l)}(z^{(l)})
{{< /katex >}}
{{% /colour %}}

## Parameter Count for Dense Layer ☆

If input units are {{< katex >}} n_{in} {{< /katex >}} and output units are {{< katex >}} n_{out} {{< /katex >}}:

{{% colour "blue" %}}
{{< katex display=true >}}
\text{Parameters}=n_{in}n_{out}+n_{out}
{{< /katex >}}
{{% /colour %}}

---

# 6. CNN Output Size

## Convolution Output Size ☆

For input size {{< katex >}} N {{< /katex >}}, kernel size {{< katex >}} K {{< /katex >}}, padding {{< katex >}} P {{< /katex >}}, and stride {{< katex >}} S {{< /katex >}}:

{{% colour "blue" %}}
{{< katex display=true >}}
O = \left\lfloor \frac{N-K+2P}{S} \right\rfloor + 1
{{< /katex >}}
{{% /colour %}}

For height and width separately:

{{% colour "blue" %}}
{{< katex display=true >}}
H_{out}=\left\lfloor \frac{H-K_h+2P_h}{S_h} \right\rfloor + 1
{{< /katex >}}
{{% /colour %}}

{{% colour "blue" %}}
{{< katex display=true >}}
W_{out}=\left\lfloor \frac{W-K_w+2P_w}{S_w} \right\rfloor + 1
{{< /katex >}}
{{% /colour %}}

## Same Padding for Odd Kernel

For stride 1 and odd kernel size:

{{% colour "blue" %}}
{{< katex display=true >}}
P = \frac{K-1}{2}
{{< /katex >}}
{{% /colour %}}

---

# 7. CNN Parameter Count

## Convolution Layer Parameters ☆

For kernel size {{< katex >}} K_h \times K_w {{< /katex >}}, input channels {{< katex >}} C_{in} {{< /katex >}}, and output channels {{< katex >}} C_{out} {{< /katex >}}:

{{% colour "blue" %}}
{{< katex display=true >}}
\text{Parameters}=(K_hK_wC_{in}+1)C_{out}
{{< /katex >}}
{{% /colour %}}

The +1 represents one bias per filter.

## 1 by 1 Convolution Parameters

{{% colour "blue" %}}
{{< katex display=true >}}
\text{Parameters}=(1 \times 1 \times C_{in}+1)C_{out}
{{< /katex >}}
{{% /colour %}}

---

# 8. Pooling Output Size

Pooling uses the same spatial size formula as convolution.

{{% colour "blue" %}}
{{< katex display=true >}}
O = \left\lfloor \frac{N-K+2P}{S} \right\rfloor + 1
{{< /katex >}}
{{% /colour %}}

Pooling usually has no learnable parameters.

---

# 9. Dilated Convolution

## Effective Kernel Size ☆

For kernel size {{< katex >}} K {{< /katex >}} and dilation {{< katex >}} D {{< /katex >}}:

{{% colour "blue" %}}
{{< katex display=true >}}
K_{eff}=K+(K-1)(D-1)
{{< /katex >}}
{{% /colour %}}

Then use {{< katex >}} K_{eff} {{< /katex >}} in the output-size formula.

---

# 10. RNN

## Hidden State Update ☆

{{% colour "blue" %}}
{{< katex display=true >}}
h_t=\phi(W_{hh}h_{t-1}+W_{xh}x_t+b_h)
{{< /katex >}}
{{% /colour %}}

## Output

{{% colour "blue" %}}
{{< katex display=true >}}
o_t=W_{ho}h_t+b_o
{{< /katex >}}
{{% /colour %}}

---

# 11. LSTM / GRU Concepts

For LSTM, remember:

| Gate | Purpose |
|---|---|
| Forget gate | what to remove from memory |
| Input gate | what new information to store |
| Candidate memory | proposed new content |
| Output gate | what to expose as hidden state |

For GRU, remember:

| Gate | Purpose |
|---|---|
| Reset gate | how much past to forget while forming candidate |
| Update gate | how much old hidden state to keep |
| Candidate hidden state | proposed new hidden representation |

---

# 12. Attention

## Attention Weighted Sum ☆

{{% colour "blue" %}}
{{< katex display=true >}}
\text{Attention}(q,K,V)=\sum_{i=1}^{n}\alpha_i v_i
{{< /katex >}}
{{% /colour %}}

## Attention Weights

{{% colour "blue" %}}
{{< katex display=true >}}
\alpha_i = \frac{\exp(\text{score}(q,k_i))}{\sum_{j=1}^{n}\exp(\text{score}(q,k_j))}
{{< /katex >}}
{{% /colour %}}

## Scaled Dot-Product Attention ☆

{{% colour "blue" %}}
{{< katex display=true >}}
\text{Attention}(Q,K,V)=\text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)V
{{< /katex >}}
{{% /colour %}}

---

# 13. Transformer

## Positional Encoding ☆

{{% colour "blue" %}}
{{< katex display=true >}}
PE(pos,2i)=\sin\left(\frac{pos}{10000^{2i/d_{model}}}\right)
{{< /katex >}}
{{% /colour %}}

{{% colour "blue" %}}
{{< katex display=true >}}
PE(pos,2i+1)=\cos\left(\frac{pos}{10000^{2i/d_{model}}}\right)
{{< /katex >}}
{{% /colour %}}

## Transformer Input

{{% colour "blue" %}}
{{< katex display=true >}}
X = \text{Embedding}(tokens) + PE(positions)
{{< /katex >}}
{{% /colour %}}

## Position-wise Feed Forward Network

{{% colour "blue" %}}
{{< katex display=true >}}
FFN(x)=\max(0,xW_1+b_1)W_2+b_2
{{< /katex >}}
{{% /colour %}}

## Add and Norm

{{% colour "blue" %}}
{{< katex display=true >}}
\text{Output}=\text{LayerNorm}(x+\text{Sublayer}(x))
{{< /katex >}}
{{% /colour %}}

---

# 14. Optimisers

## Gradient Descent ☆

{{% colour "blue" %}}
{{< katex display=true >}}
\theta_{t+1}=\theta_t-\eta \nabla_{\theta}\mathcal{L}(\theta_t)
{{< /katex >}}
{{% /colour %}}

## Mini-Batch Gradient

{{% colour "blue" %}}
{{< katex display=true >}}
g=\frac{1}{B}\sum_{i \in B}\nabla_{\theta}\ell_i(\theta)
{{< /katex >}}
{{% /colour %}}

## Momentum

{{% colour "blue" %}}
{{< katex display=true >}}
v_t=\beta v_{t-1}+g_t
{{< /katex >}}
{{% /colour %}}

{{% colour "blue" %}}
{{< katex display=true >}}
\theta_{t+1}=\theta_t-\eta v_t
{{< /katex >}}
{{% /colour %}}

## Adam

{{% colour "blue" %}}
{{< katex display=true >}}
m_t=\beta_1m_{t-1}+(1-\beta_1)g_t
{{< /katex >}}
{{% /colour %}}

{{% colour "blue" %}}
{{< katex display=true >}}
v_t=\beta_2v_{t-1}+(1-\beta_2)g_t^2
{{< /katex >}}
{{% /colour %}}

{{% colour "blue" %}}
{{< katex display=true >}}
\theta_{t+1}=\theta_t-\eta\frac{\hat{m}_t}{\sqrt{\hat{v}_t}+\epsilon}
{{< /katex >}}
{{% /colour %}}

---

# 15. Regularisation

## L2 / Weight Decay ☆

{{% colour "blue" %}}
{{< katex display=true >}}
J_{regularised}(\theta)=J(\theta)+\lambda\|\theta\|_2^2
{{< /katex >}}
{{% /colour %}}

## L1

{{% colour "blue" %}}
{{< katex display=true >}}
J_{regularised}(\theta)=J(\theta)+\lambda\|\theta\|_1
{{< /katex >}}
{{% /colour %}}

## Batch Normalisation

{{% colour "blue" %}}
{{< katex display=true >}}
\hat{x}_i=\frac{x_i-\mu_B}{\sqrt{\sigma_B^2+\epsilon}}
{{< /katex >}}
{{% /colour %}}

{{% colour "blue" %}}
{{< katex display=true >}}
y_i=\gamma\hat{x}_i+\beta
{{< /katex >}}
{{% /colour %}}

## Xavier Initialisation

{{% colour "blue" %}}
{{< katex display=true >}}
W \sim U\left(-\sqrt{\frac{6}{n_{in}+n_{out}}},\sqrt{\frac{6}{n_{in}+n_{out}}}\right)
{{< /katex >}}
{{% /colour %}}

## He Initialisation

{{% colour "blue" %}}
{{< katex display=true >}}
W \sim N\left(0,\frac{2}{n_{in}}\right)
{{< /katex >}}
{{% /colour %}}

---

# 16. Numerical Checklist ☆

## CNN Output Size Questions

Steps:

1. Identify input size {{< katex >}} N {{< /katex >}}
2. Identify kernel size {{< katex >}} K {{< /katex >}}
3. Identify padding {{< katex >}} P {{< /katex >}}
4. Identify stride {{< katex >}} S {{< /katex >}}
5. Substitute into output formula
6. Repeat separately for height and width if needed
7. Output channels equal number of filters

## CNN Parameter Count Questions

Steps:

1. Identify kernel height and width
2. Identify input channels
3. Identify number of filters
4. Add one bias per filter
5. Use {{< katex >}} (K_hK_wC_{in}+1)C_{out} {{< /katex >}}

## Softmax Questions

Steps:

1. Exponentiate each logit
2. Add all exponentials
3. Divide each exponential by the total
4. Choose class with highest probability

## Optimiser Numerical Questions

Steps:

1. Compute gradient
2. Multiply by learning rate
3. Subtract from old parameter
4. For momentum, update velocity first
5. For Adam, update first and second moments

---

# 17. Quick Comparison Table

| Topic | Must remember |
|---|---|
| Regression | identity activation, squared loss |
| Binary classification | sigmoid, binary cross-entropy |
| Multi-class classification | softmax, categorical cross-entropy |
| DFNN | hidden layers create non-linearity |
| CNN | local connectivity, parameter sharing |
| Pooling | spatial reduction, usually no parameters |
| RNN | hidden state stores sequence history |
| LSTM | gates control memory |
| GRU | simpler gated RNN |
| Attention | query-key-value weighted retrieval |
| Transformer | self-attention plus positional encoding |
| Optimisers | update parameters to minimise loss |
| Regularisation | reduce overfitting and improve generalisation |

---

# 18. Final Memory Lines

{{% hint success %}}
**Neuron:** weighted sum → activation → prediction  
**Classification:** score → sigmoid or softmax → probability → loss  
**CNN:** image → convolution → feature map → activation → pooling → classifier  
**RNN:** current input plus previous hidden state → new hidden state  
**Attention:** query compares with keys → softmax weights → weighted values  
**Transformer:** embedding plus position → attention → feed-forward → output  
**Optimiser:** gradient tells direction, learning rate controls step size  
**Regularisation:** reduce memorisation, improve generalisation
{{% /hint %}}

---
{{< home-link "Home" >}} | {{< section-index >}}
