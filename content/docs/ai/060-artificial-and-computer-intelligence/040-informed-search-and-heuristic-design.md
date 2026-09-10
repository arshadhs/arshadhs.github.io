---
title: "Informed Search and Heuristic Design"
draft: false
tags: ["AI", "ACI", "Informed Search", "Heuristics", "Greedy Best-First Search", "A* Search"]
categories: ["AI", "Artificial and Computational Intelligence"]
weight: 400
menu: main
---

# Informed Search and Heuristic Design

Informed search uses additional knowledge to estimate which states are most promising. A good heuristic can greatly reduce unnecessary exploration while preserving solution quality.

## Learning Objectives

- explain the purpose of a heuristic function
- compare Greedy Best-First Search and A* Search
- test heuristics for admissibility and consistency
- explain heuristic dominance and effective branching factor
- derive heuristics from relaxed problems and subproblems
- describe pattern databases, landmarks and learned heuristics

## 1. What Is Informed Search? ☆

Uninformed search knows only how to generate successors and recognise a goal. **Informed search** additionally estimates whether one non-goal state is more promising than another.

{{% colour "green" %}}A heuristic is a practical estimate that guides search towards a goal.{{% /colour %}}

The heuristic function is written as {{< katex >}} h(n) {{< /katex >}} and estimates the remaining cost from node {{< katex >}} n {{< /katex >}} to a goal.

{{% hint info %}}
A heuristic is like a compass in a maze. It does not necessarily reveal the exact route, but it helps the search choose a promising direction.
{{% /hint %}}

## 2. Greedy Best-First Search ☆

**Greedy Best-First Search (GBFS)** expands the node that appears closest to the goal.

{{% colour "green" %}}
{{< katex display=true >}}
f(n) = h(n)
{{< /katex >}}
{{% /colour %}}

It ignores the cost already paid to reach the node. This can make it fast, but a promising-looking state may lie on an expensive route. GBFS therefore does not generally guarantee an optimal solution.

## 3. A* Search ☆

**A* Search** combines the actual cost so far with the estimated remaining cost.

{{% colour "green" %}}
{{< katex display=true >}}
f(n) = g(n) + h(n)
{{< /katex >}}
{{% /colour %}}

where:

- {{< katex >}} g(n) {{< /katex >}} is the actual path cost from the start to {{< katex >}} n {{< /katex >}}
- {{< katex >}} h(n) {{< /katex >}} is the estimated cost from {{< katex >}} n {{< /katex >}} to the goal
- {{< katex >}} f(n) {{< /katex >}} is the estimated total cost of a solution through {{< katex >}} n {{< /katex >}}

At each step, A* expands the frontier node with the smallest {{< katex >}} f(n) {{< /katex >}}.

| Algorithm | Evaluation | Behaviour |
|---|---|---|
| UCS | {{< katex >}} g(n) {{< /katex >}} | Cheapest path so far |
| GBFS | {{< katex >}} h(n) {{< /katex >}} | Apparently closest to goal |
| A* | {{< katex >}} g(n)+h(n) {{< /katex >}} | Balances travelled and remaining cost |

## 4. Admissible Heuristics ☆

A heuristic is **admissible** if it never overestimates the true remaining cost.

{{% colour "green" %}}
{{< katex display=true >}}
0 \leq h(n) \leq h^{*}(n)
{{< /katex >}}
{{% /colour %}}

Here, {{< katex >}} h^{*}(n) {{< /katex >}} is the actual cheapest cost from {{< katex >}} n {{< /katex >}} to a goal.

An admissible heuristic is optimistic: it may underestimate, but it does not exaggerate. Straight-line distance is an admissible estimate of road distance because a road route cannot be shorter than the direct geometric distance.

## 5. Consistent Heuristics ☆

A heuristic is **consistent** if its estimate obeys a triangle-like condition across every transition:

{{% colour "green" %}}
{{< katex display=true >}}
h(n) \leq c(n,a,n') + h(n')
{{< /katex >}}
{{% /colour %}}

The estimate at a node must not exceed the step cost to a successor plus the successor's estimate.

Consistency ensures that {{< katex >}} f(n) {{< /katex >}} values do not decrease along a path. For graph search, this means that once A* expands a node, the cheapest path to that node has already been found.

Every consistent heuristic with {{< katex >}} h(Goal)=0 {{< /katex >}} is admissible, but an admissible heuristic need not always be consistent.

## 6. What Makes a Good Heuristic?

A useful heuristic should be:

- **accurate** — close to the true remaining cost
- **fast to compute** — guidance should not cost more than the search it saves
- **admissible** — when optimal A* search is required
- **consistent** — to avoid decreasing estimates along paths
- **informative** — able to distinguish promising states

### Heuristic dominance

Suppose {{< katex >}} h_1 {{< /katex >}} and {{< katex >}} h_2 {{< /katex >}} are admissible and:

{{% colour "green" %}}
{{< katex display=true >}}
h_2(n) \geq h_1(n)
{{< /katex >}}
{{% /colour %}}

