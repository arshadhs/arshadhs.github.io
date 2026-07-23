---
title: "Multi-Armed Bandit Problem"
draft: false
tags: ["AI", "ML", "Reinforcement Learning", "Multi-Armed Bandits"]
categories: ["AI", "ML"]
weight: 200
menu: main
---

# Multi-Armed Bandit Problem

The Multi-Armed Bandit (MAB) problem is the simplest setting for studying **decision-making under uncertainty**.

An agent repeatedly chooses one of {{< katex >}} k {{< /katex >}} actions. Each action produces a numerical reward drawn from an unknown distribution. The objective is to maximise the expected total reward over time.

{{% colour "green" %}}**The central challenge is deciding when to exploit current knowledge and when to explore uncertain alternatives.**{{% /colour %}}

---

## The k-Armed Bandit Problem ☆

At every time step:

1. choose an action from {{< katex >}} k {{< /katex >}} possible actions;
2. receive a numerical reward;
3. update what is known about the selected action;
4. choose again.

Unlike a full Markov Decision Process, a basic non-associative bandit has no changing state representation. The same decision problem is faced repeatedly.

```mermaid
flowchart LR
    C[Choose one of k actions] --> R[Observe reward]
    R --> U[Update action knowledge]
    U --> C

    style C fill:#E1F5FE
    style R fill:#C8E6C9
    style U fill:#FFF9C4
```

---

## True Action Value ☆

The true value of action {{< katex >}} a {{< /katex >}} is its expected reward.

{{% colour "green" %}}
{{< katex display=true >}}
q_*(a) \doteq \mathbb{E}\left[R_t \mid A_t = a\right]
{{< /katex >}}
{{% /colour %}}

Where:

- {{< katex >}} A_t {{< /katex >}} is the action selected at time {{< katex >}} t {{< /katex >}};
- {{< katex >}} R_t {{< /katex >}} is the reward received;
- {{< katex >}} q_*(a) {{< /katex >}} is the unknown true action value.

If all true action values were known, the solution would be simple: always choose the action with the highest value.

In practice, the agent must estimate them.

---

## Estimated Action Value ☆

Let {{< katex >}} Q_t(a) {{< /katex >}} denote the estimated value of action {{< katex >}} a {{< /katex >}} at time {{< katex >}} t {{< /katex >}}.

Using the sample-average method:

{{% colour "green" %}}
{{< katex display=true >}}
Q_t(a) =
\frac{\sum_{i=1}^{t-1} R_i\,\mathbf{1}_{A_i=a}}
{\sum_{i=1}^{t-1} \mathbf{1}_{A_i=a}}
{{< /katex >}}
{{% /colour %}}

This is the average of the rewards observed on previous occasions when action {{< katex >}} a {{< /katex >}} was selected.

---

## Greedy Action Selection ☆

A greedy method chooses the action with the highest current estimate.

{{% colour "green" %}}
{{< katex display=true >}}
A_t = \underset{a}{\operatorname{arg\,max}}\;Q_t(a)
{{< /katex >}}
{{% /colour %}}

### Advantage

- It exploits the best action currently known.

### Limitation

- Early estimates may be inaccurate.
- An apparently poor action may actually be optimal.
- A greedy method may never gather the evidence needed to correct its mistake.

---

## Exploration versus Exploitation ☆

| Strategy | Meaning | Benefit | Risk |
|---|---|---|---|
| Exploitation | Choose the action currently estimated to be best | Gains reward from current knowledge | May commit to a wrong estimate |
| Exploration | Try another action | Improves knowledge and may discover a better action | May sacrifice immediate reward |

This is not a one-time decision. The agent must manage the trade-off throughout learning.

---

## Epsilon-Greedy Action Selection ☆

An epsilon-greedy policy behaves greedily most of the time and selects a random action with probability {{< katex >}} \varepsilon {{< /katex >}}.

{{% colour "green" %}}
{{< katex display=true >}}
A_t =
\begin{cases}
\underset{a}{\operatorname{arg\,max}}\;Q_t(a), & \text{with probability } 1-\varepsilon \\
\text{a random action}, & \text{with probability } \varepsilon
\end{cases}
{{< /katex >}}
{{% /colour %}}

The random choice is made from all actions, so the greedy action can also be selected during the exploratory part.

### Probability of Selecting the Greedy Action ☆

With {{< katex >}} k {{< /katex >}} actions and one unique greedy action:

{{% colour "green" %}}
{{< katex display=true >}}
\Pr(\text{greedy action}) = (1-\varepsilon) + \frac{\varepsilon}{k}
{{< /katex >}}
{{% /colour %}}

For two actions and {{< katex >}} \varepsilon = 0.5 {{< /katex >}}:

{{% colour "green" %}}
{{< katex display=true >}}
\Pr(\text{greedy action}) = 0.5 + \frac{0.5}{2} = 0.75
{{< /katex >}}
{{% /colour %}}

---

## The 10-Armed Testbed

The slides compare greedy and epsilon-greedy methods on a 10-armed testbed.

The qualitative result is:

- a fully greedy method improves quickly but may settle on a suboptimal action;
- a small amount of exploration improves long-term performance;
- in the shown experiment, {{< katex >}} \varepsilon = 0.1 {{< /katex >}} achieves a higher average reward and a higher percentage of optimal actions than the greedy method;
- {{< katex >}} \varepsilon = 0.01 {{< /katex >}} explores more slowly and therefore improves more gradually.

{{% hint info %}}
The best value of {{< katex >}} \varepsilon {{< /katex >}} depends on reward uncertainty, the length of the task, and whether action values change over time.
{{% /hint %}}

---

## Incremental Sample-Average Update ☆

Recomputing a complete average after every reward is inefficient. The estimate can be updated incrementally.

{{% colour "green" %}}
{{< katex display=true >}}
Q_{n+1} = Q_n + \frac{1}{n}\left[R_n - Q_n\right]
{{< /katex >}}
{{% /colour %}}

This has the general form:

{{% colour "green" %}}
{{< katex display=true >}}
\text{New Estimate}
=
\text{Old Estimate}
+
\text{Step Size}\left[\text{Target}-\text{Old Estimate}\right]
{{< /katex >}}
{{% /colour %}}

The term {{< katex >}} R_n-Q_n {{< /katex >}} is the estimation error.

---

## Incremental Epsilon-Greedy Bandit Algorithm ☆

```text
Initialise, for each action a:
    Q(a) = 0
    N(a) = 0

Repeat:
    With probability 1 - epsilon:
        choose an action with the largest Q(a)
    Otherwise:
        choose a random action

    Observe reward R
    N(a) = N(a) + 1
    Q(a) = Q(a) + (1 / N(a)) * (R - Q(a))
```

### Minimal Python Implementation

```python
import random
from typing import Sequence


def select_action(values: Sequence[float], epsilon: float) -> int:
    """Select an action using epsilon-greedy exploration."""
    if not 0.0 <= epsilon <= 1.0:
        raise ValueError("epsilon must be between 0 and 1")
    if not values:
        raise ValueError("values must not be empty")

    if random.random() < epsilon:
        return random.randrange(len(values))

    best_value = max(values)
    best_actions = [i for i, value in enumerate(values) if value == best_value]
    return random.choice(best_actions)


def update_sample_average(old_value: float, reward: float, count: int) -> float:
    """Update an action value after its count-th observation."""
    if count <= 0:
        raise ValueError("count must be positive")
    return old_value + (reward - old_value) / count
```

---

## Stationary and Non-Stationary Bandits ☆

### Stationary Problem

The reward distribution of each action remains fixed over time.

The sample-average method is suitable because all observations remain equally relevant.

### Non-Stationary Problem

The true action values change over time.

Older rewards may no longer describe the current problem, so recent observations should receive greater weight.

A constant step size is used:

{{% colour "green" %}}
{{< katex display=true >}}
Q_{n+1} = Q_n + \alpha\left[R_n-Q_n\right]
{{< /katex >}}
{{% /colour %}}

Expanding the recursion gives an exponentially recency-weighted average:

{{% colour "green" %}}
{{< katex display=true >}}
Q_{n+1}
=
(1-\alpha)^n Q_1
+
\sum_{i=1}^{n}\alpha(1-\alpha)^{n-i}R_i
{{< /katex >}}
{{% /colour %}}

Recent rewards receive larger weights than older rewards.

---

## Sample Average versus Constant Step Size ☆

| Property | Sample Average | Constant Step Size |
|---|---|---|
| Step size | {{< katex >}} 1/n {{< /katex >}} | Fixed {{< katex >}} \alpha {{< /katex >}} |
| Weighting | All rewards eventually weighted equally | Recent rewards weighted more strongly |
| Best suited to | Stationary tasks | Non-stationary tasks |
| Continual adaptation | Decreases over time | Maintained |

---

## Optimistic Initial Values ☆

Instead of initialising every estimate to zero, the agent can begin with unrealistically high action values.

For example, setting every estimate to {{< katex >}} 5 {{< /katex >}} makes the first rewards appear disappointing. The learner then switches between actions, causing several actions to be tried before estimates converge.

### Benefits

- encourages early exploration;
- simple to implement.

### Limitations

- depends on a suitable scale for the initial values;
- the effect is temporary for sample-average methods;
- with a constant step size, the initial bias can persist for longer;
- it is less reliable in non-stationary settings because it primarily encourages exploration near the start.

---

## Upper-Confidence-Bound Action Selection ☆

Epsilon-greedy exploration treats non-greedy actions indiscriminately. Upper-Confidence-Bound (UCB) action selection instead prefers actions that are either promising or uncertain.

{{% colour "green" %}}
{{< katex display=true >}}
A_t
=
\underset{a}{\operatorname{arg\,max}}
\left[
Q_t(a)
+
c\sqrt{\frac{\ln t}{N_t(a)}}
\right]
{{< /katex >}}
{{% /colour %}}

The two terms represent:

- {{< katex >}} Q_t(a) {{< /katex >}} - estimated action value;
- {{< katex >}} c\sqrt{\ln(t)/N_t(a)} {{< /katex >}} - uncertainty bonus.

