---
title: "Rewards, Returns, Policies and Value Functions"
draft: false
tags: ["AI", "ML", "Reinforcement Learning", "MDP", "Bellman Equation"]
categories: ["AI", "ML"]
weight: 400
menu: main
---

# Rewards, Returns, Policies and Value Functions

An MDP describes how states, actions, rewards and transitions fit together. The next task is to evaluate behaviour: what should the agent try to achieve, how should future rewards be counted, and how good is a state or action over the long term?

Rewards define the objective, returns combine rewards across time, a policy describes behaviour, and value functions predict the long-term quality of that behaviour.

{{% colour "blue" %}}**Reward is immediate feedback; return is accumulated feedback; value is expected return.**{{% /colour %}}

---

## Goals and Rewards ☆

The **reward hypothesis** proposes that a goal can be expressed as maximising the expected cumulative value of a scalar reward signal.

The agent does not directly understand ideas such as winning a game, cleaning a room or driving safely. It learns to prefer behaviour that produces greater return.

### Specify What, Not How

A reward should represent what is to be achieved without prescribing every step of the solution. If the complete action sequence is already programmed, there is little left for the agent to learn.

{{% hint info %}}
The reward defines the destination. Learning discovers the route.
{{% /hint %}}

### Reward Design Failures

A poorly designed reward may be maximised in an unintended way.

| Intended goal | Poor reward | Possible unintended behaviour |
|---|---|---|
| Win at chess | Reward every captured piece | Capture pieces while falling into a losing trap |
| Clean a room | Reward every unit of dirt collected | Deposit dirt and collect it repeatedly |
| Reach a destination efficiently | Reward only arrival | Reach the goal using an unnecessarily long route |

A better design may combine positive rewards for accomplishing the goal with penalties for unsafe, wasteful or manipulative behaviour.

{{% hint warning %}}
An RL agent follows the incentives encoded in the reward, not the designer's unstated intention. Reward design is therefore part of modelling the problem, not a cosmetic implementation detail.
{{% /hint %}}

---

## Reward versus Return ☆

The reward {{< katex >}} R_{t+1} {{< /katex >}} is the immediate feedback received after action {{< katex >}} A_t {{< /katex >}}.

The **return** {{< katex >}} G_t {{< /katex >}} combines rewards that arrive from time {{< katex >}} t+1 {{< /katex >}} onwards.

| Quantity | Meaning |
|---|---|
| {{< katex >}} R_{t+1} {{< /katex >}} | Immediate reward after the current action |
| {{< katex >}} G_t {{< /katex >}} | Total future reward from the current time |
| {{< katex >}} V_\pi(s) {{< /katex >}} | Expected return from state {{< katex >}} s {{< /katex >}} under policy {{< katex >}} \pi {{< /katex >}} |

An agent aims to maximise expected return rather than a single immediate reward.

---

## Episodic Tasks ☆

An **episodic task** naturally divides interaction into episodes. Each episode ends at a terminal time {{< katex >}} T {{< /katex >}}.

Examples include:

- a game ending in a win, loss or draw;
- a trip through a maze;
- an attempt to balance a pole until it falls.

For an undiscounted episodic task:

{{% colour "blue" %}}
{{< katex display=true >}}
G_t
=
R_{t+1}+R_{t+2}+\cdots+R_T
{{< /katex >}}
{{% /colour %}}

A new episode begins after the terminal state is reached.

---

## Continuing Tasks and Discounted Return ☆

A **continuing task** has no natural terminal state. If a positive reward is received forever, simply adding all future rewards may produce an infinite return.

The solution is to discount rewards that lie further in the future:

{{% colour "blue" %}}
{{< katex display=true >}}
G_t
=
R_{t+1}
+\gamma R_{t+2}
+\gamma^2 R_{t+3}
+\cdots
=
\sum_{k=0}^{\infty}\gamma^k R_{t+k+1}
{{< /katex >}}
{{% /colour %}}

The discount rate satisfies:

{{% colour "blue" %}}
{{< katex display=true >}}
0 \leq \gamma \leq 1
{{< /katex >}}
{{% /colour %}}

### Meaning of the Discount Rate

| Value of {{< katex >}} \gamma {{< /katex >}} | Behaviour |
|---|---|
| {{< katex >}} 0 {{< /katex >}} | Only the immediate reward matters |
| Close to {{< katex >}} 0 {{< /katex >}} | Strong preference for near-term rewards |
| Close to {{< katex >}} 1 {{< /katex >}} | Distant rewards remain important |
| {{< katex >}} 1 {{< /katex >}} | Future rewards are not discounted; may be unsuitable for continuing tasks |

The discount rate can represent time preference and also keep infinite-horizon returns finite.

### Constant Reward Example ☆

If the agent receives reward {{< katex >}} +1 {{< /katex >}} forever and {{< katex >}} \gamma=0.95 {{< /katex >}}:

{{% colour "blue" %}}
{{< katex display=true >}}
G_t
=
1+0.95+0.95^2+\cdots
=
\frac{1}{1-0.95}
=
20
{{< /katex >}}
{{% /colour %}}