for every node. Then {{< katex >}} h_2 {{< /katex >}} **dominates** {{< katex >}} h_1 {{< /katex >}} because it is closer to the true cost without overestimating. A* using {{< katex >}} h_2 {{< /katex >}} will usually expand no more nodes than A* using {{< katex >}} h_1 {{< /katex >}}.

If {{< katex >}} h(n)=0 {{< /katex >}} for every node, A* behaves like UCS.

## 7. Effective Branching Factor

The **effective branching factor** measures how strongly a heuristic reduces the apparent width of a search tree.

If A* generates {{< katex >}} N+1 {{< /katex >}} nodes to find a solution at depth {{< katex >}} d {{< /katex >}}, then {{< katex >}} b^{*} {{< /katex >}} satisfies:

{{% colour "green" %}}
{{< katex display=true >}}
N+1 = 1+b^{*}+(b^{*})^2+\cdots +(b^{*})^d
{{< /katex >}}
{{% /colour %}}

A smaller {{< katex >}} b^{*} {{< /katex >}} indicates better guidance and fewer generated nodes.

## 8. Heuristics from Relaxed Problems ☆

A **relaxed problem** removes one or more constraints from the original problem, making it easier to solve.

The optimal cost of the relaxed problem is a lower bound on the original cost because removing restrictions cannot make the problem harder. It can therefore provide an admissible heuristic.

### 8-puzzle example

| Relaxation | Resulting heuristic |
|---|---|
| A tile may move to any position | Number of misplaced tiles |
| Tiles may move through one another | Sum of Manhattan distances |

Manhattan distance is usually more informative because it accounts for how far each tile is from its goal position.

{{% hint success %}}
Relax the rules, solve the easier problem, and use that solution cost as a lower-bound estimate for the original problem.
{{% /hint %}}

## 9. Pattern Databases ☆

A **pattern database** stores exact solution costs for abstracted subproblems.

For a sliding-tile puzzle, it may precompute the optimal cost for configurations involving only a selected group of tiles. During search, the algorithm looks up the matching abstract pattern and uses its stored cost as a heuristic.

Pattern databases exchange preparation time and storage for faster search later.

## 10. Landmarks

A landmark heuristic precomputes distances involving selected important states. In a road network, landmarks might be major cities or interchanges. These known distances help estimate the cost between a current location and a destination without solving every route from scratch.

## 11. Learning Heuristics from Experience

Instead of designing every heuristic manually, a model can learn to predict remaining cost from previously solved problems.

Examples include:

- learning which features predict expensive or unproductive states
- training a neural network to produce {{< katex >}} h(s;\theta) {{< /katex >}}
- using learned estimates inside A* or GBFS

For grid-based problems such as Sokoban, mazes with teleports and sliding-tile puzzles, a neural network can process the grid and output one predicted heuristic value.

{{% hint warning %}}
A learned heuristic may guide search well without being admissible. Prediction accuracy and the guarantees required by A* are separate considerations.
{{% /hint %}}

## Common Mistakes

{{% hint warning %}}
- {{< katex >}} h(n) {{< /katex >}} estimates the cost from the current node to the goal; {{< katex >}} g(n) {{< /katex >}} is the cost already paid.
- GBFS uses only {{< katex >}} h(n) {{< /katex >}}; A* uses both {{< katex >}} g(n) {{< /katex >}} and {{< katex >}} h(n) {{< /katex >}}.
- An admissible heuristic need not be exact; it only must not overestimate.
- The most accurate heuristic is not automatically best if it is extremely expensive to compute.
- A relaxed problem removes constraints; it does not add new restrictions.
{{% /hint %}}

## Practice Questions

1. Explain the different roles of {{< katex >}} g(n) {{< /katex >}}, {{< katex >}} h(n) {{< /katex >}} and {{< katex >}} f(n) {{< /katex >}} in A*.
2. Why can GBFS reach a non-optimal solution?
3. Test whether a given heuristic is admissible.
4. Test consistency across an edge with cost {{< katex >}} c(n,a,n') {{< /katex >}}.
5. Why does a relaxed problem produce a lower bound?
6. Compare misplaced tiles with Manhattan distance for the 8-puzzle.
7. What does a pattern database store?
8. Why can a more informative heuristic reduce the effective branching factor?

## Key Takeaways

{{% hint success %}}
- GBFS chooses by estimated remaining cost; A* combines actual and estimated cost.
- Admissibility prevents overestimation, while consistency constrains estimates across neighbouring states.
- A more informative admissible heuristic normally allows A* to expand fewer nodes.
- Relaxed problems, pattern databases, landmarks and experience can all produce useful heuristics.
- Learned heuristics can improve guidance, but their optimality guarantees must be checked separately.
{{% /hint %}}

## Checklist

- [ ] I can calculate {{< katex >}} f(n)=g(n)+h(n) {{< /katex >}}.
- [ ] I can distinguish GBFS, UCS and A*.
- [ ] I can test admissibility and consistency.
- [ ] I can explain heuristic dominance and effective branching factor.
- [ ] I can derive a heuristic from a relaxed problem.
- [ ] I can explain pattern databases and learned heuristics.

---
{{< home-link "Home" >}} | {{< section-index >}}
