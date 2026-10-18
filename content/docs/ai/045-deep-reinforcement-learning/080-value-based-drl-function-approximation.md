---
title: "Value-Based DRL: Function Approximation"
draft: false
tags: ["AI", "ML", "Reinforcement Learning", "Function Approximation", "Semi-Gradient TD", "Tile Coding"]
categories: ["AI", "ML"]
weight: 800
menu: main
---

# Value-Based DRL: Function Approximation

Tabular methods store a separate value for every state or state-action pair. This becomes impractical when the state space is very large, continuous or represented by raw observations. Function approximation replaces this table with a parameterised function that can **learn from observed states, generalise to related states and predict values for states not seen during training**.

**Course Content covered in this module:**

- Function approximation
- Feature Construction for Linear Methods:
  - Tile Coding
  - Asymmetric Tile Coding
- Linear function approximation
- Semi-Gradient TD methods
- Off-policy function approximation and TD divergence
- TD-Gammon

{{% colour "blue" %}}**Instead of memorising one value per state, the agent learns parameters shared across many states.**{{% /colour %}}

---

## Learning Objectives

By the end of this module, you should be able to:

- explain why tabular value functions fail to scale;
- distinguish a state, its feature vector and the approximator's parameters;
- derive gradient Monte Carlo and semi-gradient TD(0) updates;
- apply linear value-function approximation to a numerical example;
- explain coarse coding, tile coding and asymmetric tilings;
- identify the source of the term **semi-gradient**;
- explain why off-policy TD with function approximation may diverge; and
- connect classical function approximation to deep value-based methods.

---

## 1. Why Tabular Learning Is Not Enough ☆

In a table, learning about one state changes only that state's entry. A nearby or perceptually similar state receives no benefit unless it is visited and updated separately.

This causes two practical problems:

- **coverage:** there may be too many states to visit during training;
- **storage:** there may be too many values to hold in memory.

For example, two Pac-Man screens may differ by only one grid position. A tabular agent treats them as unrelated keys, whereas an approximator can represent their common structure through shared features and weights.

{{< mermaid >}}
flowchart TD
    A["Observed state"] --> B["Feature representation"]
    B --> C["Shared parameters"]
    C --> D["Estimated value"]
    C --> E["Generalisation to similar states"]

    style A fill:#E1F5FE,stroke:#1E88E5
    style B fill:#BBDEFB,stroke:#1E88E5
    style C fill:#90CAF9,stroke:#1E88E5
    style D fill:#E1F5FE,stroke:#1E88E5
    style E fill:#E1F5FE,stroke:#1E88E5
{{< /mermaid >}}

{{% hint info %}}
More training does not change the basic limitation of a table. It may fill more entries, but it still does not naturally share information between related states.
{{% /hint %}}

---

## 2. Parameterised Value Functions ☆

Let a differentiable function approximate the state value:

{{% colour "blue" %}}
{{< katex display=true >}}
\hat{v}(s,\mathbf{w}) \approx v_\pi(s)
{{< /katex >}}
{{% /colour %}}

Here:

- {{< katex >}}s{{< /katex >}} is the current state;
- {{< katex >}}\mathbf{x}(s){{< /katex >}} is a feature vector describing that state;
- {{< katex >}}\mathbf{w}{{< /katex >}} is the vector of learnable parameters; and
- {{< katex >}}\hat v(s,\mathbf w){{< /katex >}} is the estimated value.

The same parameter vector is used for every state. Consequently, changing one weight can change the predictions of many states.

An action-value approximator similarly uses both state and action:

{{% colour "blue" %}}
{{< katex display=true >}}
\hat{q}(s,a,\mathbf{w}) \approx q_\pi(s,a)
{{< /katex >}}
{{% /colour %}}

State-value approximation supports **prediction**. Action-value approximation is normally required for model-free **control**, because the agent must compare the values of available actions.

---

## 3. The Prediction Objective

A common objective is the mean-squared value error under a state distribution {{< katex >}}\mu{{< /katex >}}:

