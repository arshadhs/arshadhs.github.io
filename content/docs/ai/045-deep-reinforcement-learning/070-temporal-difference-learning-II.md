---
title: "Temporal-Difference Learning II and DRL Taxonomy"
draft: false
tags: ["AI", "ML", "Reinforcement Learning", "Temporal-Difference Learning", "DRL Taxonomy"]
categories: ["AI", "ML"]
weight: 700
menu: main
---

# Temporal-Difference Learning II and DRL Taxonomy

One-step TD methods bootstrap after a single transition. Multi-step methods instead use several observed rewards before bootstrapping, forming a bridge between TD(0) and Monte Carlo learning.

**Course Content covered in this module:**

- Temporal-Difference Learning: n-step returns and TD(λ)
- Classification of Reinforcement Learning approaches, algorithms and applications:
  - Model-Based versus Model-Free
  - Value-Based versus Policy-Based
  - On-Policy versus Off-Policy

{{% colour "blue" %}}**The number of sampled steps controls how far learning looks ahead before it bootstraps.**{{% /colour %}}

---

## Learning Objectives

By the end of this module, you should be able to:

- calculate one-step, two-step and n-step returns;
- apply the n-step TD prediction update;
- explain the bias-variance trade-off created by the choice of {{< katex >}} n {{< /katex >}};
- explain how TD(λ) combines returns of different lengths;
- describe the purpose of eligibility traces; and
- classify reinforcement learning algorithms along three independent dimensions.

---

## 1. From One-Step TD to Multi-Step TD ☆

TD(0) uses one observed reward and then bootstraps from the estimated value of the next state:

{{% colour "blue" %}}
{{< katex display=true >}}
G_{t:t+1}
=
R_{t+1}
+
\gamma V(S_{t+1})
{{< /katex >}}
{{% /colour %}}

Monte Carlo learning waits until the episode ends and uses the complete observed return. An n-step method lies between these two extremes:

- it observes the next {{< katex >}} n {{< /katex >}} rewards;
- it then bootstraps from the estimated value of the state reached after those steps.

{{< mermaid >}}
flowchart TD
    A["TD(0): one reward"] --> B["n-step TD: several rewards"]
    B --> C["Monte Carlo: complete episode"]

    style A fill:#E1F5FE,stroke:#1E88E5
    style B fill:#BBDEFB,stroke:#1E88E5
    style C fill:#90CAF9,stroke:#1E88E5
{{< /mermaid >}}

---

## 2. The n-Step Return ☆

The n-step return from time {{< katex >}} t {{< /katex >}} is:

{{% colour "blue" %}}
{{< katex display=true >}}
G_{t:t+n}
=
R_{t+1}
+
\gamma R_{t+2}
+
\cdots
+
\gamma^{n-1}R_{t+n}
+
\gamma^n V(S_{t+n})
{{< /katex >}}
{{% /colour %}}

The final term is the bootstrap estimate. If the episode terminates before step {{< katex >}} t+n {{< /katex >}}, there is no successor value beyond the terminal state, so the remaining bootstrap term is zero.

### Special Cases

For {{< katex >}} n=1 {{< /katex >}}:

{{% colour "blue" %}}
{{< katex display=true >}}
G_{t:t+1}
=
R_{t+1}
+
\gamma V(S_{t+1})
{{< /katex >}}
{{% /colour %}}

This is the ordinary TD(0) target.

For {{< katex >}} n=2 {{< /katex >}}:

{{% colour "blue" %}}
{{< katex display=true >}}
G_{t:t+2}
=
R_{t+1}
+
\gamma R_{t+2}
+
\gamma^2V(S_{t+2})
{{< /katex >}}
{{% /colour %}}

As {{< katex >}} n {{< /katex >}} reaches the remaining episode length, the target becomes the Monte Carlo return because no estimated successor value is needed.

---

## 3. n-Step TD Prediction ☆

