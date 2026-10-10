---
title: "Knowledge Representation and Propositional Logic"
draft: false
tags: ["AI", "ACI", "Knowledge Representation", "Knowledge-Based Agents", "Propositional Logic", "Wumpus World", "Entailment", "CNF", "Resolution", "DPLL"]
categories: ["AI", "Artificial and Computational Intelligence"]
weight: 800
menu: main
---

# Knowledge Representation and Propositional Logic

An intelligent agent can store observations and rules, combine them through logical reasoning, and use the resulting conclusions to choose an action. Knowledge representation makes this information explicit so that the agent can reuse what it knows and reason about situations it has not directly observed.

**Module topics**

- Overview of Logics- Propositional, Predicate, TT-Entail, Theorem Proving
- Logic Representation of a sample agent, Proof by resolution, DPLL Algorithm, Agents based on Propositional logic
- Unification, forward chaining, Backward Chaining, Resolution

The explanations below develop knowledge-based agents and propositional reasoning, using one continuous Wumpus World example.

{{% hint info %}}
In modern AI, knowledge representation and logical reasoning are used when a system must follow explicit rules, satisfy constraints, or justify its conclusions. For example, a planning system can represent action preconditions and effects, then check whether a proposed sequence of actions achieves a goal. A neural model can recognise objects or suggest solutions, while a symbolic reasoning component checks what follows from the available facts and rules. Google DeepMind’s AlphaGeometry illustrates this combination: a neural language model proposes useful geometric constructions, and a symbolic deduction engine develops proofs. Its reasoning uses richer mathematical representations than basic propositional logic, but follows the same principle of deriving conclusions from explicit knowledge. These methods are especially useful when correctness and traceability matter; their conclusions still depend on the accuracy of the facts and rules supplied.
{{% /hint %}}

{{< mermaid >}}
flowchart TD
  L["Logic"] --> PL["Propositional<br/>Logic"]
  L --> PDL["Predicate Logic<br/>(First-Order Logic)"]

  PL --> PROP["Propositions<br/>True or False"]
  PL --> INF["Inference"]

  INF --> TT["Truth Table<br/>Entailment"]
  INF --> TP["Theorem<br/>Proving"]

  TP --> PC["Proof by<br/>Contradiction"]
  TP --> RES["Resolution<br/>using CNF"]

  PDL --> OBJ["Objects and<br/>Predicates"]
  PDL --> VAR["Variables"]
  PDL --> Q["Quantifiers<br/>forall / exists"]

  style L fill:#90CAF9,stroke:#1E88E5,color:#000

  style PL fill:#CE93D8,stroke:#8E24AA,color:#000
  style PDL fill:#CE93D8,stroke:#8E24AA,color:#000
  style INF fill:#CE93D8,stroke:#8E24AA,color:#000

  style PROP fill:#C8E6C9,stroke:#2E7D32,color:#000
  style TT fill:#C8E6C9,stroke:#2E7D32,color:#000
  style TP fill:#C8E6C9,stroke:#2E7D32,color:#000
  style PC fill:#C8E6C9,stroke:#2E7D32,color:#000
  style RES fill:#C8E6C9,stroke:#2E7D32,color:#000
  style OBJ fill:#C8E6C9,stroke:#2E7D32,color:#000
  style VAR fill:#C8E6C9,stroke:#2E7D32,color:#000
  style Q fill:#C8E6C9,stroke:#2E7D32,color:#000
{{< /mermaid >}}

## Learning Objectives

- distinguish facts, rules, queries and inferred conclusions
- explain how TELL and ASK support a knowledge-based agent
- interpret propositional symbols, connectives and truth assignments
- formulate rules for breeze, stench and safe locations
- combine successive observations to infer hazards before entering a square
- test entailment using the models that satisfy a knowledge base
- apply equivalence laws and inference rules to prove a query
- convert propositional sentences into conjunctive normal form
- prove entailment through resolution refutation
- distinguish resolution from DPLL satisfiability checking

## Big Picture

{{< mermaid >}}
flowchart TD
    P["Current percept"] --> T["TELL: add facts"]
    R["Rules about the world"] --> K["Knowledge base"]
    T --> K
    K --> I["Logical inference"]
    Q["ASK: action query"] --> I
    I --> A["Select and record action"]
    A --> E["Execute action"]
    E --> P

    style P fill:#E1F5FE
    style T fill:#C8E6C9
    style R fill:#EDE7F6
    style K fill:#E1F5FE
    style I fill:#FFF9C4
    style Q fill:#EDE7F6
    style A fill:#C8E6C9
    style E fill:#E1F5FE
{{< /mermaid >}}

## 1. Knowledge, Facts and Rules

A **knowledge base**, usually written as KB, is a collection of sentences expressed in a knowledge representation language. These sentences describe what the agent knows about the world and the relationships it can use to derive further information.

| Component | Meaning | Example |
|---|---|---|
| Fact | A statement available to the agent | No breeze is detected at location (1,1). |
| Rule | A relationship between statements | A breeze occurs when at least one adjacent square contains a pit. |
| Query | A question expressed as a logical sentence | Is location (1,2) pit-free? |
| Derived fact | A conclusion obtained from existing sentences | Location (1,2) contains no pit. |