{{% colour "blue" %}}
{{< katex display=true >}}
\overline{VE}(\mathbf{w})
=
\sum_{s}\mu(s)
\left[v_\pi(s)-\hat v(s,\mathbf{w})\right]^2
{{< /katex >}}
{{% /colour %}}

The distribution {{< katex >}}\mu(s){{< /katex >}} determines which states matter most. Frequently visited states receive more weight when {{< katex >}}\mu{{< /katex >}} is the on-policy state distribution.

The true value {{< katex >}}v_\pi(s){{< /katex >}} is usually unknown. Learning therefore uses a sampled target {{< katex >}}U_t{{< /katex >}} in its place.

For a differentiable approximator, the general stochastic-gradient update is:

{{% colour "blue" %}}
{{< katex display=true >}}
\mathbf{w}_{t+1}
=
\mathbf{w}_t
+
\alpha
\left[U_t-\hat v(S_t,\mathbf{w}_t)\right]
\nabla_{\mathbf{w}}\hat v(S_t,\mathbf{w}_t)
{{< /katex >}}
{{% /colour %}}

It has three factors:

| Factor | Meaning |
|---|---|
| {{< katex >}}\alpha{{< /katex >}} | Step size |
| {{< katex >}}U_t-\hat v(S_t,\mathbf w_t){{< /katex >}} | Prediction error |
| {{< katex >}}\nabla_{\mathbf w}\hat v(S_t,\mathbf w_t){{< /katex >}} | Direction in which the parameters change the prediction |

---

## 4. Gradient Monte Carlo ☆

Monte Carlo prediction uses the complete sampled return as its target:

{{% colour "blue" %}}
{{< katex display=true >}}
U_t=G_t
{{< /katex >}}
{{% /colour %}}

The update becomes:

{{% colour "blue" %}}
{{< katex display=true >}}
\mathbf{w}_{t+1}
=
\mathbf{w}_t
+
\alpha
\left[G_t-\hat v(S_t,\mathbf{w}_t)\right]
\nabla_{\mathbf{w}}\hat v(S_t,\mathbf{w}_t)
{{< /katex >}}
{{% /colour %}}

Because {{< katex >}}G_t{{< /katex >}} is an unbiased sample of {{< katex >}}v_\pi(S_t){{< /katex >}} under the policy, this is a true stochastic-gradient method for the squared prediction objective. Its familiar Monte Carlo limitation remains: the update must wait for the return to become available.

---

## 5. Linear Function Approximation ☆

In a linear approximator, the estimated value is a weighted sum of features:

{{% colour "blue" %}}
{{< katex display=true >}}
\hat v(s,\mathbf{w})
=
\mathbf{w}^{\mathsf T}\mathbf{x}(s)
=
\sum_{i=1}^{d}w_i x_i(s)
{{< /katex >}}
{{% /colour %}}

Its gradient is especially simple:

{{% colour "blue" %}}
{{< katex display=true >}}
\nabla_{\mathbf w}\hat v(s,\mathbf w)=\mathbf{x}(s)
{{< /katex >}}
{{% /colour %}}

Therefore, the generic update reduces to:

{{% colour "blue" %}}
{{< katex display=true >}}
\mathbf{w}_{t+1}
=
\mathbf{w}_t
+
\alpha
\left[U_t-\mathbf{w}_t^{\mathsf T}\mathbf{x}(S_t)\right]
\mathbf{x}(S_t)
{{< /katex >}}
{{% /colour %}}

Only features active in the current state receive a direct update. However, those weights are shared, so every other state containing the same features can acquire a different prediction.

### Worked Example: Generalisation Through Shared Weights ☆

Suppose:

- {{< katex >}}\mathbf{x}(S_t)=[2,2,1]^{\mathsf T}{{< /katex >}};
- {{< katex >}}\mathbf{w}_t=[0,0,0]^{\mathsf T}{{< /katex >}};
- target {{< katex >}}U_t=10{{< /katex >}}; and
- {{< katex >}}\alpha=0.1{{< /katex >}}.

The current estimate is zero, so:

{{% colour "blue" %}}
{{< katex display=true >}}
\begin{aligned}
\mathbf{w}_{t+1}
&=
\begin{bmatrix}0\\0\\0\end{bmatrix}
+0.1(10-0)
\begin{bmatrix}2\\2\\1\end{bmatrix}\\
&=
\begin{bmatrix}2\\2\\1\end{bmatrix}
\end{aligned}
{{< /katex >}}
{{% /colour %}}

Now consider an unvisited but related state with {{< katex >}}\mathbf{x}(S')=[2,4,1]^{\mathsf T}{{< /katex >}}. Its value can already be predicted:

{{% colour "blue" %}}
{{< katex display=true >}}
\hat v(S',\mathbf w_{t+1})
=
2(2)+2(4)+1(1)
=13
{{< /katex >}}
{{% /colour %}}

The number {{< katex >}}13{{< /katex >}} is not a memorised table entry. It arises because the newly learned parameters are reused for the new feature vector.

---

## 6. Semi-Gradient TD(0) ☆

TD(0) uses a bootstrapped target:

{{% colour "blue" %}}
{{< katex display=true >}}
U_t
=
R_{t+1}
+
\gamma\hat v(S_{t+1},\mathbf{w}_t)
{{< /katex >}}
{{% /colour %}}

Define the approximate TD error:

{{% colour "blue" %}}
{{< katex display=true >}}
\delta_t
=
R_{t+1}
+
\gamma\hat v(S_{t+1},\mathbf{w}_t)
-
\hat v(S_t,\mathbf{w}_t)
{{< /katex >}}
{{% /colour %}}

The semi-gradient TD(0) update is:

{{% colour "blue" %}}
{{< katex display=true >}}
\mathbf{w}_{t+1}
=
\mathbf{w}_t
+
\alpha\delta_t
\nabla_{\mathbf w}\hat v(S_t,\mathbf{w}_t)
{{< /katex >}}
{{% /colour %}}

For a linear approximator:

{{% colour "blue" %}}
{{< katex display=true >}}
\mathbf{w}_{t+1}
=
\mathbf{w}_t
+
\alpha\delta_t\mathbf{x}(S_t)
{{< /katex >}}
{{% /colour %}}

### Why Is It Called a Semi-Gradient?

The TD target itself contains {{< katex >}}\hat v(S_{t+1},\mathbf w_t){{< /katex >}}, which depends on the same parameters being updated. A full derivative of the squared TD error would differentiate through both the current estimate and this target.

Semi-gradient TD deliberately treats the target as if it were constant and differentiates only {{< katex >}}\hat v(S_t,\mathbf w_t){{< /katex >}}. It therefore follows only part of the complete gradient.

{{% hint warning %}}
The word **semi** does not mean that half the data or half the features are used. It means that the derivative through the bootstrapped target is omitted.
{{% /hint %}}

### Why Use It?

Semi-gradient methods remain important because they:

- learn continually and online;
- do not need to wait until an episode ends;
- work with continuing tasks;
- are computationally efficient; and
- converge reliably in important on-policy linear cases under standard assumptions.

They do not possess the same general objective-descent guarantee as gradient Monte Carlo because the bootstrapped target is biased and moves with the parameters.

---

## 7. Semi-Gradient n-Step TD

The n-step return from the previous module can also serve as the target:

{{% colour "blue" %}}
{{< katex display=true >}}
G_{t:t+n}
=
R_{t+1}+\gamma R_{t+2}+\cdots
+\gamma^{n-1}R_{t+n}
+\gamma^n\hat v(S_{t+n},\mathbf w)
{{< /katex >}}
{{% /colour %}}

The parameter update is:

{{% colour "blue" %}}
{{< katex display=true >}}
\mathbf{w}_{t+n}
=
\mathbf{w}_{t+n-1}
+
\alpha
\left[G_{t:t+n}-\hat v(S_t,\mathbf w_{t+n-1})\right]
\nabla_{\mathbf w}\hat v(S_t,\mathbf w_{t+n-1})
{{< /katex >}}
{{% /colour %}}

The conceptual change from tabular n-step TD is small: update a **weight vector** instead of one table entry.

---

## 8. Feature Construction ☆

The approximator can generalise only through the representation supplied to it. Good features make relevant states similar; poor features may merge states that require different values or separate states that should share information.

### Coarse Coding

Coarse coding places overlapping receptive fields over a continuous state space.

- A feature is {{< katex >}}1{{< /katex >}} when the state lies inside its receptive field.
- It is {{< katex >}}0{{< /katex >}} otherwise.
- A single state activates several overlapping features.
- Updating that state changes all corresponding weights.

Nearby states share some active regions, which produces smooth generalisation.

### Tile Coding

Tile coding overlays several offset grids called **tilings**. Each state activates one tile in each tiling.

{{< mermaid >}}
flowchart TD
    A["Continuous state"] --> B["Several offset tilings"]
    B --> C["One active tile per tiling"]
    C --> D["Sparse binary feature vector"]
    D --> E["Linear value estimate"]

    style A fill:#E1F5FE,stroke:#1E88E5
    style B fill:#BBDEFB,stroke:#1E88E5
    style C fill:#90CAF9,stroke:#1E88E5
    style D fill:#BBDEFB,stroke:#1E88E5
    style E fill:#E1F5FE,stroke:#1E88E5
{{< /mermaid >}}

The representation is sparse, quick to compute and capable of local generalisation. The number and width of the tilings control resolution and generalisation.

### Asymmetric Tile Coding

Ordinary symmetric tiles generalise by similar amounts in each dimension. Asymmetric tilings use different shapes, widths or offsets across dimensions when the task requires different resolutions.

For example, a control problem may require very fine resolution for velocity but coarser resolution for position. Asymmetric tiles encode that preference directly.

### Other Feature Constructions

Radial basis functions, Fourier bases and polynomial features provide other fixed representations. Their shared limitation is that feature design may require domain knowledge. Deep neural networks reduce this dependence by learning representations jointly with the value estimate.

---

## 9. Off-Policy Function Approximation and TD Divergence ☆

Function approximation is not automatically stable. A particularly dangerous combination contains three elements:

{{< mermaid >}}
flowchart TD
    A["Function approximation"] --> D["Deadly triad"]
    B["Bootstrapping"] --> D
    C["Off-policy learning"] --> D
    D --> E["Possible instability or divergence"]

    style A fill:#E1F5FE,stroke:#1E88E5
    style B fill:#E1F5FE,stroke:#1E88E5
    style C fill:#E1F5FE,stroke:#1E88E5
    style D fill:#90CAF9,stroke:#1E88E5
    style E fill:#FFCDD2,stroke:#D32F2F
{{< /mermaid >}}

- **Function approximation** couples the values of different states through shared parameters.
- **Bootstrapping** updates an estimate towards another estimate.
- **Off-policy learning** learns about a target policy from data generated by a different behaviour policy.

Together, the update can move one estimate, alter many other estimates and repeatedly chase changing targets under a distribution different from the target policy's distribution. In some problems, the weights grow rather than converge—even with a linear approximator.

{{% hint warning %}}
Divergence is a possibility, not an assertion that every off-policy approximation method fails. Stability depends on the algorithm, representation, sampling distribution and step size.
{{% /hint %}}

---

## 10. TD-Gammon: Why Function Approximation Matters

TD-Gammon used a neural network and temporal-difference learning to estimate positions in backgammon. It illustrated two central ideas:

- the value function can be represented by a nonlinear approximator instead of a table; and
- self-play can produce training experience without labelled optimal moves.

Its importance is conceptual: value approximation allows TD learning to operate in a state space where enumerating and learning every position separately is infeasible.

---

## 11. From Linear Approximation to Deep Q-Learning

A neural network follows the same broad pattern as a linear approximator:

1. receive a state representation;
2. produce an estimated value or action values;
3. compare the estimate with a learning target;
4. back-propagate the error to update shared parameters.

The difference is representational power. A linear method combines supplied features, while a deep network can learn intermediate features from inputs such as image pixels.

For action selection, a value-based agent approximates {{< katex >}}q(s,a){{< /katex >}} and can act greedily or {{< katex >}}\varepsilon{{< /katex >}}-greedily. Deep Q-learning develops this idea further, but its architecture, replay memory, target network and stability mechanisms belong to the next module.

---

## Gradient Monte Carlo versus Semi-Gradient TD

| Property | Gradient Monte Carlo | Semi-Gradient TD(0) |
|---|---|---|
| Target | Complete return {{< katex >}}G_t{{< /katex >}} | {{< katex >}}R_{t+1}+\gamma\hat v(S_{t+1},\mathbf w){{< /katex >}} |
| Bootstraps? | No | Yes |
| Update timing | After return is available | After each transition |
| Target depends on current weights? | No | Yes |
| Gradient description | True stochastic gradient | Semi-gradient |
| Suitable for continuing tasks? | Not naturally | Yes |

---

## Common Mistakes ☆

{{% hint warning %}}
- The feature vector {{< katex >}}\mathbf{x}(s){{< /katex >}} describes a state; the weight vector {{< katex >}}\mathbf w{{< /katex >}} is learned.
- Function approximation does not mean only neural networks; linear approximators are function approximators too.
- A state-value estimate alone does not directly select an action in a model-free control problem.
- Semi-gradient TD does not differentiate through the bootstrapped target.
- Generalisation can help or harm: unrelated states should not be forced to share inappropriate features.
- Linear approximation can still diverge under the deadly triad.
- Tile coding creates features; it is not itself a reinforcement learning control algorithm.
{{% /hint %}}

---

## Practice Questions

1. Why does a tabular value function not generalise to an unseen state?
2. Distinguish {{< katex >}}s{{< /katex >}}, {{< katex >}}\mathbf{x}(s){{< /katex >}} and {{< katex >}}\mathbf w{{< /katex >}}.
3. Derive the gradient Monte Carlo update from a squared-error objective.
4. For a supplied feature vector, target and step size, calculate one linear weight update.
5. Why is TD(0) with function approximation called a semi-gradient method?
6. Compare coarse coding with tile coding.
7. Why might asymmetric tilings be useful when state dimensions have different sensitivities?
8. Name the three parts of the deadly triad and explain their interaction.
9. Why is {{< katex >}}\hat q(s,a,\mathbf w){{< /katex >}} more directly useful than {{< katex >}}\hat v(s,\mathbf w){{< /katex >}} for model-free control?
10. Explain how function approximation creates the bridge from tabular Q-learning to DQN.

---

## Key Takeaways ☆

{{% hint success %}}
- Function approximation replaces independent table entries with shared parameters.
- Shared parameters enable learning, generalisation and prediction across related states.
- Gradient Monte Carlo uses an unbiased complete-return target.
- Semi-gradient TD treats its bootstrapped target as constant during differentiation.
- Linear approximation makes the update direction equal to the active feature vector.
- Coarse coding and tile coding turn continuous states into overlapping features.
- Function approximation, bootstrapping and off-policy learning can produce divergence when combined.
- Deep value-based learning extends the same parameter-update idea with learned nonlinear features.
{{% /hint %}}

---

## Checklist

- [ ] I can explain why tabular methods do not scale.
- [ ] I can write the generic stochastic-gradient value update.
- [ ] I can apply the linear function-approximation update.
- [ ] I can derive and explain semi-gradient TD(0).
- [ ] I can distinguish weights from features.
- [ ] I can explain coarse coding, tile coding and asymmetric tilings.
- [ ] I can identify the deadly triad.
- [ ] I can explain how this material leads towards DQN.

---

## References

1. Sutton and Barto, *Reinforcement Learning: An Introduction*, Second Edition, Chapters 9 and 11.
2. Official Course Content for Value-Based DRL Methods.
3. Supplied material on on-policy prediction with approximation.

---
{{< home-link "Home" >}} | {{< section-index >}}
