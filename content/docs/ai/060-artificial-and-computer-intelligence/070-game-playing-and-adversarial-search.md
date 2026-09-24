---
title: "Game Playing and Adversarial Search"
draft: false
tags: ["AI", "ACI", "Game Playing", "Minimax", "Alpha-Beta Pruning", "Monte Carlo Tree Search", "Stochastic Games"]
categories: ["AI", "Artificial and Computational Intelligence"]
weight: 700
menu: main
---

# Game Playing and Adversarial Search

Game-playing agents search in environments where other agents influence the outcome. In competitive games, a good move must account not only for what the agent wants to achieve, but also for the strongest response an opponent can make.

## Learning Objectives

- formulate a game as an adversarial search problem
- construct a game tree with alternating MAX and MIN turns
- design and interpret a static evaluation function
- back up values using Minimax
- apply Alpha-Beta Pruning and identify cut-offs
- explain the four phases of Monte Carlo Tree Search
- calculate an MCTS selection score using UCB1
- evaluate chance outcomes in a stochastic game

## Big Picture

{{< mermaid >}}
flowchart TD
    A["Game state"] --> B["Generate legal moves"]
    B --> C["Build game tree"]
    C --> D["Evaluate leaves"]
    D --> E["Choose robust action"]
    E --> A

    style A fill:#E1F5FE
    style B fill:#FFF9C4
    style C fill:#EDE7F6
    style D fill:#FFF9C4
    style E fill:#C8E6C9
{{< /mermaid >}}

## 1. Normal Search and Adversarial Search

In normal search, the agent controls its own actions and the transition model determines what happens next. In adversarial search, another agent chooses actions that may reduce the first agent's utility.

| Aspect | Normal search | Adversarial search |
|---|---|---|
| Decision makers | Usually one | Two or more |
| Goals | One agent's goal | Often conflicting goals |
| Successor choice | Made by the agent or environment | Alternates between players |
| Solution | A path to a goal | A strategy for every relevant opponent response |
| Typical method | BFS, UCS or A* | Minimax, Alpha-Beta or MCTS |

{{% hint info %}}
A route planner asks, “Which path reaches the destination?” A game-playing agent asks, “Which move remains good after my opponent gives the strongest reply?”
{{% /hint %}}

## 2. Formal Game Model ☆

A game can be represented using six components:

| Component | Meaning |
|---|---|
| Initial state | Starting board or situation |
| Player function | Identifies whose turn it is |
| Actions function | Returns the legal moves in a state |
| Result function | Gives the state produced by a move |
| Terminal test | Checks whether the game has ended |
| Utility function | Gives the final outcome value for a player |

For a simple zero-sum game, terminal utilities may be:

| Outcome for MAX | Utility |
|---|---:|
| Win | +1 |
| Draw | 0 |
| Loss | -1 |

MAX attempts to make the value as large as possible. MIN attempts to make the same value as small as possible.

## 3. Game Properties

Games can differ along several dimensions:

- **observable or partially observable** - whether the complete state is visible
- **deterministic or stochastic** - whether actions can have random outcomes
- **single-player, two-player or multiplayer**
- **zero-sum or non-zero-sum** - whether one player's gain is exactly another's loss
- **sequential or simultaneous** - whether players move in turns or at the same time

A standard Minimax problem normally assumes two players, alternating turns, deterministic actions, perfect information and zero-sum utility.

## 4. Constructing a Game Tree ☆

A game tree represents possible move sequences.

1. Place the initial state at **level 0**.
2. Treat the player named as starting the game as **MAX**.
3. Generate every legal MAX move at level 1.
4. Alternate the player at every level: MAX, MIN, MAX, MIN and so on.
5. Continue to a terminal state or the exact depth limit.
6. Assign utility to terminal leaves or static evaluation values to depth-limited leaves.

{{% hint warning %}}
The starting player determines the entire MAX/MIN labelling. If the wrong player is placed at the root, all backed-up choices can be reversed.
{{% /hint %}}

### Game tree and state-space graph

The underlying game may contain repeated positions, but a game tree records move histories. The same board can therefore appear in different nodes after different move sequences. Practical systems use transposition tables to reuse evaluations of repeated positions.