Some sentences are accepted as starting assumptions or **axioms**. Other sentences are obtained through observation or inferred from earlier knowledge.

### Why store knowledge?

Suppose an agent has found and verified a shortest route between two locations. Storing the route avoids repeating the same search when the environment is unchanged. If a new road appears, the agent must update its knowledge before relying on the stored route.

A knowledge base supports more than retrieval. It also allows the agent to combine facts and rules to answer a question whose answer was never stored directly.

{{% hint info %}}
The agent may know that no breeze is present in its current square without having visited the neighbouring squares. A rule connecting breeze to pits lets it infer something about those unvisited squares.
{{% /hint %}}

## 2. Knowledge-Based Agents: TELL and ASK ☆

A knowledge-based agent combines perception, stored knowledge, inference and action.

### TELL

TELL adds a sentence to the knowledge base. The sentence might describe a newly observed percept, an initial condition or an action the agent has selected.

### ASK

ASK queries the knowledge base. The inference procedure determines what follows from the stored sentences and uses that information to support a decision.

### Agent cycle

1. Receive a percept from the environment.
2. TELL the knowledge base what was perceived.
3. ASK which action should be performed.
4. TELL the knowledge base which action was selected.
5. Execute the action and receive the next percept.

When facts can change over time, their time or state must be represented appropriately. Recording that an action was selected does not by itself establish that every intended outcome occurred.

## 3. Propositional Logic: Language and Meaning

**Propositional logic** represents statements that are either true or false. Compound sentences are built by joining simpler statements with logical connectives.

{{< mermaid >}}
flowchart TD
    PL["Propositional Logic"] --> INF["Inference"]
    INF --> TT["Truth Table<br/>Entailment"]
    INF --> TP["Theorem<br/>Proving"]

    TP --> PC["Proof by<br/>Contradiction"]
    TP --> RES["Resolution<br/>using CNF"]

style PL fill:#90CAF9,stroke:#1E88E5,color:#000
style INF fill:#CE93D8,stroke:#8E24AA,color:#000
style TT fill:#C8E6C9,stroke:#2E7D32,color:#000
style TP fill:#C8E6C9,stroke:#2E7D32,color:#000
style PC fill:#C8E6C9,stroke:#2E7D32,color:#000
style RES fill:#C8E6C9,stroke:#2E7D32,color:#000

{{< /mermaid >}}

### Basic terminology

| Term | Meaning | Example |
|---|---|---|
| Proposition | A statement with a truth value | There is a pit at location (1,2). |
| Symbol or atom | A name for an atomic proposition | {{< katex >}} P_{1,2} {{< /katex >}} |
| Literal | An atom or its negation | {{< katex >}} P_{1,2} {{< /katex >}} or {{< katex >}} \neg P_{1,2} {{< /katex >}} |
| Compound sentence | A sentence containing connectives | {{< katex >}} P_{1,2}\lor P_{2,1} {{< /katex >}} |
| Clause | A disjunction of literals | {{< katex >}} \neg B_{1,1}\lor P_{1,2}\lor P_{2,1} {{< /katex >}} |
| Model | A complete truth assignment to the relevant symbols | Assign each symbol True or False. |

A unit clause contains one literal, such as {{< katex >}} \neg P_{1,1} {{< /katex >}}.

A question such as “Where is the pit?” is not itself a proposition. To query an agent, formulate a sentence whose truth can be tested, such as “There is no pit at (1,2).”

### Syntax and semantics

- **Syntax** specifies which expressions are well formed.
- **Semantics** specifies when an expression is true in a model.

The sentence {{< katex >}} P_{1,2}\lor P_{2,1} {{< /katex >}} is true in a model where either symbol is true, including a model where both are true.

## 4. Logical Connectives ☆

| Connective | Symbol | Meaning |
|---|---|---|
| Negation | {{< katex >}} \neg P {{< /katex >}} | P is false. |
| Conjunction | {{< katex >}} P\land Q {{< /katex >}} | Both P and Q are true. |
| Disjunction | {{< katex >}} P\lor Q {{< /katex >}} | At least one of P and Q is true. |
| Implication | {{< katex >}} P\Rightarrow Q {{< /katex >}} | Whenever P is true, Q must be true. |
| Biconditional | {{< katex >}} P\Leftrightarrow Q {{< /katex >}} | P and Q have the same truth value. |

### Truth table

| P | Q | {{< katex >}} \neg P {{< /katex >}} | {{< katex >}} P\land Q {{< /katex >}} | {{< katex >}} P\lor Q {{< /katex >}} | {{< katex >}} P\Rightarrow Q {{< /katex >}} | {{< katex >}} P\Leftrightarrow Q {{< /katex >}} |
|---|---|---|---|---|---|---|
| True | True | False | True | True | True | True |
| True | False | False | False | True | False | False |
| False | True | True | False | True | True | False |
| False | False | True | False | False | True | True |

### Understanding implication

The implication {{< katex >}} P\Rightarrow Q {{< /katex >}} is false only when P is true and Q is false. It places a requirement on cases where P holds.

For example, “If there is a pit here, neighbouring squares have breeze” does not say that the absence of this particular pit removes every possible cause of breeze.

### Precedence

The usual order is negation, conjunction, disjunction, implication, then biconditional. Use parentheses whenever the intended grouping could be unclear.

