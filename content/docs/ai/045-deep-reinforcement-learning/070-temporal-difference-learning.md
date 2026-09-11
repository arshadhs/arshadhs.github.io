---
title: "Temporal-Difference Learning"
draft: false
tags: ["AI", "ML", "Reinforcement Learning", "Temporal-Difference Learning"]
categories: ["AI", "ML"]
weight: 700
menu: main
---

# Temporal-Difference Learning

Temporal-Difference (TD) learning updates predictions from one transition at a time. It learns directly from experience like Monte Carlo methods, but it does not need to wait until the episode ends.

{{% colour "blue" %}}**TD learning updates an estimate using an immediate reward and another current estimate.**{{% /colour %}}

---

## 1. From Monte Carlo to Temporal Difference ☆

Monte Carlo learning uses the complete observed return:

{{% colour "blue" %}}
{{< katex display=true >}}
\text{MC target}=G_t
{{< /katex >}}
{{% /colour %}}

TD learning uses a one-step target:

{{% colour "blue" %}}
{{< katex display=true >}}
\text{TD target}
=
R_{t+1}+\gamma V(S_{t+1})
{{< /katex >}}
{{% /colour %}}

The next-state value is an estimate rather than a complete return. Updating one estimate from another estimate is called **bootstrapping**.

| Method | Learns from experience? | Needs complete episode? | Bootstraps? |
|---|---:|---:|---:|
| Dynamic Programming | No sampled experience required | No | Yes |
| Monte Carlo | Yes | Yes | No |
| Temporal Difference | Yes | No | Yes |

{{% hint info %}}
TD combines a useful property of each earlier approach: it learns from sampled experience like Monte Carlo, while updating from an estimated successor value like Dynamic Programming.
{{% /hint %}}

---

## 2. The General Update Pattern ☆

Many reinforcement learning algorithms follow:

{{% colour "blue" %}}
{{< katex display=true >}}
\text{New estimate}
=
\text{Old estimate}
+
\alpha\left[
\text{Target}-\text{Old estimate}
\right]
{{< /katex >}}
{{% /colour %}}

The quantity in brackets is the prediction error. A positive error raises the estimate; a negative error lowers it.

- {{< katex >}} \alpha {{< /katex >}} controls how strongly new information changes the estimate.
- {{< katex >}} \gamma {{< /katex >}} controls the importance of future rewards.

---

## 3. TD Error ☆

For state-value prediction, the TD error is:

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

It compares what was predicted for the current state with a revised one-step prediction after observing the reward and next state.

{{% hint success %}}
The TD error is the surprise in the transition: **reward plus discounted next value minus current value**.
{{% /hint %}}

---

## 4. TD(0) Prediction ☆

TD(0) evaluates a policy by updating the current state's value after every transition:

{{% colour "blue" %}}
{{< katex display=true >}}
V(S_t)
\leftarrow
V(S_t)
+
\alpha
\left[
R_{t+1}+\gamma V(S_{t+1})-V(S_t)
\right]
{{< /katex >}}
{{% /colour %}}

It is called TD(0) because it uses a one-step target rather than waiting for additional sampled rewards.

### Worked Example

Suppose:

- {{< katex >}} V(S_t)=5 {{< /katex >}};
- {{< katex >}} R_{t+1}=2 {{< /katex >}};
- {{< katex >}} V(S_{t+1})=6 {{< /katex >}};
- {{< katex >}} \gamma=0.9 {{< /katex >}};
- {{< katex >}} \alpha=0.1 {{< /katex >}}.

The TD target is:

{{% colour "blue" %}}
{{< katex display=true >}}
2+0.9(6)=7.4
{{< /katex >}}
{{% /colour %}}

The TD error is:

{{% colour "blue" %}}
{{< katex display=true >}}
\delta_t=7.4-5=2.4
{{< /katex >}}
{{% /colour %}}

The updated value is:

{{% colour "blue" %}}
{{< katex display=true >}}
V(S_t)
\leftarrow
5+0.1(2.4)
=
5.24
{{< /katex >}}
{{% /colour %}}

---

## 5. Prediction versus Control