## 5. Static Evaluation Functions ☆

Large games cannot normally be searched to terminal states. A depth-limited search therefore estimates the value of a non-terminal leaf using a **static evaluation function**.

{{% colour "green" %}}
{{< katex display=true >}}
Eval(s)=\sum_{i=1}^{m}w_i f_i(s)
{{< /katex >}}
{{% /colour %}}

where:

- {{< katex >}} f_i(s) {{< /katex >}} is a measurable feature of state {{< katex >}} s {{< /katex >}}
- {{< katex >}} w_i {{< /katex >}} is the importance assigned to that feature

Possible features include material advantage, mobility, safety, board control, distance to a goal or possible winning lines.

### Worked interpretation

Suppose a board is evaluated using:

{{% colour "green" %}}
{{< katex display=true >}}
Eval(s)=4(\text{MAX possible chains})-3(\text{MIN possible chains})
{{< /katex >}}
{{% /colour %}}

If MAX can still complete two chains and MIN can complete one, then:

{{% colour "green" %}}
{{< katex display=true >}}
Eval(s)=4(2)-3(1)=5
{{< /katex >}}
{{% /colour %}}

The positive value means the position favours MAX. The number is an estimate, not a guaranteed final win.

### Utility versus static evaluation

| Utility function | Static evaluation function |
|---|---|
| Applied to a terminal state | Applied at a depth limit before the game ends |
| Represents the true game outcome | Estimates how favourable a position is |
| Often win, draw or loss | Usually a weighted feature score |

## 6. Minimax ☆

Minimax assumes that both players choose optimally:

- MAX chooses the largest child value.
- MIN chooses the smallest child value.

{{% colour "green" %}}
{{< katex display=true >}}
V(s)=
\begin{cases}
U(s), & \text{if } s \text{ is terminal}\\
\max_{a\in Actions(s)}V(Result(s,a)), & \text{if MAX moves}\\
\min_{a\in Actions(s)}V(Result(s,a)), & \text{if MIN moves}
\end{cases}
{{< /katex >}}
{{% /colour %}}

### Worked example

{{< mermaid >}}
flowchart TD
    R["MAX"] --> A["MIN"]
    R --> B["MIN"]
    A --> A1["+3"]
    A --> A2["+5"]
    B --> B1["-2"]
    B --> B2["+4"]

    style R fill:#C8E6C9
    style A fill:#E1F5FE
    style B fill:#E1F5FE
    style A1 fill:#FFF9C4
    style A2 fill:#FFF9C4
    style B1 fill:#FFF9C4
    style B2 fill:#FFF9C4
{{< /mermaid >}}

MIN backs up the smaller value on each branch:

- left branch: {{< katex >}} \min(3,5)=3 {{< /katex >}}
- right branch: {{< katex >}} \min(-2,4)=-2 {{< /katex >}}

MAX then chooses {{< katex >}} \max(3,-2)=3 {{< /katex >}}. The left action is therefore the best guaranteed move.

### Cost of Minimax

With branching factor {{< katex >}} b {{< /katex >}} and search depth {{< katex >}} d {{< /katex >}}, a complete depth-limited tree contains on the order of {{< katex >}} b^d {{< /katex >}} leaves. This rapid growth motivates pruning and selective search.

## 7. Alpha-Beta Pruning ☆

Alpha-Beta Pruning produces the same selected move and root value as Minimax while avoiding branches that cannot affect the result.

- **Alpha** is the best value MAX can already guarantee along the current path.
- **Beta** is the best value MIN can already guarantee along the current path.

Initially:

{{% colour "green" %}}
{{< katex display=true >}}
\alpha=-\infty,\qquad \beta=+\infty
{{< /katex >}}
{{% /colour %}}

At a MAX node, update {{< katex >}} \alpha=\max(\alpha,v) {{< /katex >}}. At a MIN node, update {{< katex >}} \beta=\min(\beta,v) {{< /katex >}}. Stop examining the remaining children when:

{{% colour "green" %}}
{{< katex display=true >}}
\alpha\geq\beta
{{< /katex >}}
{{% /colour %}}