## 5. Wumpus World: A Logical Agent's Environment

Wumpus World is a four-by-four grid containing pits, a Wumpus and gold. The agent starts safely at (1,1), initially facing right. The locations of the hazards and gold are hidden from the agent.

The objective is to obtain the gold and leave through the starting square while avoiding death and unnecessary actions.

### Example layout

The layout below is used in the worked example. The x-coordinate increases to the right and the y-coordinate increases upwards. The agent discovers the hidden locations through local percepts.

| y / x | 1 | 2 | 3 | 4 |
|---|---|---|---|---|
| 4 | - | - | - | Pit |
| 3 | Wumpus | Gold | Pit | - |
| 2 | - | - | - | - |
| 1 | Agent/start | - | Pit | - |

### PEAS description

| Component | Description |
|---|---|
| Performance measure | +1000 for leaving with gold; -1000 for death; -1 per action; -10 for using the arrow |
| Environment | A four-by-four grid with walls, pits, a Wumpus and gold |
| Actuators | Movement, turning, grabbing gold, shooting an arrow and climbing out |
| Sensors | Percepts for stench, breeze, glitter, bump and scream |

### Actions

| Action | Effect |
|---|---|
| Forward | Move one square in the direction currently faced, unless blocked by a wall. |
| TurnLeft | Rotate 90 degrees left without changing location. |
| TurnRight | Rotate 90 degrees right without changing location. |
| Grab | Collect gold from the current square. |
| Shoot | Fire the single available arrow in the direction faced. |
| Climb | Leave the world from location (1,1). |

For example, an agent at (2,1) facing east must turn left and then move forward to reach (2,2). Turning alone changes orientation, not position.

### Percepts

| Percept | Meaning |
|---|---|
| Stench | A Wumpus is in an orthogonally adjacent square in the model used here. |
| Breeze | At least one orthogonally adjacent square contains a pit. |
| Glitter | Gold is in the current square. |
| Bump | A forward movement encountered a boundary wall. |
| Scream | The Wumpus has been killed; the sound is heard throughout the world. |

Diagonal squares are not adjacent for these warning rules. The safety deductions below concern the world before the Wumpus is killed.

### Task environment

| Dimension | Classification | Reason |
|---|---|---|
| Observability | Partially observable | Sensors provide local clues rather than the complete map. |
| Determinism | Deterministic | A given state and action have a fixed resulting state. |
| Episodic or sequential | Sequential | Earlier actions and observations affect later decisions. |
| Static or dynamic | Static | The world does not change independently while the agent reasons. |
| Discrete or continuous | Discrete | Locations, orientations, actions and percepts are discrete. |
| Number of agents | Single agent | The Wumpus is not modelled as a strategic opponent. |

Random placement at the beginning does not make the subsequent action model stochastic. A deterministic transition can still have an outcome the agent cannot predict fully because it does not know the complete state.

## 6. Representing Wumpus World with Logic ☆

### Symbols

| Symbol | Interpretation |
|---|---|
| {{< katex >}} P_{x,y} {{< /katex >}} | A pit exists at (x,y). |
| {{< katex >}} W_{x,y} {{< /katex >}} | The Wumpus exists at (x,y). |
| {{< katex >}} B_{x,y} {{< /katex >}} | Breeze is perceived at (x,y). |
| {{< katex >}} S_{x,y} {{< /katex >}} | Stench is perceived at (x,y). |

Gold, glitter, visited locations and safe locations can also be recorded. The deductions here concentrate on pits and the Wumpus.

### Neighbourhood rules

Let {{< katex >}} N(x,y) {{< /katex >}} be the set of valid orthogonally adjacent squares. Exclude coordinates outside the grid.

| Square | Valid neighbours |
|---|---|
| (1,1) | (1,2), (2,1) |
| (2,1) | (1,1), (2,2), (3,1) |
| (2,2) | (1,2), (2,1), (2,3), (3,2) |

A breeze is present exactly when at least one neighbouring square contains a pit:

{{% colour "green" %}}
{{< katex display=true >}}
B_{x,y}\Leftrightarrow\bigvee_{(u,v)\in N(x,y)}P_{u,v}
{{< /katex >}}
{{% /colour %}}

The corresponding Wumpus rule is:

{{% colour "green" %}}
{{< katex display=true >}}
S_{x,y}\Leftrightarrow\bigvee_{(u,v)\in N(x,y)}W_{u,v}
{{< /katex >}}
{{% /colour %}}

These expressions are templates for generating propositional sentences for each valid square.

### Positive and negative observations

If breeze is present, at least one neighbouring square has a pit. If breeze is absent, every neighbouring square is pit-free:

{{% colour "green" %}}
{{< katex display=true >}}
\neg B_{x,y}\Rightarrow\bigwedge_{(u,v)\in N(x,y)}\neg P_{u,v}
{{< /katex >}}
{{% /colour %}}

Similarly:

{{% colour "green" %}}
{{< katex display=true >}}
\neg S_{x,y}\Rightarrow\bigwedge_{(u,v)\in N(x,y)}\neg W_{u,v}
{{< /katex >}}
{{% /colour %}}

A pit causes breeze in its valid neighbouring squares. However, another pit may also cause breeze in those squares. Establishing that one square is pit-free therefore does not establish that its neighbours have no breeze.

