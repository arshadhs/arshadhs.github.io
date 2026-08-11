---
title: "Deep Reinforcement Learning"
draft: false
tags: ["Deep Reinforcement Learning", "Reinforcement Learning", "AI", "ML"]
categories: ["AI", "ML"]
weight: 045
menu: main
bookCollapseSection: true
---

# Deep Reinforcement Learning

Deep Reinforcement Learning (DRL) studies how an **agent** learns to make a sequence of decisions by interacting with an **environment** and receiving feedback through rewards.

Reinforcement learning provides the framework for sequential decision-making. Deep learning extends this framework with powerful function approximators that can handle large or complex state and action spaces.

{{% hint info %}}
**Deep Reinforcement Learning = Reinforcement Learning + Deep Neural Networks**
{{% /hint %}}

The learning path begins with classical reinforcement learning foundations and progresses towards value-based deep learning, policy-gradient methods, model-based approaches, and imitation learning.

---

## Big Picture

{{< mermaid >}}
flowchart TD
    A["RL Foundations"] --> B["Bandits"]
    B --> C["MDPs"]
    C --> D["Dynamic Programming"]
    D --> E["Monte Carlo"]
    E --> F["Temporal-Difference Learning"]
    F --> G["Function Approximation"]
    G --> H["Deep Q-Learning"]
    H --> I["Policy Gradients"]
    I --> J["Model-Based Deep RL"]
    J --> K["Imitation Learning"]

    style A fill:#E1F5FE
    style B fill:#C8E6C9
    style C fill:#FFF9C4
    style D fill:#EDE7F6
    style E fill:#E1F5FE
    style F fill:#C8E6C9
    style G fill:#FFF9C4
    style H fill:#EDE7F6
    style I fill:#E1F5FE
    style J fill:#C8E6C9
    style K fill:#FFF9C4
{{< /mermaid >}}

---

## Modular Structure

### 1. Introduction: Introducing Reinforcement Learning

- Introduction to Reinforcement Learning
- Examples and applications
- Elements of Reinforcement Learning
  - Policy
  - Reward
  - Value
  - Model of the environment
- Characteristics of reinforcement learning
- Reinforcement Learning for Tic-Tac-Toe
- Historical background
- Multi-Armed Bandit Problem
- Stationary and non-stationary bandits
- Exploration versus exploitation
- Bandit Gradient Algorithm
- Associative Search

### 2. Markov Decision Process Framework

- Finite Markov Decision Processes
- Agent-environment interaction
- Goals
- Rewards and returns
- Policies and value functions
- Bellman equations
- Optimal policies
- Optimal value functions

### 3. Approaches to Solving Reinforcement Learning Problems

- Dynamic Programming
  - Policy Iteration
  - Value Iteration
  - Generalised Policy Iteration
  - Efficiency of Dynamic Programming
- Monte Carlo Methods
  - Prediction
  - Control
  - Incremental Monte Carlo
- Temporal-Difference Learning
- Relationship between Dynamic Programming, Monte Carlo, and Temporal-Difference methods

### 4. Classification of Reinforcement Learning Approaches

- Model-Based versus Model-Free learning
- Value-Based versus Policy-Based learning
- On-Policy versus Off-Policy learning
- Deep Learning as a function approximator

### 5. Value-Based Deep Reinforcement Learning

- Function approximation
- Feature construction for linear methods
- Tile Coding and Asymmetric Tile Coding
- Linear function approximation
- Semi-Gradient TD methods
- Off-policy function approximation and TD divergence
- Deep Q-Networks
- Double DQN
- Dueling Networks
- Prioritised Experience Replay
- Rainbow

### 6. Policy Gradient Methods

- Policy Gradient Methods
- Policy Gradient Theorem
- REINFORCE
- REINFORCE with baseline
- Actor-Critic methods
- A2C and A3C
- Proximal Policy Optimisation
- Entropy regularisation
- Continuous control
- Deterministic Policy Gradient
- DDPG
- TD3
- Soft Actor-Critic

### 7. Model-Based Deep Reinforcement Learning

- Upper-Confidence-Bound Action Selection
- Monte Carlo Tree Search
- AlphaGo Zero
- MuZero
- PlaNet
- Dreamer and latent world models

### 8. Imitation Learning

- Introduction to Imitation Learning
- Supervised approaches to imitation
- Behaviour Cloning
- Inverse Reinforcement Learning
- GAIL
- Dataset augmentation
- DAGGER
- Applications in autonomous driving, game playing, and robotics

### 9. Multi-Agent Reinforcement Learning

- Multi-agent environments
- Cooperative and competitive agents
- Centralised versus decentralised reinforcement learning
- Multi-Agent PPO

### 10. Special Topics

- Safe Reinforcement Learning
- Constrained RL
- Safe exploration
- Adversarial training
- Corrigibility
- Distribution shift
- Human-in-the-loop learning
- Formal methods in Safe RL
- Offline / Batch Reinforcement Learning