---

## Recursive Form of Return ☆

The discounted return can be separated into the immediate reward and the remaining return:

{{% colour "blue" %}}
{{< katex display=true >}}
G_t
=
R_{t+1}+\gamma G_{t+1}
{{< /katex >}}
{{% /colour %}}

This one-step recursive relationship is fundamental. It allows long-term quantities to be expressed using the immediate reward and the value of what follows.

{{% hint success %}}
The Bellman equations are built from the same pattern: **current reward plus discounted future value**.
{{% /hint %}}

---

## Cart-Pole as Episodic or Continuing

The cart-pole task applies forces to a moving cart so that a hinged pole remains upright.

### Episodic Formulation

- reward {{< katex >}} +1 {{< /katex >}} for every time step the pole remains balanced;
- the episode ends when the pole falls or a boundary is crossed;
- greater return means balancing for longer.

### Continuing Formulation

- the task does not terminate after failure;
- failure may produce a large negative reward;
- the system is reset and interaction continues;
- discounting controls the influence of the unending future.

The physical system may be identical, but the return and terminal-state design change the learning problem.

---

## Policy ☆

A **policy** maps states to probabilities of selecting actions.

{{% colour "blue" %}}
{{< katex display=true >}}
\pi(a\mid s)
=
\Pr(A_t=a\mid S_t=s)
{{< /katex >}}
{{% /colour %}}

A deterministic policy selects one action in each state. A stochastic policy assigns a probability distribution over the available actions.

The purpose of learning is to improve the policy using experience.

---

## State-Value Function ☆

The state-value function under policy {{< katex >}} \pi {{< /katex >}} is the expected return when the agent starts in state {{< katex >}} s {{< /katex >}} and then follows {{< katex >}} \pi {{< /katex >}}.

{{% colour "blue" %}}
{{< katex display=true >}}
v_\pi(s)
\doteq
\mathbb{E}_\pi[G_t\mid S_t=s]
{{< /katex >}}
{{% /colour %}}

It answers:

> How good is it to be in this state while following this policy?

---

## Action-Value Function ☆

The action-value function under policy {{< katex >}} \pi {{< /katex >}} is the expected return after taking action {{< katex >}} a {{< /katex >}} in state {{< katex >}} s {{< /katex >}} and then following {{< katex >}} \pi {{< /katex >}}.

{{% colour "blue" %}}
{{< katex display=true >}}
q_\pi(s,a)
\doteq
\mathbb{E}_\pi[G_t\mid S_t=s,A_t=a]
{{< /katex >}}
{{% /colour %}}

It answers:

> How good is this action in this state while following this policy afterwards?

---

## Relationship Between State and Action Values ☆

The value of a state is the policy-weighted average of its action values:

{{% colour "blue" %}}
{{< katex display=true >}}
v_\pi(s)
=
\sum_a \pi(a\mid s)q_\pi(s,a)
{{< /katex >}}
{{% /colour %}}

If the transition model is known, an action value can be expressed using next-state values:

{{% colour "blue" %}}
{{< katex display=true >}}
q_\pi(s,a)
=
\sum_{s',r}
p(s',r\mid s,a)
\left[r+\gamma v_\pi(s')\right]
{{< /katex >}}
{{% /colour %}}

| Function | Conditions on the present | Main question |
|---|---|---|
| {{< katex >}} v_\pi(s) {{< /katex >}} | State is fixed | How good is this state? |
| {{< katex >}} q_\pi(s,a) {{< /katex >}} | State and first action are fixed | How good is this action here? |

---

## Bellman Expectation Equation ☆

The Bellman equation decomposes a state's value into:

1. the expected immediate reward;
2. the discounted value of the expected next state.

{{% colour "blue" %}}
{{< katex display=true >}}
v_\pi(s)
=
\sum_a \pi(a\mid s)
\sum_{s',r}p(s',r\mid s,a)
\left[r+\gamma v_\pi(s')\right]
{{< /katex >}}
{{% /colour %}}

The equation is an expectation over:

- actions chosen by the policy;
- next states and rewards produced by the environment.

{{% hint info %}}
The Bellman equation does not merely add rewards. It links the value of one state to the values of possible successor states.
{{% /hint %}}

---

## Gridworld Interpretation

Suppose a gridworld uses an equiprobable random policy with four actions. Each action is selected with probability {{< katex >}} 1/4 {{< /katex >}}.

For any state {{< katex >}} s {{< /katex >}}, its value is the average of the four one-step outcomes:

{{% colour "blue" %}}
{{< katex display=true >}}
v_\pi(s)
=
\frac{1}{4}
\sum_{a\in\{\uparrow,\downarrow,\leftarrow,\rightarrow\}}
\left[r(s,a)+\gamma v_\pi(s')\right]
{{< /katex >}}
{{% /colour %}}

An action that attempts to leave the grid may keep the agent in the same state and produce a negative reward. A special state may instead send the agent to another cell with a larger positive reward.

Repeated Bellman updates propagate this information through the grid.

---

## Comparing Policies ☆

A policy {{< katex >}} \pi {{< /katex >}} is at least as good as policy {{< katex >}} \pi' {{< /katex >}} if:

{{% colour "blue" %}}
{{< katex display=true >}}
v_\pi(s)\geq v_{\pi'}(s)
\qquad \text{for every } s\in\mathcal{S}
{{< /katex >}}
{{% /colour %}}

An **optimal policy**, denoted {{< katex >}} \pi_* {{< /katex >}}, is at least as good as every other policy. More than one optimal policy may exist.

---

## Optimal Value Functions ☆

The optimal state-value function gives the greatest achievable expected return from each state:

{{% colour "blue" %}}
{{< katex display=true >}}
v_*(s)
=
\max_\pi v_\pi(s)
{{< /katex >}}
{{% /colour %}}

The optimal action-value function gives the greatest achievable expected return after taking an action in a state:

{{% colour "blue" %}}
{{< katex display=true >}}
q_*(s,a)
=
\max_\pi q_\pi(s,a)
{{< /katex >}}
{{% /colour %}}

If {{< katex >}} q_*(s,a) {{< /katex >}} is known, an optimal policy can choose an action that maximises it.

---

## Bellman Optimality Equations ☆

The optimal value of a state uses the best available action rather than averaging actions according to a fixed policy:

{{% colour "blue" %}}
{{< katex display=true >}}
v_*(s)
=
\max_a
\sum_{s',r}p(s',r\mid s,a)
\left[r+\gamma v_*(s')\right]
{{< /katex >}}
{{% /colour %}}

For action values:

{{% colour "blue" %}}
{{< katex display=true >}}
q_*(s,a)
=
\sum_{s',r}p(s',r\mid s,a)
\left[r+\gamma\max_{a'}q_*(s',a')\right]
{{< /katex >}}
{{% /colour %}}

### Expectation versus Optimality

| Equation | Action selection |
|---|---|
| Bellman expectation equation | Averages actions using {{< katex >}} \pi(a\mid s) {{< /katex >}} |
| Bellman optimality equation | Selects the maximum-valued action |

{{% hint warning %}}
Do not replace an expectation with a maximum unless the objective is optimal control. Policy evaluation asks how good a given policy is; optimality asks how good the best possible behaviour can be.
{{% /hint %}}

---

## Common Mistakes ☆

{{% hint warning %}}
- Treating immediate reward as the same quantity as return or value.
- Assuming {{< katex >}} \gamma=0 {{< /katex >}} removes all rewards; it retains the immediate reward.
- Using {{< katex >}} \gamma=1 {{< /katex >}} in an infinite continuing task without checking whether the return remains finite.
- Confusing {{< katex >}} v_\pi(s) {{< /katex >}} with {{< katex >}} q_\pi(s,a) {{< /katex >}}.
- Using a maximum in the Bellman expectation equation for a fixed stochastic policy.
- Assuming an apparently reasonable reward cannot be exploited in an unintended way.
{{% /hint %}}

---

## Practice Questions

1. Explain why maximising immediate reward can produce poor long-term behaviour.
2. Calculate the infinite discounted return for reward {{< katex >}} +2 {{< /katex >}} and {{< katex >}} \gamma=0.8 {{< /katex >}}.
3. What changes when {{< katex >}} \gamma {{< /katex >}} is set to zero?
4. Compare episodic and continuing formulations of cart-pole.
5. Explain the difference between {{< katex >}} v_\pi(s) {{< /katex >}} and {{< katex >}} q_\pi(s,a) {{< /katex >}}.
6. Why does the Bellman expectation equation average over actions while the Bellman optimality equation uses a maximum?
7. Give an example of reward hacking and propose a better reward design.

---

## Key Takeaways ☆

{{% hint success %}}
- Rewards specify the agent's objective, so their design must reflect the intended behaviour.
- Return combines future rewards; discounting controls how strongly distant rewards matter.
- A policy maps states to action probabilities.
- State values and action values measure expected return under a policy.
- Bellman equations express long-term value recursively as immediate reward plus discounted future value.
- Optimality equations replace policy-weighted action averages with the best available action.
{{% /hint %}}

---

## Checklist

- [ ] I can distinguish reward, return and value.
- [ ] I can calculate episodic and discounted returns.
- [ ] I can interpret the discount rate.
- [ ] I can define a policy and distinguish deterministic from stochastic policies.
- [ ] I can explain state-value and action-value functions.
- [ ] I can interpret every term in the Bellman expectation equation.
- [ ] I can distinguish Bellman expectation and Bellman optimality equations.
- [ ] I can explain why reward design can produce unintended behaviour.

---

## References

1. Sutton and Barto, *Reinforcement Learning: An Introduction*, Chapter 3.
2. Supplied Deep Reinforcement Learning slides and recordings on rewards, returns, policies, value functions and Bellman equations.

---
{{< home-link "Home" >}} | {{< section-index >}}