Absence of breeze describes neighbouring pits, not whether the current square contains a pit. The safety of the starting square is supplied as an initial condition.

### Safe and visited are different

Before the Wumpus is killed, a location is safe when it contains neither a pit nor the Wumpus:

{{% colour "green" %}}
{{< katex display=true >}}
Safe_{x,y}\Leftrightarrow(\neg P_{x,y}\land\neg W_{x,y})
{{< /katex >}}
{{% /colour %}}

A square can be known to be safe before it has been visited. A safe square may still contain breeze or stench because these warn about adjacent hazards.

## 7. Worked Wumpus Deductions ☆

Use only the observations available to the agent at each stage. A complete diagram may reveal the true world to the reader, but those hidden locations are not automatically facts in the agent's KB.

### Observation 1: no breeze and no stench at (1,1)

The valid neighbours are (1,2) and (2,1).

{{% colour "green" %}}
{{< katex display=true >}}
\neg B_{1,1}\Rightarrow(\neg P_{1,2}\land\neg P_{2,1})
{{< /katex >}}
{{% /colour %}}

{{% colour "green" %}}
{{< katex display=true >}}
\neg S_{1,1}\Rightarrow(\neg W_{1,2}\land\neg W_{2,1})
{{< /katex >}}
{{% /colour %}}

Both neighbours are safe. The agent chooses to visit (2,1).

### Observation 2: breeze and no stench at (2,1)

Breeze gives three possible pit locations:

{{% colour "green" %}}
{{< katex display=true >}}
B_{2,1}\Rightarrow(P_{1,1}\lor P_{2,2}\lor P_{3,1})
{{< /katex >}}
{{% /colour %}}

The starting square is already known to be pit-free. Combining that fact with the disjunction gives:

{{% colour "green" %}}
{{< katex display=true >}}
P_{2,2}\lor P_{3,1}
{{< /katex >}}
{{% /colour %}}

At least one candidate contains a pit; both remain possible. The agent cannot yet identify which candidate is pit-free.

No stench establishes:

{{% colour "green" %}}
{{< katex display=true >}}
\neg W_{1,1}\land\neg W_{2,2}\land\neg W_{3,1}
{{< /katex >}}
{{% /colour %}}

The agent can return to (1,1) and explore the known safe square (1,2). Backtracking provides new information without entering an unresolved hazard location.

### Observation 3: stench and no breeze at (1,2)

No breeze establishes:

{{% colour "green" %}}
{{< katex display=true >}}
\neg P_{1,1}\land\neg P_{1,3}\land\neg P_{2,2}
{{< /katex >}}
{{% /colour %}}

Combining {{< katex >}} \neg P_{2,2} {{< /katex >}} with the earlier {{< katex >}} P_{2,2}\lor P_{3,1} {{< /katex >}} identifies a pit at (3,1).

Stench gives:

{{% colour "green" %}}
{{< katex display=true >}}
W_{1,1}\lor W_{1,3}\lor W_{2,2}
{{< /katex >}}
{{% /colour %}}

Earlier knowledge excludes a Wumpus at (1,1) and (2,2), leaving:

{{% colour "green" %}}
{{< katex display=true >}}
W_{1,3}
{{< /katex >}}
{{% /colour %}}

Location (2,2) is now known to be both pit-free and Wumpus-free.

### Observation 4: no breeze and no stench at (2,2)

Its neighbours are (1,2), (2,1), (2,3) and (3,2). The first two are visited; the last two are unvisited.

The negative percepts establish that (2,3) and (3,2) are safe. In this world, visiting (2,3) produces glitter, allowing the agent to grab the gold and return through known safe squares to climb out at (1,1).

### What the agent has achieved

| Location | Conclusion | Supporting information |
|---|---|---|
| (1,2) | Safe before visiting | No breeze and no stench at (1,1) |
| (2,1) | Safe before visiting | No breeze and no stench at (1,1) |
| (2,2) | Safe before visiting | No breeze at (1,2), no stench at (2,1) |
| (3,1) | Contains a pit | Breeze at (2,1), exclusions at (1,1) and (2,2) |
| (1,3) | Contains the Wumpus | Stench at (1,2), exclusions at (1,1) and (2,2) |

{{% hint success %}}
Combine current percepts with earlier facts. A conclusion about an unvisited square can become certain even when no single observation establishes it alone.
{{% /hint %}}

## 8. Entailment and Truth-Table Inference ☆

Entailment asks whether a conclusion must be true whenever the knowledge base is true.

{{% colour "green" %}}
{{< katex display=true >}}
KB\models q
\quad\Longleftrightarrow\quad
M(KB)\subseteq M(q)
{{< /katex >}}
{{% /colour %}}

Here, {{< katex >}} M(KB) {{< /katex >}} is the set of models satisfying every sentence in the KB.

### A five-sentence knowledge base

Consider a KB restricted to information associated with the first two observations:

{{% colour "green" %}}
{{< katex display=true >}}
\begin{aligned}
R_1 &: \neg P_{1,1}\\
R_2 &: B_{1,1}\Leftrightarrow(P_{1,2}\lor P_{2,1})\\
R_3 &: B_{2,1}\Leftrightarrow(P_{1,1}\lor P_{2,2}\lor P_{3,1})\\
R_4 &: \neg B_{1,1}\\
R_5 &: B_{2,1}
\end{aligned}
{{< /katex >}}
{{% /colour %}}