An **alpha cut-off** normally occurs at a MIN node whose value is already no greater than an ancestor's alpha. A **beta cut-off** occurs at a MAX node whose value is already no less than an ancestor's beta.

### Worked left-to-right trace

A MAX root has three MIN children with leaf values:

```text
A: [6, 9, 4]
B: [8, 2, 7]
C: [5, 3, 11]
```

1. A returns {{< katex >}} \min(6,9,4)=4 {{< /katex >}}. The root updates {{< katex >}} \alpha=4 {{< /katex >}}.
2. At B, the running beta becomes 8 and then 2. Since {{< katex >}} \beta=2\leq\alpha=4 {{< /katex >}}, leaf 7 is pruned. This is an alpha cut-off at a MIN node.
3. At C, beta becomes 5 and then 3. Since {{< katex >}} 3\leq4 {{< /katex >}}, leaf 11 is pruned.
4. The root chooses {{< katex >}} \max(4,2,3)=4 {{< /katex >}}, exactly as Minimax would.

### Move ordering

Alpha-Beta Pruning is most effective when strong moves are examined first.

| Ordering | Approximate work |
|---|---|
| Worst ordering | {{< katex >}} O(b^d) {{< /katex >}}, like ordinary Minimax |
| Perfect ordering | {{< katex >}} O(b^{d/2}) {{< /katex >}}, allowing roughly twice the depth in the same time |

Ordering changes how much is pruned, but never changes the correct Minimax value.

## 8. Monte Carlo Tree Search ☆

Monte Carlo Tree Search (MCTS) grows only selected parts of a game tree. It uses simulated play to balance:

- **exploitation** - revisit moves with strong observed results
- **exploration** - investigate moves that have been tried less often

{{< mermaid >}}
flowchart TD
    S["Selection"] --> E["Expansion"]
    E --> R["Simulation"]
    R --> B["Backpropagation"]
    B --> S

    style S fill:#E1F5FE
    style E fill:#FFF9C4
    style R fill:#EDE7F6
    style B fill:#C8E6C9
{{< /mermaid >}}

### The four phases

1. **Selection:** start at the root and repeatedly select a promising child.
2. **Expansion:** add a child for a legal move that has not yet been explored.
3. **Simulation:** play from the new node to an outcome using random or lightweight-policy moves.
4. **Backpropagation:** update visit counts and accumulated results along the selected path.

### UCB1 selection

A common selection score is:

{{% colour "green" %}}
{{< katex display=true >}}
UCB1_i=\frac{w_i}{n_i}+c\sqrt{\frac{\ln N}{n_i}}
{{< /katex >}}
{{% /colour %}}

where:

- {{< katex >}} w_i {{< /katex >}} is the accumulated reward of child {{< katex >}} i {{< /katex >}}
- {{< katex >}} n_i {{< /katex >}} is that child's visit count
- {{< katex >}} N {{< /katex >}} is the parent visit count
- {{< katex >}} c {{< /katex >}} controls exploration

The first term exploits high average reward; the second favours less-visited children.

### Worked selection

Let the parent have {{< katex >}} N=25 {{< /katex >}} visits and three children with win/visit records X = 9/12, Y = 5/9 and Z = 1/4. Using {{< katex >}} c=\sqrt{2} {{< /katex >}}:

| Child | Average reward | Exploration term | UCB1 |
|---|---:|---:|---:|
| X | 0.750 | 0.732 | 1.482 |
| Y | 0.556 | 0.846 | 1.402 |
| Z | 0.250 | 1.269 | 1.519 |

MCTS selects Z. Its observed reward is weakest, but its uncertainty is greatest because it has only four visits.

### When MCTS is useful

MCTS is valuable when:

- the game tree is too large for exhaustive search
- a simulator can produce complete or approximate outcomes
- a strong handcrafted evaluation function is unavailable
- a decision must improve progressively as more computation becomes available

## 9. Stochastic Games ☆

In a stochastic game, an action may lead to several possible outcomes with known or estimated probabilities. The game tree therefore contains **chance nodes** as well as MAX and MIN nodes.

At a chance node, use an expected value:

{{% colour "green" %}}
{{< katex display=true >}}
V(s)=\sum_{r}P(r\mid s,a)V(Result(s,a,r))
{{< /katex >}}
{{% /colour %}}

This gives **expectiminimax**: MAX nodes take a maximum, MIN nodes take a minimum, and chance nodes take a probability-weighted average.

### Expected-utility example

An agent can choose Attack or Defend:

- Attack: 70% chance of +10 and 30% chance of -6
- Defend: 90% chance of +5 and 10% chance of -2

{{% colour "green" %}}
{{< katex display=true >}}
EU(Attack)=0.7(10)+0.3(-6)=5.2
{{< /katex >}}
{{% /colour %}}

{{% colour "green" %}}
{{< katex display=true >}}
EU(Defend)=0.9(5)+0.1(-2)=4.3
{{< /katex >}}
{{% /colour %}}

The rational choice is Attack because 5.2 is greater than 4.3. Choosing only by the worst possible outcome would instead favour Defend, but that would ignore the supplied probabilities.

### Randomness and hidden information

Randomness means an action has uncertain outcomes. **Imperfect information** means part of the state is hidden, such as an opponent's cards. A game may have either property or both. Hidden information may require belief states, information sets or sampling; it is not represented merely by adding a chance node.

## 10. Choosing a Game-Playing Method

| Method | Main idea | Best suited to | Main limitation |
|---|---|---|---|
| Minimax | Back up worst-case optimal replies | Moderate deterministic trees | Exponential growth |
| Alpha-Beta | Prune branches irrelevant to Minimax | Minimax trees with useful move ordering | Worst ordering gives little pruning |
| MCTS | Grow promising branches using simulations | Very large trees and anytime decisions | Statistical estimate; depends on rollouts |
| Expectiminimax | Average chance outcomes using probabilities | Stochastic games | Chance branching can be expensive |

## Common Mistakes

{{% hint warning %}}
- Label the player named as starting the game as MAX, then alternate turns at every level.
- A static evaluation value is from MAX's perspective; MIN selects the smaller value rather than manually changing every sign.
- Evaluate leaves at the specified depth, even when the game has not ended.
- Alpha-Beta Pruning changes the number of evaluated nodes, not the Minimax result.
- A branch can be pruned only after the current alpha and beta bounds make it irrelevant.
- MCTS backpropagates the simulation outcome; it does not expand the complete tree first.
- At a chance node, multiply each outcome by its probability before adding.
{{% /hint %}}

## Practice Questions

1. Formulate a two-player board game using the six game components.
2. Construct a game tree to a stated depth and label MAX and MIN levels.
3. Design a weighted static evaluation function for a simple strategy game.
4. Back up a depth-two tree using Minimax and identify the best root move.
5. Apply Alpha-Beta Pruning left to right and list the pruned leaves.
6. Explain why move ordering affects pruning but not the result.
7. Describe Selection, Expansion, Simulation and Backpropagation for a traffic-control decision.
8. Calculate UCB1 for several children and identify the selected child.
9. Draw a tree containing MAX, MIN and chance nodes and apply expectiminimax.
10. Compare Minimax with MCTS for a very large game tree.

## Key Takeaways

{{% hint success %}}
- A game tree alternates players, and the starting player is treated as MAX.
- Static evaluation estimates non-terminal positions; Minimax backs those values up under optimal opposition.
- Alpha-Beta Pruning returns the same value as Minimax while avoiding irrelevant branches.
- MCTS balances exploitation and exploration through repeated selection, expansion, simulation and backpropagation.
- Stochastic games require probability-weighted chance-node values rather than only minimum and maximum operations.
{{% /hint %}}

## Checklist

- [ ] I can formulate a game as a search problem.
- [ ] I can construct and label a game tree correctly.
- [ ] I can calculate and interpret a static evaluation value.
- [ ] I can apply Minimax from the leaves to the root.
- [ ] I can update alpha and beta and justify every pruned branch.
- [ ] I can explain all four phases of MCTS.
- [ ] I can calculate a UCB1 selection score.
- [ ] I can calculate expected utility at a chance node.

---
{{< home-link "Home" >}} | {{< section-index >}}
