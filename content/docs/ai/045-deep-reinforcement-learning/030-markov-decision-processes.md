---
title: "Markov Decision Processes"
draft: false
tags: ["AI", "ML", "Reinforcement Learning", "Markov Decision Process", "MDP", "Bellman Equation"]
categories: ["AI", "ML"]
weight: 300
menu: main
---

# Markov Decision Processes

A **Markov Decision Process (MDP)** is the mathematical framework used to describe sequential decision-making in reinforcement learning. It brings together the **agent**, **environment**, **states**, **actions**, **transition dynamics**, **rewards**, **returns**, **policies**, and **value functions** in one model.

A basic bandit asks which action is best in a recurring situation. An MDP goes further: the action taken now can change the **next state**, which changes the decisions and rewards that become possible later.

Once the MDP itself is defined, the next questions are: **What is the agent trying to achieve? How should rewards over time be counted? How good is a state or action? How do we evaluate a policy, and what does optimal behaviour mean?**

{{% colour "blue" %}}**An MDP models states, actions, transitions and rewards; returns, policies and value functions tell us how good behaviour is over time.**{{% /colour %}}

{{% hint info %}}
**Reward = immediate feedback. Return = accumulated future reward. Value = expected return.**
{{% /hint %}}

- Markov Decision Processes
- Modelling Agent-Environment interaction using MDP; Examples 
- Discussion on Goals , Rewards & Returns; Policy and Value Functions 
- Bellman Equation for value functions 
- Optimal Policy and Optimal Value functions 

---

## From Bandits to Sequential Decisions ☆

### Non-Associative Bandit

A basic multi-armed bandit has one recurring decision context. Choosing an action produces a reward, but it does not move the agent through a sequence of different states.

### Sequential Decision Problem

In a sequential problem:

- the agent can occupy different states;
- the useful action depends on the current state;
- an action influences the next state;
- present choices can affect future rewards.

```mermaid
flowchart LR
    S[Current state] --> A[Choose action]
    A --> E[Environment responds]
    E --> R[Reward]
    E --> N[Next state]
    N --> S

    style S fill:#E1F5FE
    style A fill:#C8E6C9
    style E fill:#FFF9C4
    style R fill:#EDE7F6
    style N fill:#E1F5FE
```

{{% hint info %}}
A simple way to distinguish the two settings is this: a bandit learns **which action is best**, while an MDP learns **which action is best in each state while considering what follows**.
{{% /hint %}}

---

---

## Agent-Environment Interface ☆

The **agent** is the learner and decision-maker. Everything outside the agent is considered part of the **environment**.

At discrete time step {{< katex >}} t {{< /katex >}}:

1. the agent observes state {{< katex >}} S_t {{< /katex >}};
2. it selects action {{< katex >}} A_t {{< /katex >}};
3. the environment produces reward {{< katex >}} R_{t+1} {{< /katex >}};
4. the environment moves to state {{< katex >}} S_{t+1} {{< /katex >}}.

The interaction repeats, producing a trajectory:

{{% colour "blue" %}}
{{< katex display=true >}}
S_0, A_0, R_1, S_1, A_1, R_2, S_2, \ldots
{{< /katex >}}
{{% /colour %}}

The boundary between agent and environment marks the limit of the agent's direct control. It does not necessarily mark the limit of its knowledge.

---

---

## The Markov Property ☆

The defining idea behind an MDP is the **Markov property**:

{{% colour "blue" %}}**Given the present state, the future is independent of the past.**{{% /colour %}}

The current state must contain all information needed to predict the next state and reward after an action. The agent should not need the complete history of earlier states and actions.

### Chess Intuition

Suppose a skilled chess player joins a game already in progress. If the current board position contains all relevant information, the player can choose the next move without being told the full sequence of moves that produced it.

The current board is therefore a suitable Markov state.

### Formal Statement

{{% colour "blue" %}}
{{< katex display=true >}}
\Pr(S_{t+1},R_{t+1}\mid S_t,A_t,S_{t-1},A_{t-1},\ldots)
=
\Pr(S_{t+1},R_{t+1}\mid S_t,A_t)
{{< /katex >}}
{{% /colour %}}

{{% hint warning %}}
The physical world may be Markovian while the agent's observation is not. If important information is omitted from the state representation, the same observed state may lead to different outcomes depending on hidden history.
{{% /hint %}}

---

---

## Components of a Finite MDP ☆

A finite MDP is commonly described using:

| Component | Meaning |
|---|---|
| {{< katex >}} \mathcal{S} {{< /katex >}} | Finite set of states |
| {{< katex >}} \mathcal{A} {{< /katex >}} | Set of actions |
| {{< katex >}} p(s',r\mid s,a) {{< /katex >}} | Probability of the next state and reward |
| Reward mechanism | Numerical feedback defining the goal |
| Start state or distribution | Where interaction begins |
| Terminal state, when applicable | Where an episode ends |

Some problems allow different actions in different states, written as {{< katex >}} \mathcal{A}(s) {{< /katex >}}.

---

---

## Model Dynamics ☆

The dynamics describe how the environment responds when action {{< katex >}} a {{< /katex >}} is taken in state {{< katex >}} s {{< /katex >}}.

### Joint Transition and Reward Probability

{{% colour "blue" %}}
{{< katex display=true >}}
p(s',r\mid s,a)
\doteq
\Pr\left(S_{t+1}=s',R_{t+1}=r\mid S_t=s,A_t=a\right)
{{< /katex >}}
{{% /colour %}}

This distribution gives the probability of receiving reward {{< katex >}} r {{< /katex >}} and arriving in state {{< katex >}} s' {{< /katex >}} after taking action {{< katex >}} a {{< /katex >}} in state {{< katex >}} s {{< /katex >}}.

### State-Transition Probability

If only the next-state probability is required, sum over all possible rewards:

{{% colour "blue" %}}
{{< katex display=true >}}
p(s'\mid s,a)
=
\sum_{r\in\mathcal{R}} p(s',r\mid s,a)
{{< /katex >}}
{{% /colour %}}

### Expected Reward for a State-Action Pair

{{% colour "blue" %}}
{{< katex display=true >}}
r(s,a)
=
\mathbb{E}[R_{t+1}\mid S_t=s,A_t=a]
{{< /katex >}}
{{% /colour %}}

### Expected Reward for a Transition

When the next state is also specified:

{{% colour "blue" %}}
{{< katex display=true >}}
r(s,a,s')
=
\mathbb{E}[R_{t+1}\mid S_t=s,A_t=a,S_{t+1}=s']
{{< /katex >}}
{{% /colour %}}

This quantity distinguishes transitions that begin with the same state and action but arrive in different next states.

The transition model may be deterministic or stochastic:

- **deterministic:** a state-action pair always produces the same next state;
- **stochastic:** several next states are possible, each with a probability.

---

---

## Gridworld Example ☆

Consider an agent moving through a grid:

- each accessible cell is a state;
- actions are north, south, east and west;
- a wall blocks movement through a cell;
- movement is noisy rather than perfectly deterministic;
- terminal cells may produce positive or negative rewards;
- each ordinary step may carry a small cost.

For example, an intended north action may move:

- north with probability {{< katex >}} 0.8 {{< /katex >}};
- west with probability {{< katex >}} 0.1 {{< /katex >}};
- east with probability {{< katex >}} 0.1 {{< /katex >}}.

If the sampled direction is blocked by a wall, the agent remains in the same cell.

This is a sequential decision problem because the chosen movement changes the state from which the next decision must be made.

---

---

## Formulating Real Problems as MDPs ☆

The first modelling task is to identify the states, actions, rewards and transition dynamics. These choices should contain enough information for useful decision-making without making the representation unnecessarily large.

### Video Game

| MDP element | Possible formulation |
|---|---|
| State | Raw image pixels or processed visual features |
| Action | Game controls |
| Reward | Change in game score |
| Dynamics | Rules and stochastic evolution of the game |

### Traffic Signal Control

| MDP element | Possible formulation |
|---|---|
| State | Current lights, approaching vehicles, waiting times, stopped vehicles and speeds |
| Action | Signal assignment or phase change |
| Reward | Reduction in traffic delay |
| Dynamics | Changing and uncertain traffic demand |

### Recycling Robot

A recycling robot searches for cans while managing a rechargeable battery.

| MDP element | Possible formulation |
|---|---|
| State | Battery level: high or low |
| Actions at high charge | Search or wait |
| Actions at low charge | Search, wait or recharge |
| Reward | Reward for collecting cans, with searching more productive than waiting |
| Dynamics | Probabilities of charge remaining high, becoming low, or requiring rescue/recharge |

The actions available can depend on the state. Recharge, for example, may only be meaningful when the battery is low.

---

---

## State Design Matters

A useful state representation should:

- contain the information needed to choose an action;
- preserve the Markov property as far as practical;
- distinguish situations requiring different behaviour;
- avoid irrelevant detail that makes learning unnecessarily difficult.

For a robot avoiding a pit, sensor readings describing the ground ahead may be state information. The decision to move forward, turn or stop belongs to the action space.

{{% hint info %}}
A practical test is to ask: if two observations look identical to the agent, should the same action have the same likely consequences? If not, important state information may be missing.
{{% /hint %}}

---

---

## Model-Based and Model-Free Perspective

The transition and reward rules collectively form a **model of the environment**.

| Approach | Use of environment model |
|---|---|
| Model-based | Uses known or learned dynamics to plan |
| Model-free | Learns values or policies directly from interaction |

An MDP describes the decision problem whether or not the learning algorithm is explicitly given the model.

---

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

---

## Common Mistakes ☆



{{% hint warning %}}

- Treating actions as states or sensor readings as actions.
- Assuming every action leads deterministically to one next state.
- Including too little information in the state to satisfy the Markov property.
- Confusing a contextual bandit with an MDP: in an MDP, actions influence future states and rewards.
- Assuming the agent-environment boundary must coincide with a physical boundary.

- Treating immediate reward as the same quantity as return or value.
- Assuming {{< katex >}} \gamma=0 {{< /katex >}} removes all rewards; it retains the immediate reward.
- Using {{< katex >}} \gamma=1 {{< /katex >}} in an infinite continuing task without checking whether the return remains finite.
- Confusing {{< katex >}} v_\pi(s) {{< /katex >}} with {{< katex >}} q_\pi(s,a) {{< /katex >}}.
- Using a maximum in the Bellman expectation equation for a fixed stochastic policy.
- Assuming an apparently reasonable reward cannot be exploited in an unintended way.

{{% /hint %}}

---

## Practice Questions



1. Why is a basic multi-armed bandit described as non-associative?

2. Explain the Markov property using a chess or navigation example.

3. Distinguish {{< katex >}} p(s',r\mid s,a) {{< /katex >}} from {{< katex >}} p(s'\mid s,a) {{< /katex >}}.

4. Formulate states, actions and rewards for a lift-control system.

5. Why can a poor state representation make an apparently Markov problem non-Markov from the agent's perspective? ---

6. Explain why maximising immediate reward can produce poor long-term behaviour.

7. Calculate the infinite discounted return for reward {{< katex >}} +2 {{< /katex >}} and {{< katex >}} \gamma=0.8 {{< /katex >}}.

8. What changes when {{< katex >}} \gamma {{< /katex >}} is set to zero?

9. Compare episodic and continuing formulations of cart-pole.

10. Explain the difference between {{< katex >}} v_\pi(s) {{< /katex >}} and {{< katex >}} q_\pi(s,a) {{< /katex >}}.

11. Why does the Bellman expectation equation average over actions while the Bellman optimality equation uses a maximum?

12. Give an example of reward hacking and propose a better reward design. ---

---

## Key Takeaways ☆



{{% hint success %}}

- An MDP models sequential interaction using states, actions, transition probabilities and rewards.
- The Markov property requires the present state to contain the information needed for predicting what follows.
- Actions affect both immediate rewards and future states.
- MDP formulation begins by carefully choosing the state, action, reward and dynamics representations.
- Gridworld, video games, traffic control and recycling robots can all be expressed using the same abstract framework.

- Rewards specify the agent's objective, so their design must reflect the intended behaviour.
- Return combines future rewards; discounting controls how strongly distant rewards matter.
- A policy maps states to action probabilities.
- State values and action values measure expected return under a policy.
- Bellman equations express long-term value recursively as immediate reward plus discounted future value.
- Optimality equations replace policy-weighted action averages with the best available action.

{{% /hint %}}

---

## Checklist



- [ ] I can distinguish a bandit problem from an MDP.

- [ ] I can explain the agent-environment interface.

- [ ] I can state and interpret the Markov property.

- [ ] I can interpret {{< katex >}} p(s',r\mid s,a) {{< /katex >}}.

- [ ] I can identify states, actions, rewards and dynamics in a new problem.

- [ ] I can explain why state representation affects whether the Markov property holds.

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

2. Supplied Deep Reinforcement Learning slides and recordings on Markov Decision Processes, associative tasks, rewards, returns, policies, value functions and Bellman equations.

---

{{< home-link "Home" >}} | {{< section-index >}}