The query is:

{{% colour "green" %}}
{{< katex display=true >}}
q=\neg P_{1,2}
{{< /katex >}}
{{% /colour %}}

### TT-Entail procedure

1. Collect the distinct symbols appearing in the KB and query.
2. Enumerate their possible True/False assignments.
3. Evaluate every KB sentence under each assignment.
4. Keep only assignments where all KB sentences are true.
5. Check whether the query is true in every retained model.

For {{< katex >}} n {{< /katex >}} distinct symbols:

{{% colour "green" %}}
{{< katex display=true >}}
\text{Number of truth assignments}=2^n
{{< /katex >}}
{{% /colour %}}

This KB contains seven distinct symbols, so there are {{< katex >}} 2^7=128 {{< /katex >}} assignments.

### The three satisfying models

Every satisfying model has:

{{% colour "green" %}}
{{< katex display=true >}}
B_{1,1}=F,\quad B_{2,1}=T,\quad
P_{1,1}=P_{1,2}=P_{2,1}=F
{{< /katex >}}
{{% /colour %}}

The remaining choices are:

| Model | {{< katex >}} P_{2,2} {{< /katex >}} | {{< katex >}} P_{3,1} {{< /katex >}} | {{< katex >}} \neg P_{1,2} {{< /katex >}} |
|---|---|---|---|
| 1 | False | True | True |
| 2 | True | False | True |
| 3 | True | True | True |

The query is true in all three, so:

{{% colour "green" %}}
{{< katex display=true >}}
KB\models\neg P_{1,2}
{{< /katex >}}
{{% /colour %}}

The same KB entails {{< katex >}} \neg P_{2,1} {{< /katex >}} and {{< katex >}} P_{2,2}\lor P_{3,1} {{< /katex >}}.

### True, false and undetermined

For a consistent KB:

| Query across KB-satisfying models | Conclusion |
|---|---|
| True in every model | The KB entails the query. |
| False in every model | The KB entails the query's negation. |
| True in some and false in others | The query is undetermined by the KB. |

In the five-sentence KB, {{< katex >}} P_{2,2} {{< /katex >}} is undetermined. The later no-breeze observation at (1,2) supplies additional information and eliminates models containing that pit.

Truth in two of three models does not establish a probability of two-thirds. Logical entailment does not assign probabilities to models.

### Computational cost

The assignment count grows exponentially. Five symbols produce 32 assignments, ten produce 1024, and twenty produce 1,048,576.

If evaluating the KB costs {{< katex >}} O(m) {{< /katex >}} for sentence size {{< katex >}} m {{< /katex >}}, straightforward enumeration takes {{< katex >}} O(m2^n) {{< /katex >}} time. The commonly stated {{< katex >}} O(2^n) {{< /katex >}} highlights the exponential dependence on symbol count.

A depth-first implementation can evaluate one assignment at a time using {{< katex >}} O(n) {{< /katex >}} additional assignment/recursion space, excluding the stored input. Storing a complete truth table requires much more space.

## 9. Theorem Proving and Inference Rules ☆

Theorem proving derives a conclusion by applying sound rules to existing sentences. A proof is a sequence of justified steps, each based on what is already available.

### Useful logical equivalences

| Law | Equivalence |
|---|---|
| Implication elimination | {{< katex >}} A\Rightarrow B\equiv\neg A\lor B {{< /katex >}} |
| Biconditional elimination | {{< katex >}} A\Leftrightarrow B\equiv(A\Rightarrow B)\land(B\Rightarrow A) {{< /katex >}} |
| Double negation | {{< katex >}} \neg\neg A\equiv A {{< /katex >}} |
| De Morgan: negated OR | {{< katex >}} \neg(A\lor B)\equiv\neg A\land\neg B {{< /katex >}} |
| De Morgan: negated AND | {{< katex >}} \neg(A\land B)\equiv\neg A\lor\neg B {{< /katex >}} |
| Contraposition | {{< katex >}} A\Rightarrow B\equiv\neg B\Rightarrow\neg A {{< /katex >}} |
| Distribution of OR over AND | {{< katex >}} A\lor(B\land C)\equiv(A\lor B)\land(A\lor C) {{< /katex >}} |

Commutativity allows the order of AND/OR operands to change. Associativity allows regrouping of consecutive ANDs or consecutive ORs. Neither permits exchanging AND with OR.

### Modus ponens

If a premise is known to be true and a rule says that it implies a conclusion, infer that conclusion:

{{% colour "green" %}}
{{< katex display=true >}}
\frac{A,\quad A\Rightarrow B}{B}
{{< /katex >}}
{{% /colour %}}

### AND elimination

If a conjunction is true, each of its components is true:

{{% colour "green" %}}
{{< katex display=true >}}
\frac{A\land B}{A}
\qquad
\frac{A\land B}{B}
{{< /katex >}}
{{% /colour %}}

### Direct proof of the Wumpus query

Start with {{< katex >}} R_2 {{< /katex >}} because it relates the query's pit symbol to an observed breeze symbol. Split the biconditional into its two implications, then select the direction connecting the possible pits to breeze.