---

## 16-Topic Learning Path

| # | Topic | Main Focus | Reference |
|---:|---|---|---|
| 1 | Introducing Reinforcement Learning | RL concepts, examples, policy, reward, value, environment model, Tic-Tac-Toe, historical background | T1 Ch. 1 |
| 2 | Multi-Armed Bandit Problem | Bandits, incremental updates, stationary and non-stationary problems, exploration versus exploitation, gradient bandits, associative search | T1 Ch. 2 |
| 3 | Markov Decision Processes | Agent-environment interaction, goals, rewards, returns, policies, value functions, Bellman equations, optimality | T1 Ch. 3 |
| 4 | Dynamic Programming | Policy Iteration, Value Iteration, Generalised Policy Iteration, efficiency | T1 Ch. 4 |
| 5 | Monte Carlo Methods I | On-policy Monte Carlo, first-visit and every-visit prediction, control, exploring starts, epsilon-soft policies | T1 Ch. 5 |
| 6 | Monte Carlo Methods II | Off-policy Monte Carlo, ordinary and weighted importance sampling, prediction and control, link between MC and TD | T1 Ch. 5 |
| 7 | Temporal-Difference Learning I | TD learning, TD(0), SARSA, Q-Learning, Expected SARSA | T1 Ch. 6–7 |
| 8 | Temporal-Difference Learning II and DRL Taxonomy | n-step returns, TD(lambda), model-based/model-free, value-based/policy-based, on-policy/off-policy | T1 Ch. 6–7, Notes |
| 9 | Value-Based DRL: Function Approximation | Function approximation, tile coding, linear methods, Semi-Gradient TD, off-policy divergence, TD-Gammon | T1 Ch. 9, 16 |
| 10 | Deep Q-Learning | DQN architecture, target networks, replay buffer, instability and fixes | T2 Ch. 4, DQN paper |
| 11 | Extensions of DQN | Double DQN, Dueling Networks, Prioritised Replay, Rainbow | T2 Ch. 5 |
| 12 | Policy Gradient Foundations | Policy Gradient Theorem, REINFORCE, baseline, Actor-Critic, continuing problems | T2 Ch. 13 |
| 13 | Advanced Policy Optimisation | A2C, A3C, PPO, entropy regularisation | T2 Ch. 6–7, Notes |
| 14 | Continuous Control | Deterministic Policy Gradient, DDPG, TD3, Soft Actor-Critic | Notes |
| 15 | Model-Based Deep RL | Model-based learning, AlphaGo Zero, MuZero, PlaNet, Dreamer | Research papers |
| 16 | Imitation Learning | Behaviour Cloning, DAGGER, GAIL, introduction to Multi-Agent RL and Safe RL | DeepMimic, BAIL, ACM-SUR-IL |

---

## How the Classical Methods Fit Together

| Method | Environment Model Required? | Complete Episode Required? | Bootstraps? |
|---|---:|---:|---:|
| Dynamic Programming | Yes | No | Yes |
| Monte Carlo | No | Yes | No |
| Temporal-Difference | No | No | Yes |

---

## Experiments

| # | Experiment | Related Topic |
|---:|---|---:|
| 1 | Implement the Bandit Gradient Algorithm | 2 |
| 2 | Implement Dynamic Programming | 3–4 |
| 3 | Implement Q-Learning | 7 |
| 4 | Implement DQN and DDQN | 10–11 |
| 5 | Implement Policy Gradient algorithms using REINFORCE and Actor-Critic methods | 12–13 |
| 6 | Implement Imitation Learning algorithms | 16 |

### Practical Tools

- **Programming language:** Python
- **Libraries:** OpenAI Gym, PyTorch, TensorFlow, Keras, NumPy
- **Environment:** Google Colab

---

## Book References

### Primary Textbook

1. Richard S. Sutton and Andrew G. Barto, *Reinforcement Learning: An Introduction*, Second Edition, MIT Press.

### Reference Textbook

1. Laura Graesser and Wah Loon Keng, *Foundations of Deep Reinforcement Learning: Theory and Practice in Python*, Addison-Wesley Data & Analytics Series, First Edition.

---

## Key Takeaways

{{% hint success %}}
- Reinforcement learning begins with interaction, rewards, policies, and value functions.
- MDPs provide the mathematical framework for sequential decision-making.
- Dynamic Programming, Monte Carlo, and Temporal-Difference learning form the classical foundation.
- Function approximation makes reinforcement learning scalable to large problems.
- Deep Q-Learning and Policy Gradient methods provide the main paths into modern DRL.
- Model-based and imitation-learning approaches extend DRL beyond direct trial-and-error learning.
{{% /hint %}}

---

{{< home-link "Home" >}} | {{< section-index >}}
