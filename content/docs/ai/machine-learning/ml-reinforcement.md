---
title: "Reinforcement Learning"
draft: false
tags: ["AI", "Machine Learning", "Reinforcement Learning"]
categories: ["AI", "ML"]
weight: 405
menu: main
---

# Reinforcement Learning (RL)

RL is learning by **trial and error**.

Reinforcement Learning (RL) is a type of machine learning where an **autonomous agent learns to make decisions by interacting with an environment**.

Instead of being told the correct answer, the agent:
- takes actions
- observes outcomes
- receives rewards or penalties
- gradually learns a strategy that maximises long-term reward

> **Reinforcement Learning teaches an agent how to act, not what to predict.**

---

{{< mermaid >}}
flowchart LR
    A[Agent] -->|Action| B[Environment]
    B -->|State + Reward| A
{{< /mermaid >}}

### 1. Agent
- The learner and decision-maker
- Chooses actions based on its current policy

### 2. Environment
- Everything the agent interacts with
- Responds to actions by changing state and giving rewards

### 3. State
- A snapshot of the environment at a given time

### 4. Action
- A choice made by the agent

### 5. Reward
- Feedback signal
- Positive reward → good outcome  
- Negative reward → bad outcome

### 6. Policy
- The agent’s strategy
- A mapping from states to actions
- Goal: **maximise cumulative reward over time**

---

## What Is an Autonomous Agent?

An **autonomous agent** is a system that:
- observes its environment
- makes decisions independently
- acts without direct human instruction

### Examples
- Robots
- Self-driving cars
- Game-playing agents (Chess, Go, Atari)
- Recommendation systems

---

## How Reinforcement Learning Differs from Other ML Types

### Reinforcement Learning vs Supervised Learning

**Supervised Learning**
- Uses labelled data
- Learns from examples of correct answers
- Goal: minimise prediction error

**Reinforcement Learning**
- No labelled “correct” actions
- Learns from rewards and penalties
- Goal: maximise long-term reward

> RL is not told *what the right action is* — it must discover it.

---

### Reinforcement Learning vs Unsupervised Learning

**Unsupervised Learning**
- Finds hidden patterns in data
- No explicit objective tied to actions
- Focuses on structure (clusters, embeddings)

**Reinforcement Learning**
- Learns by **interaction**
- Uses a reward function
- Focuses on **decision-making**

> RL learns **what to do**, not just **what exists**.

---

## A Key Conceptual Difference

- Supervised & Unsupervised Learning:
  - Assume data points are independent
  - Learn a static model from a dataset

- Reinforcement Learning:
  - Data arrives as **sequences**
  - State → Action → Reward → Next State
  - Decisions affect future data

{{% hint info %}}
Reinforcement learning models **sequential decision-making**, which closely mirrors how humans and animals learn.
{{% /hint %}}

---

## Reinforcement Learning from Human Feedback (RLHF)

RLHF is a modern extension of reinforcement learning where **humans help define what “good” looks like**.

### How RLHF Works
1. Humans evaluate model outputs (e.g., ranking answers)
2. A **reward model** is trained from this feedback
3. Reinforcement learning optimises the agent using this reward model

### Why RLHF Matters

Some concepts are hard to define mathematically:
- humour
- helpfulness
- politeness
- safety

Humans can easily **judge** these qualities, even if they can’t formalise them.

### Example
It’s hard to write a formula for “funny”, but easy for humans to rate jokes.  
That feedback becomes a reward signal to improve the model.

RLHF is widely used in:
- Large Language Models
- Chatbots
- AI assistants

---

## Where Reinforcement Learning Is Used

- Robotics
- Autonomous vehicles
- Game-playing AI
- Recommendation systems
- Resource allocation
- Training LLM behaviour (via RLHF)

---

## Summary

- Reinforcement Learning trains **agents**, not predictors
- Learning happens through **trial and error**
- Rewards guide behaviour instead of labels
- Decisions are **sequential and interdependent**
- RLHF brings humans into the reward loop

---

> **Reinforcement Learning teaches machines how to behave by rewarding good decisions and punishing bad ones.**

---

## References

- [Learning through Human Feedback – DeepMind](https://deepmind.google/blog/learning-through-human-feedback/)
- [Reinforcement Learning](https://www.ibm.com/think/topics/reinforcement-learning#1268897081)
- [What is RLHF?](https://www.ibm.com/think/topics/rlhf#1268897082)

---

{{< home-link "Home" >}} | {{< section-index >}}