| Step | Derived sentence | Reason |
|---|---|---|
| 1 | {{< katex >}} (P_{1,2}\lor P_{2,1})\Rightarrow B_{1,1} {{< /katex >}} | Biconditional and AND elimination |
| 2 | {{< katex >}} \neg B_{1,1}\Rightarrow\neg(P_{1,2}\lor P_{2,1}) {{< /katex >}} | Contraposition |
| 3 | {{< katex >}} \neg(P_{1,2}\lor P_{2,1}) {{< /katex >}} | Modus ponens with R4 |
| 4 | {{< katex >}} \neg P_{1,2}\land\neg P_{2,1} {{< /katex >}} | De Morgan's law |
| 5 | {{< katex >}} \neg P_{1,2} {{< /katex >}} | AND elimination |

The query follows without constructing 128 truth-table rows.

### Choosing a useful next step

Look for a sentence containing the query symbol and connect it to facts already known. Here, the useful known fact is no breeze at (1,1). The biconditional provides a relationship in both directions; contraposition then produces a rule whose premise matches that negative fact.

Rule selection is a search problem. Applying a sound rule preserves validity, but applying irrelevant rules may not move the proof towards its goal.

## 10. Conjunctive Normal Form ☆

**Conjunctive normal form**, or CNF, is an AND of clauses, where each clause is an OR of literals.

{{% colour "green" %}}
{{< katex display=true >}}
(A\lor\neg B)\land(A\lor B\lor\neg C)\land\neg A
{{< /katex >}}
{{% /colour %}}

This expression contains three clauses. The last clause is a unit clause.

### Conversion procedure

1. Eliminate biconditionals.
2. Replace implications using implication elimination.
3. Push negations inward using De Morgan's laws and double negation.
4. Distribute OR over AND.
5. Separate the resulting conjunction into clauses.

### Worked conversion of R2

Start with:

{{% colour "green" %}}
{{< katex display=true >}}
B_{1,1}\Leftrightarrow(P_{1,2}\lor P_{2,1})
{{< /katex >}}
{{% /colour %}}

Eliminate the biconditional:

{{% colour "green" %}}
{{< katex display=true >}}
\begin{aligned}
&(B_{1,1}\Rightarrow(P_{1,2}\lor P_{2,1}))\\
&\land((P_{1,2}\lor P_{2,1})\Rightarrow B_{1,1})
\end{aligned}
{{< /katex >}}
{{% /colour %}}

Eliminate implications:

{{% colour "green" %}}
{{< katex display=true >}}
\begin{aligned}
&(\neg B_{1,1}\lor P_{1,2}\lor P_{2,1})\\
&\land(\neg(P_{1,2}\lor P_{2,1})\lor B_{1,1})
\end{aligned}
{{< /katex >}}
{{% /colour %}}

Apply De Morgan's law to the second part, then distribute OR over AND:

{{% colour "green" %}}
{{< katex display=true >}}
\begin{aligned}
&(\neg B_{1,1}\lor P_{1,2}\lor P_{2,1})\\
&\land(\neg P_{1,2}\lor B_{1,1})\\
&\land(\neg P_{2,1}\lor B_{1,1})
\end{aligned}
{{< /katex >}}
{{% /colour %}}

### Complete clause set for the five-sentence KB

| Clause | Sentence |
|---|---|
| C1 | {{< katex >}} \neg P_{1,1} {{< /katex >}} |
| C2 | {{< katex >}} \neg B_{1,1} {{< /katex >}} |
| C3 | {{< katex >}} B_{2,1} {{< /katex >}} |
| C4 | {{< katex >}} \neg B_{1,1}\lor P_{1,2}\lor P_{2,1} {{< /katex >}} |
| C5 | {{< katex >}} \neg P_{1,2}\lor B_{1,1} {{< /katex >}} |
| C6 | {{< katex >}} \neg P_{2,1}\lor B_{1,1} {{< /katex >}} |
| C7 | {{< katex >}} \neg B_{2,1}\lor P_{1,1}\lor P_{2,2}\lor P_{3,1} {{< /katex >}} |
| C8 | {{< katex >}} \neg P_{1,1}\lor B_{2,1} {{< /katex >}} |
| C9 | {{< katex >}} \neg P_{2,2}\lor B_{2,1} {{< /katex >}} |
| C10 | {{< katex >}} \neg P_{3,1}\lor B_{2,1} {{< /katex >}} |

The clauses are jointly required: the KB is their conjunction.

## 11. Proof by Contradiction and Resolution ☆

Proof by contradiction tests whether the knowledge base can remain true when the query is assumed false.

{{% colour "green" %}}
{{< katex display=true >}}
KB\models q
\quad\Longleftrightarrow\quad
KB\land\neg q\text{ is unsatisfiable}
{{< /katex >}}
{{% /colour %}}

The extra sentence is a temporary assumption for the proof. It is not a new observation about the environment.

### Resolution rule

Resolution combines two clauses containing complementary literals:

{{% colour "green" %}}
{{< katex display=true >}}
\frac{A\lor L,\quad B\lor\neg L}{A\lor B}
{{< /katex >}}
{{% /colour %}}

Here, A and B stand for the remaining disjunctions. The complementary literal is removed, and the remaining literals form the **resolvent**.