After the n-step return becomes available, the value of the starting state is updated by:

{{% colour "blue" %}}
{{< katex display=true >}}
V(S_t)
\leftarrow
V(S_t)
+
\alpha
\left[
G_{t:t+n}-V(S_t)
\right]
{{< /katex >}}
{{% /colour %}}

The n-step TD error is therefore:

{{% colour "blue" %}}
{{< katex display=true >}}
\delta_t^{(n)}
=
G_{t:t+n}-V(S_t)
{{< /katex >}}
{{% /colour %}}

Unlike TD(0), the update cannot be made immediately after one transition. It becomes available after {{< katex >}} n {{< /katex >}} rewards have been observed, or when the episode terminates.

### Simplified Algorithm

```text
Initialise V(s)

For each episode:
    generate experience one transition at a time
    after n rewards are available:
        calculate the n-step return
        update the state visited n steps earlier
    continue until all remaining states have been updated
```

---

## 4. Worked n-Step Example ☆

Suppose:

- {{< katex >}} n=3 {{< /katex >}};
- rewards are {{< katex >}} 2, 0, 4 {{< /katex >}};
- {{< katex >}} \gamma=0.9 {{< /katex >}};
- {{< katex >}} V(S_{t+3})=5 {{< /katex >}};
- {{< katex >}} V(S_t)=3 {{< /katex >}};
- {{< katex >}} \alpha=0.1 {{< /katex >}}.

First calculate the three-step return:

{{% colour "blue" %}}
{{< katex display=true >}}
\begin{aligned}
G_{t:t+3}
&=
2+0.9(0)+0.9^2(4)+0.9^3(5)\\
&=
2+0+3.24+3.645\\
&=
8.885
\end{aligned}
{{< /katex >}}
{{% /colour %}}

Then update the value:

{{% colour "blue" %}}
{{< katex display=true >}}
\begin{aligned}
V(S_t)
&\leftarrow
3+0.1(8.885-3)\\
&=
3.5885
\end{aligned}
{{< /katex >}}
{{% /colour %}}

---

## 5. Choosing n: Bias and Variance ☆

The choice of {{< katex >}} n {{< /katex >}} determines how much the target depends on current estimates and how much it depends on sampled rewards.

| Choice | Bootstrapping | Typical bias | Typical variance | Update delay |
|---|---:|---:|---:|---:|
| Small {{< katex >}} n {{< /katex >}} | More | Higher | Lower | Shorter |
| Large {{< katex >}} n {{< /katex >}} | Less | Lower | Higher | Longer |
| Full return | None | Lower | Highest | Until termination |

There is no universally best value of {{< katex >}} n {{< /katex >}}. It depends on the task, reward noise, quality of current estimates and acceptable update delay.

---

## 6. TD(λ): Combining Different Step Lengths ☆

Choosing one fixed value of {{< katex >}} n {{< /katex >}} can be restrictive. TD(λ) combines one-step, two-step, three-step and longer returns using geometrically decreasing weights.

For a continuing formulation, the λ-return is:

{{% colour "blue" %}}
{{< katex display=true >}}
G_t^{\lambda}
=
(1-\lambda)
\sum_{n=1}^{\infty}
\lambda^{n-1}G_{t:t+n}
{{< /katex >}}
{{% /colour %}}

The value update is:

{{% colour "blue" %}}
{{< katex display=true >}}
V(S_t)
\leftarrow
V(S_t)
+
\alpha
\left[
G_t^{\lambda}-V(S_t)
\right]
{{< /katex >}}
{{% /colour %}}

### Meaning of λ

- {{< katex >}} \lambda=0 {{< /katex >}} gives the one-step TD target.
- Intermediate values blend short and long returns.
- {{< katex >}} \lambda \rightarrow 1 {{< /katex >}} gives increasing weight to longer returns and approaches Monte Carlo learning in episodic tasks.