An action selected infrequently has a larger uncertainty bonus. As it is selected more often, the bonus decreases.

{{% hint info %}}
UCB implements **optimism in the face of uncertainty**: an uncertain action is treated as potentially better until enough evidence is collected.
{{% /hint %}}

---

## From Action Values to Action Preferences

Value-based bandit methods estimate {{< katex >}} Q(a) {{< /katex >}}. Gradient bandit methods instead learn a numerical preference {{< katex >}} H_t(a) {{< /katex >}} for every action.

A softmax function converts preferences into probabilities.

{{% colour "green" %}}
{{< katex display=true >}}
\pi_t(a)
=
\Pr(A_t=a)
=
\frac{e^{H_t(a)}}{\sum_b e^{H_t(b)}}
{{< /katex >}}
{{% /colour %}}

Unlike hard argmax selection, softmax produces a differentiable stochastic policy and naturally supports exploration.

---

## Gradient Bandit Update ☆

After choosing action {{< katex >}} A_t {{< /katex >}} and receiving reward {{< katex >}} R_t {{< /katex >}}, preferences are adjusted relative to a baseline {{< katex >}} \bar{R}_t {{< /katex >}}.

For the selected action:

{{% colour "green" %}}
{{< katex display=true >}}
H_{t+1}(A_t)
=
H_t(A_t)
+
\alpha(R_t-\bar{R}_t)\left[1-\pi_t(A_t)\right]
{{< /katex >}}
{{% /colour %}}

For every other action:

{{% colour "green" %}}
{{< katex display=true >}}
H_{t+1}(a)
=
H_t(a)
-
\alpha(R_t-\bar{R}_t)\pi_t(a),
\qquad a \ne A_t
{{< /katex >}}
{{% /colour %}}

### Interpretation

- reward above the baseline increases the selected action's preference;
- reward below the baseline decreases it;
- preferences of other actions move in the opposite direction;
- the baseline reduces variance and improves learning behaviour.

This is the bandit-gradient algorithm interpreted as stochastic gradient ascent on expected reward.

---

## Associative Search and Contextual Bandits ☆

A basic bandit is **non-associative**: the agent repeatedly faces the same action-selection problem.

An associative or contextual bandit includes a situation or context. The best action may differ from one context to another.

{{% colour "green" %}}**Policy: a mapping from situations to actions that are best in those situations.**{{% /colour %}}

Examples include:

- choosing an advertisement based on a user's context;
- selecting a treatment based on patient features;
- recommending content based on current behaviour;
- selecting actions for different grid locations or operating conditions.

Contextual bandits form a bridge between simple bandits and full Markov Decision Processes.

---

## Method Comparison ☆

| Method | Main idea | Exploration mechanism | Important limitation |
|---|---|---|---|
| Greedy | Select largest estimate | None | Can lock onto a suboptimal action |
| Epsilon-greedy | Usually greedy, sometimes random | Uniform random exploration | Does not target uncertainty |
| Optimistic initialisation | Start with high estimates | Early disappointment forces switching | Mainly encourages early exploration |
| UCB | Add an uncertainty bonus | Select uncertain promising actions | More difficult to extend to general RL |
| Gradient bandit | Learn action preferences | Stochastic softmax policy | Requires careful step size and baseline |

---

## Common Mistakes ☆

{{% hint warning %}}
- Confusing the unknown true value {{< katex >}} q_*(a) {{< /katex >}} with the estimate {{< katex >}} Q_t(a) {{< /katex >}}.
- Assuming epsilon-greedy never selects the greedy action during random exploration.
- Using the sample-average update for a rapidly changing non-stationary problem without considering constant {{< katex >}} \alpha {{< /katex >}}.
- Treating UCB's uncertainty term as part of the estimated reward rather than an exploration bonus.
- Calling a contextual bandit a full MDP even though actions do not affect a sequence of future states in the same way.
{{% /hint %}}

---

## Checklist ☆

You should be able to:

1. define the k-armed bandit problem;
2. distinguish {{< katex >}} q_*(a) {{< /katex >}} from {{< katex >}} Q_t(a) {{< /katex >}};
3. derive and apply the sample-average and incremental updates;
4. calculate epsilon-greedy action probabilities;
5. explain exploration versus exploitation;
6. compare stationary and non-stationary updates;
7. explain optimistic initial values;
8. interpret every term in the UCB equation;
9. derive softmax probabilities from action preferences;
10. explain the gradient bandit update and the role of the reward baseline;
11. distinguish non-associative from associative bandit tasks.

{{% hint success %}}
**Key takeaway:** Bandit methods learn which action to choose while simultaneously collecting the evidence needed to estimate action quality.
{{% /hint %}}

---

## References

1. Sutton and Barto, *Reinforcement Learning: An Introduction*, Chapter 2.
2. BITS Pilani Deep Reinforcement Learning course handout, Contact Session 2.
3. Supplied lecture slides on Multi-Armed Bandits, including action-value methods, non-stationarity, UCB, gradient bandits, and associative search.

---
{{< home-link "Home" >}} | {{< section-index >}}