### Unit resolution

When one parent clause is a unit clause:

{{% colour "green" %}}
{{< katex display=true >}}
\frac{A\lor B,\quad\neg A}{B}
{{< /katex >}}
{{% /colour %}}

### Wumpus resolution proof

The query is {{< katex >}} \neg P_{1,2} {{< /katex >}}, so add its negation, {{< katex >}} P_{1,2} {{< /katex >}}, to the CNF clause set.

| Step | Parent clauses | Resolvent |
|---|---|---|
| 1 | {{< katex >}} \neg P_{1,2}\lor B_{1,1} {{< /katex >}} and {{< katex >}} \neg B_{1,1} {{< /katex >}} | {{< katex >}} \neg P_{1,2} {{< /katex >}} |
| 2 | {{< katex >}} \neg P_{1,2} {{< /katex >}} and assumed {{< katex >}} P_{1,2} {{< /katex >}} | {{< katex >}} \Box {{< /katex >}} |

The empty clause {{< katex >}} \Box {{< /katex >}} contains no literals and is always false. Deriving it establishes that the KB and negated query cannot all be satisfied together.

Therefore, the KB entails {{< katex >}} \neg P_{1,2} {{< /katex >}}.

### A second resolution example

Let:

{{% colour "green" %}}
{{< katex display=true >}}
KB=(A\lor\neg B)\land(A\lor B\lor\neg C)\land\neg A
{{< /katex >}}
{{% /colour %}}

Prove {{< katex >}} \neg C {{< /katex >}} by adding {{< katex >}} C {{< /katex >}}:

| Step | Parent clauses | Resolvent |
|---|---|---|
| 1 | {{< katex >}} A\lor B\lor\neg C {{< /katex >}} and {{< katex >}} C {{< /katex >}} | {{< katex >}} A\lor B {{< /katex >}} |
| 2 | {{< katex >}} A\lor B {{< /katex >}} and {{< katex >}} \neg A {{< /katex >}} | {{< katex >}} B {{< /katex >}} |
| 3 | {{< katex >}} A\lor\neg B {{< /katex >}} and {{< katex >}} B {{< /katex >}} | {{< katex >}} A {{< /katex >}} |
| 4 | {{< katex >}} A {{< /katex >}} and {{< katex >}} \neg A {{< /katex >}} | {{< katex >}} \Box {{< /katex >}} |

The negated query is inconsistent with the KB, so {{< katex >}} KB\models\neg C {{< /katex >}}.

### Interpreting the result

- Unsatisfiability of {{< katex >}} KB\land\neg q {{< /katex >}} establishes entailment.
- A satisfying model of {{< katex >}} KB\land\neg q {{< /katex >}} is a counterexample, establishing that q is not entailed.
- A counterexample does not establish that the KB entails the opposite query.
- Failure to find a contradiction during an unfinished search is not a conclusion about entailment.

## 12. DPLL: Satisfiability Checking

The **Davis-Putnam-Logemann-Loveland algorithm**, or DPLL, is a complete backtracking procedure for deciding whether a propositional CNF formula is satisfiable.

It searches over truth assignments and uses three improvements:

| Improvement | Purpose |
|---|---|
| Early termination | Stop when all clauses are satisfied or a clause is false under the current assignments. |
| Pure-symbol heuristic | Assign a symbol that occurs only positively or only negatively in the remaining clauses. |
| Unit-clause heuristic | Force the assignment needed to satisfy a clause with one remaining literal. |

DPLL can test entailment by checking satisfiability of {{< katex >}} KB\land\neg q {{< /katex >}}. An unsatisfiable result establishes {{< katex >}} KB\models q {{< /katex >}}.

Resolution derives clauses. DPLL principally searches for a model, using unit propagation and backtracking to reduce the work.

## 13. Consistency and a Recommendation Example

Logical reasoning depends on how the information is represented and whether the supplied facts can hold together.

### Representing a fixed customer

| Symbol | Statement |
|---|---|
| E | The customer frequently purchases electronics. |
| T | The customer is tech-savvy. |
| X | The customer makes expensive purchases. |
| L | The customer follows technology trends. |
| R | The customer receives recommendations for new electronic products. |

Suppose the rules are:

{{% colour "green" %}}
{{< katex display=true >}}
(E\Rightarrow T)\land(X\Rightarrow T)
\land(T\Rightarrow L)\land(L\Rightarrow R)
{{< /katex >}}
{{% /colour %}}

Their CNF is:

{{% colour "green" %}}
{{< katex display=true >}}
(\neg E\lor T)\land(\neg X\lor T)
\land(\neg T\lor L)\land(\neg L\lor R)
{{< /katex >}}
{{% /colour %}}

Given E, repeated modus ponens derives T, then L, then R. The customer receives recommendations under these rules.

One satisfying assignment to these four rules with E true is:

| E | T | X | L | R |
|---|---|---|---|---|
| True | True | False | True | True |

An assignment with X true also satisfies the rules. Tech-savviness alone does not determine whether this customer makes expensive purchases.

### Statements about a population

“Not all tech-savvy customers make expensive purchases” means that at least one tech-savvy customer does not make expensive purchases. It leaves open whether other tech-savvy customers make expensive purchases.