TD(0) predicts state values under a given policy. To improve behaviour, the agent usually learns action values {{< katex >}} Q(s,a) {{< /katex >}}.

The main one-step TD control methods differ in how they choose the next value used in the target:

- SARSA uses the action actually selected next.
- Expected SARSA averages over actions under the policy.
- Q-learning uses the highest next-action value.

---

## 6. SARSA: On-Policy TD Control ☆

SARSA is named after the five items in a transition:

{{% colour "blue" %}}
{{< katex display=true >}}
S_t,\ A_t,\ R_{t+1},\ S_{t+1},\ A_{t+1}
{{< /katex >}}
{{% /colour %}}

Its update is:

{{% colour "blue" %}}
{{< katex display=true >}}
Q(S_t,A_t)
\leftarrow
Q(S_t,A_t)
+
\alpha
\left[
R_{t+1}
+
\gamma Q(S_{t+1},A_{t+1})
-
Q(S_t,A_t)
\right]
{{< /katex >}}
{{% /colour %}}

The next action {{< katex >}} A_{t+1} {{< /katex >}} is selected by the same policy used to generate behaviour. SARSA is therefore **on-policy**.

### Simplified SARSA Algorithm

```text
Initialise Q(s,a)
Choose an action A using an epsilon-greedy policy

For each transition:
    take action A
    observe reward R and next state S'
    choose next action A' using the same policy
    update Q(S,A) using Q(S',A')
    set S = S' and A = A'
```

Because SARSA learns about its exploratory behaviour, the possibility of taking a non-greedy action is reflected in its values.

---

## 7. Expected SARSA ☆

Expected SARSA replaces the sampled next action with the expected action value under the policy:

{{% colour "blue" %}}
{{< katex display=true >}}
Q(S_t,A_t)
\leftarrow
Q(S_t,A_t)
+
\alpha
\left[
R_{t+1}
+
\gamma
\sum_a \pi(a\mid S_{t+1})Q(S_{t+1},a)
-
Q(S_t,A_t)
\right]
{{< /katex >}}
{{% /colour %}}

Instead of depending on one randomly selected next action, it averages over all possible next actions using their policy probabilities. This generally reduces variance.

---

## 8. Q-Learning: Off-Policy TD Control ☆

Q-learning uses the maximum estimated value in the next state:

{{% colour "blue" %}}
{{< katex display=true >}}
Q(S_t,A_t)
\leftarrow
Q(S_t,A_t)
+
\alpha
\left[
R_{t+1}
+
\gamma\max_a Q(S_{t+1},a)
-
Q(S_t,A_t)
\right]
{{< /katex >}}
{{% /colour %}}

The behaviour policy may remain {{< katex >}} \varepsilon {{< /katex >}}-greedy, but the target assumes the greedy next action. Q-learning is therefore **off-policy**.

### Simplified Q-Learning Algorithm

```text
Initialise Q(s,a)

For each transition:
    choose A from S using an epsilon-greedy behaviour policy
    take A and observe R and S'
    update Q(S,A) using the maximum Q-value in S'
    set S = S'
```

Q-learning is the tabular foundation of Deep Q-Learning, where a neural network replaces the Q-table.

---

## 9. One Transition, Three Different Targets ☆

Suppose the next state has three actions:

| Action | Q-value | Policy probability |
|---|---:|---:|
| Left | 4 | 0.1 |
| Right | 8 | 0.8 |
| Wait | 2 | 0.1 |

Let {{< katex >}} R_{t+1}=1 {{< /katex >}} and {{< katex >}} \gamma=0.9 {{< /katex >}}.

### SARSA

If the policy actually selects Left:

{{% colour "blue" %}}
{{< katex display=true >}}
\text{Target}
=
1+0.9(4)
=
4.6
{{< /katex >}}
{{% /colour %}}

### Q-Learning

The maximum next-action value is {{< katex >}} 8 {{< /katex >}}:

{{% colour "blue" %}}
{{< katex display=true >}}
\text{Target}
=
1+0.9(8)
=
8.2
{{< /katex >}}
{{% /colour %}}

### Expected SARSA

The expected next value is:

{{% colour "blue" %}}
{{< katex display=true >}}
0.1(4)+0.8(8)+0.1(2)=7
{{< /katex >}}
{{% /colour %}}

Therefore:

{{% colour "blue" %}}
{{< katex display=true >}}
\text{Target}
=
1+0.9(7)
=
7.3
{{< /katex >}}
{{% /colour %}}

The algorithms observe the same transition but learn from different assumptions about what happens next.

---

## 10. SARSA versus Q-Learning versus Expected SARSA ☆

| Method | Next value in target | Policy relationship | Main interpretation |
|---|---|---|---|
| SARSA | {{< katex >}} Q(S_{t+1},A_{t+1}) {{< /katex >}} | On-policy | Learn from the next action actually selected |
| Expected SARSA | {{< katex >}} \sum_a\pi(a\mid S_{t+1})Q(S_{t+1},a) {{< /katex >}} | Usually on-policy | Average over actions the policy may select |
| Q-learning | {{< katex >}} \max_aQ(S_{t+1},a) {{< /katex >}} | Off-policy | Learn about the greedy target policy |

### Memory Line

{{% hint success %}}
- **SARSA:** what action will I actually take?
- **Expected SARSA:** what is the average value under my policy?
- **Q-learning:** what is the best action I could take?
{{% /hint %}}

---

## 11. Terminal States

If {{< katex >}} S_{t+1} {{< /katex >}} is terminal, it has no future return. Its successor value is treated as zero.

The target becomes:

{{% colour "blue" %}}
{{< katex display=true >}}
\text{Target}=R_{t+1}
{{< /katex >}}
{{% /colour %}}

This applies to TD(0), SARSA, Expected SARSA and Q-learning.

---

## 12. Advantages and Limitations of TD Learning

### Advantages

- learns without a transition model;
- updates after each transition;
- can learn during continuing tasks;
- usually learns earlier than methods that wait for complete returns;
- forms the basis of many deep reinforcement learning algorithms.

### Limitations

- bootstrapping can introduce bias;
- learning depends on step-size and exploration choices;
- correlated experience can destabilise learning with nonlinear function approximation;
- off-policy learning with function approximation requires particular care.

---

## Common Mistakes ☆

{{% hint warning %}}
- TD(0) does not wait for the complete return; it uses the next state's current estimate.
- SARSA is on-policy because its target uses the next action selected by the behaviour policy.
- Q-learning may behave epsilon-greedily while learning about a greedy target policy.
- The max in Q-learning is taken over next-state actions, not over possible rewards.
- Expected SARSA uses a probability-weighted expectation, not a simple unweighted average.
{{% /hint %}}

---

## Practice Questions

1. What does bootstrapping mean in TD learning?
2. Calculate a TD(0) update for a given reward and pair of state values.
3. Why is SARSA classified as on-policy?
4. Why is Q-learning classified as off-policy?
5. Compare the targets used by SARSA, Expected SARSA and Q-learning.
6. What happens to the TD target when the next state is terminal?
7. Explain how TD learning combines ideas from Dynamic Programming and Monte Carlo.

---

## Key Takeaways ☆

{{% hint success %}}
- TD learning updates from the immediate reward and an estimated successor value.
- TD(0) predicts state values one transition at a time.
- SARSA uses the next action actually selected and is on-policy.
- Expected SARSA averages next-action values under the policy.
- Q-learning uses the maximum next-action value and is off-policy.
- Q-learning provides the tabular foundation for Deep Q-Networks.
{{% /hint %}}

---

## Checklist

- [ ] I can explain the TD target and TD error.
- [ ] I can perform a TD(0) value update.
- [ ] I can write the SARSA and Q-learning updates.
- [ ] I can distinguish on-policy from off-policy TD control.
- [ ] I can calculate an Expected SARSA target.
- [ ] I can handle terminal states correctly.

---

## References

1. Sutton and Barto, *Reinforcement Learning: An Introduction*, Chapters 6 and 7.
2. Supplied material on TD(0), SARSA, Expected SARSA and Q-learning.

---
{{< home-link "Home" >}} | {{< section-index >}}