{{% hint info %}}
The discount factor {{< katex >}} \gamma {{< /katex >}} controls the importance of rewards over time. The trace-decay parameter {{< katex >}} \lambda {{< /katex >}} controls how strongly learning credit is carried backwards across recently visited states.
{{% /hint %}}

---

## 7. Eligibility Traces ☆

The λ-return is the **forward view** of TD(λ): it considers a weighted mixture of future n-step returns. Eligibility traces provide the corresponding **backward view**, allowing values to be updated online.

The ordinary TD error remains:

{{% colour "blue" %}}
{{< katex display=true >}}
\delta_t
=
R_{t+1}
+
\gamma V(S_{t+1})
-
V(S_t)
{{< /katex >}}
{{% /colour %}}

For an accumulating trace:

{{% colour "blue" %}}
{{< katex display=true >}}
e_t(s)
=
\gamma\lambda e_{t-1}(s)
+
\mathbb{1}\{S_t=s\}
{{< /katex >}}
{{% /colour %}}

Every state is updated according to its current eligibility:

{{% colour "blue" %}}
{{< katex display=true >}}
V(s)
\leftarrow
V(s)
+
\alpha\delta_t e_t(s)
{{< /katex >}}
{{% /colour %}}

A recently visited state has a larger trace and receives more credit or blame. Its trace decays when it is not revisited.

---

## 8. Three Independent Classification Questions ☆

Reinforcement learning algorithms can be classified by asking three different questions:

{{< mermaid >}}
flowchart TD
    A["Reinforcement Learning Algorithm"] --> B["Uses a model?"]
    A --> C["Learns values or a policy?"]
    A --> D["Learns about the behaviour policy?"]

    style A fill:#90CAF9,stroke:#1E88E5
    style B fill:#E1F5FE,stroke:#1E88E5
    style C fill:#E1F5FE,stroke:#1E88E5
    style D fill:#E1F5FE,stroke:#1E88E5
{{< /mermaid >}}

These dimensions are independent. For example, an algorithm can be model-free, value-based and off-policy at the same time.

---

## 9. Model-Based versus Model-Free ☆

| Model-Based | Model-Free |
|---|---|
| Uses or learns environment dynamics | Learns without requiring explicit dynamics |
| Can plan using predicted transitions | Learns directly from sampled experience |
| Can evaluate hypothetical actions through the model | Must obtain useful information from interaction or stored experience |
| Examples: Dynamic Programming, planning with a learned model | Examples: Monte Carlo, SARSA, Q-learning, DQN |