Representing this claim requires identifying customers explicitly or using a language with quantifiers. A proposition about one arbitrary customer does not automatically express a claim about the whole population.

### Checking for inconsistent facts

If E and {{< katex >}} \neg T {{< /katex >}} are both supplied, the rule {{< katex >}} E\Rightarrow T {{< /katex >}} derives T, producing a contradiction.

{{% colour "green" %}}
{{< katex display=true >}}
E,\quad \neg E\lor T,\quad\neg T
\ \vdash\ T,\neg T
\ \vdash\ \Box
{{< /katex >}}
{{% /colour %}}

The inconsistency exists independently of any recommendation query. In classical logic, an inconsistent KB has no satisfying models and formally entails every sentence. A useful decision system must therefore resolve the conflicting facts or assumptions before interpreting its conclusions.

Propositional reasoning establishes logical consequences. A numerical likelihood requires a probabilistic model in addition to these logical rules.

## 14. Comparing Propositional Inference Methods

| Method | Main approach | Useful feature | Main limitation |
|---|---|---|---|
| TT-Entail | Check all relevant truth assignments. | Clear definition of entailment | Exponential assignment count |
| Theorem proving | Apply inference rules to sentences. | A proof may use only relevant facts. | Finding useful proof steps |
| Resolution refutation | Derive a contradiction from CNF plus the negated query. | One general clause-inference rule | Clause combinations can grow rapidly. |
| DPLL | Search for a satisfying CNF assignment. | Pruning through propagation and backtracking | Exponential worst-case search |

All these approaches can be implemented in an automated reasoning system.

### Strengths and limits of propositional logic

Propositional logic is declarative: sentences describe what holds in the world. It supports negative information and alternatives, such as “a pit is in one of these two squares,” without requiring an immediate choice.

Its expressive power is limited. Separate symbols and sentences are needed for particular locations or individuals. General statements about all objects or some object motivate predicate logic and quantifiers.

## Practical Exploration

The following Python example checks the five-sentence Wumpus KB. Each iteration represents one complete truth assignment.

```python
from itertools import product

satisfying_models = []

for P11, B11, P12, P21, B21, P22, P31 in product(
    (False, True), repeat=7
):
    sentences = [
        not P11,
        B11 == (P12 or P21),
        B21 == (P11 or P22 or P31),
        not B11,
        B21,
    ]

    if all(sentences):
        satisfying_models.append({
            "P22": P22,
            "P31": P31,
            "query_not_P12": not P12,
        })

print("Satisfying models:", len(satisfying_models))
for model in satisfying_models:
    print(model)
```

The output contains three models, and the query is true in all three. This demonstrates the distinction between generating a candidate assignment and accepting it as a model of the KB.

## Practice Questions

1. What is the difference between a fact, a rule and an inferred conclusion?
2. An agent detects no breeze at (2,2). Which locations can it identify as pit-free?
3. How many truth assignments exist for five distinct propositional symbols?
4. Does the five-sentence Wumpus KB entail that there is a pit at (2,2)?
5. Convert {{< katex >}} A\Rightarrow(B\lor C) {{< /katex >}} into CNF.
6. Given {{< katex >}} A\lor B {{< /katex >}} and {{< katex >}} \neg A {{< /katex >}}, identify the resolvent.

### Short Answers

1. A fact is available information; a rule links statements; an inferred conclusion follows by applying rules to existing information.
2. The valid neighbours (1,2), (2,1), (2,3) and (3,2). The observation alone does not establish the current square's pit status.
3. {{< katex >}} 2^5=32 {{< /katex >}}.
4. No. Some KB-satisfying models contain that pit and others do not, so its presence is undetermined.
5. {{< katex >}} \neg A\lor B\lor C {{< /katex >}}, which is already a single CNF clause.
6. B, obtained by unit resolution.

## Key Takeaways

{{% hint success %}}
- A knowledge-based agent combines observations with rules to derive useful conclusions.
- Negative warning percepts can establish the safety of unvisited neighbouring squares.
- Entailment requires truth in every model satisfying the KB.
- An undetermined statement is not automatically false.
- CNF is a conjunction of disjunctions of literals.
- Resolution refutation proves a query by making its negation inconsistent with the KB.
- DPLL searches for satisfying assignments using propagation and backtracking.
{{% /hint %}}

## Checklist

- [ ] I can distinguish facts, rules, literals, clauses and models.
- [ ] I can explain the TELL/ASK agent cycle.
- [ ] I can formulate valid neighbouring-square rules.
- [ ] I can infer safe squares from successive percepts.
- [ ] I can distinguish entailment, negation entailment and uncertainty.
- [ ] I can convert a biconditional into CNF.
- [ ] I can show a resolution proof ending in an empty clause.
- [ ] I can explain the purpose and three main improvements of DPLL.

## References

- Stuart Russell and Peter Norvig, *Artificial Intelligence: A Modern Approach*, 4th edition, Chapters 7 and 8.
- UC Berkeley CS188, [Propositional Logic](https://inst.eecs.berkeley.edu/~cs188/textbook/logic/propositional.html) and [Propositional Logical Inference](https://inst.eecs.berkeley.edu/~cs188/textbook/logic/inference.html).

---

{{< home-link "Home" >}} | {{< section-index >}}