The relevant model is usually represented by transition and reward information such as {{< katex >}} p(s',r\mid s,a) {{< /katex >}}.

{{% hint warning %}}
Model-free does not mean that the agent has no neural network or internal representation. It means that the algorithm does not require an explicit model of environment transitions and rewards for planning.
{{% /hint %}}

---

## 10. Value-Based versus Policy-Based ☆

### Value-Based

A value-based method learns {{< katex >}} V(s) {{< /katex >}} or {{< katex >}} Q(s,a) {{< /katex >}} and derives behaviour from those estimates.

Examples include SARSA, Q-learning and DQN.

### Policy-Based

A policy-based method directly parameterises and improves {{< katex >}} \pi_\theta(a\mid s) {{< /katex >}}. REINFORCE is a standard example.

### Actor-Critic

Actor-Critic methods combine both ideas:

- the **actor** represents the policy;
- the **critic** estimates value and evaluates the actor's decisions.

---

## 11. On-Policy versus Off-Policy ☆

| On-Policy | Off-Policy |
|---|---|
| Learns about the policy generating the behaviour | Learns about a target policy that may differ from the behaviour policy |
| Behaviour policy = target policy | Behaviour policy ≠ target policy is permitted |
| Example: SARSA | Example: Q-learning |

The distinction concerns the relationship between two policies:

- **behaviour policy:** generates experience;
- **target policy:** is evaluated or improved.

An off-policy method can explore using an {{< katex >}} \varepsilon {{< /katex >}}-greedy behaviour policy while learning about a greedy target policy.

---

## 12. Classifying Common Algorithms ☆

| Algorithm | Model use | Main representation | Policy relationship |
|---|---|---|---|
| Dynamic Programming | Model-Based | Value-Based | Planning rather than sampled behaviour |
| On-policy Monte Carlo | Model-Free | Value-Based | On-Policy |
| Off-policy Monte Carlo | Model-Free | Value-Based | Off-Policy |
| TD(0) prediction | Model-Free | State value | Evaluates the supplied policy |
| SARSA | Model-Free | Value-Based | On-Policy |
| Expected SARSA | Model-Free | Value-Based | Usually On-Policy |
| Q-learning | Model-Free | Value-Based | Off-Policy |
| DQN | Model-Free | Value-Based | Off-Policy |
| REINFORCE | Model-Free | Policy-Based | On-Policy |
| Actor-Critic | Usually Model-Free | Value and policy | Depends on the algorithm |

{{% hint info %}}
Do not force every algorithm into one label. The three axes answer different questions, and some algorithm families have both on-policy and off-policy variants.
{{% /hint %}}

---

## Common Mistakes ☆

{{% hint warning %}}
- n-step TD does not sum only rewards; unless termination occurs, it also includes a discounted bootstrap value.
- The exponent on the bootstrap term is {{< katex >}} n {{< /katex >}}, while the final sampled reward uses {{< katex >}} \gamma^{n-1} {{< /katex >}}.
- A larger {{< katex >}} n {{< /katex >}} does not automatically mean better learning; it changes bias, variance and update delay.
- {{< katex >}} \gamma {{< /katex >}} and {{< katex >}} \lambda {{< /katex >}} have different roles.
- Model-Free is not the same as Value-Based.
- On-Policy and Policy-Based are different classifications.
{{% /hint %}}

---

## Practice Questions

1. Write the three-step return and identify its bootstrap term.
2. Calculate an n-step TD update for a supplied reward sequence.
3. Explain what happens as {{< katex >}} n {{< /katex >}} grows to the remaining episode length.
4. Compare the bias and variance of small and large values of {{< katex >}} n {{< /katex >}}.
5. Explain the meanings of {{< katex >}} \lambda=0 {{< /katex >}} and {{< katex >}} \lambda\rightarrow1 {{< /katex >}}.
6. What information is stored by an eligibility trace?
7. Classify SARSA and Q-learning along the three taxonomy dimensions.
8. Why are Value-Based and On-Policy not opposite categories?

---

## Key Takeaways ☆

{{% hint success %}}
- n-step TD observes several rewards before bootstrapping.
- TD(0) and Monte Carlo are the two ends of the multi-step spectrum.
- Small {{< katex >}} n {{< /katex >}} usually means more bias and less variance; large {{< katex >}} n {{< /katex >}} usually means less bias and more variance.
- TD(λ) combines returns of different lengths.
- Eligibility traces distribute each TD error across recently visited states.
- Model-Based/Model-Free, Value-Based/Policy-Based and On-Policy/Off-Policy are separate classification axes.
{{% /hint %}}

---

## Checklist

- [ ] I can calculate an n-step return.
- [ ] I can apply the n-step TD update.
- [ ] I can explain the bias-variance effect of changing {{< katex >}} n {{< /katex >}}.
- [ ] I can explain the meaning of {{< katex >}} \lambda {{< /katex >}}.
- [ ] I can describe eligibility traces.
- [ ] I can distinguish the three DRL taxonomy axes.
- [ ] I can classify common reinforcement learning algorithms.

---

## References

1. Sutton and Barto, *Reinforcement Learning: An Introduction*, Chapters 7 and 12.
2. Official Course Content for Temporal-Difference Learning II and DRL taxonomy.
3. Supplied material introducing n-step TD prediction.

---
{{< home-link "Home" >}} | {{< section-index >}}
