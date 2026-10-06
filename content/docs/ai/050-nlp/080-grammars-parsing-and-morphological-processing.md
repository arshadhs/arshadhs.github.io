---
title: "Grammars, Parsing, and Morphological Processing"
draft: false
tags: ["Natural Language Processing", "NLP", "Grammars", "Context-Free Grammar", "Parsing", "Chart Parsing", "Morphology"]
categories: ["AI", "ML"]
weight: 800
menu: main
---

# Grammars, Parsing, and Morphological Processing

Natural-language sentences are more than sequences of words. Their words form phrases, and the way those phrases are grouped determines the sentence's structure and meaning.

A grammar describes the structures that a language permits. A parser uses that grammar to discover the hidden structure of a particular sentence.

## Course Content

- Grammars and sentence structure
- What makes a good grammar
- A bottom-up chart parser
- Probabilistic Context-Free Grammars
- Probabilistic CKY parsing of PCFGs
- Ways to learn PCFG rule probabilities
- Problems with PCFGs
- Improving PCFGs by splitting non-terminals and using lexicalised CFGs

## Learning Objectives

- Explain why sentence structure is necessary for resolving ambiguity.
- Represent a grammar using terminals, non-terminals, productions, and a start symbol.
- Recognise the defining restriction of a Context-Free Grammar.
- Construct and interpret a simple parse tree.
- Compare top-down and bottom-up parsing.
- Explain how a chart prevents repeated parsing work.
- Distinguish active arcs, completed constituents, the agenda, and the chart.
- Explain morphological analysis and generation using finite-state models.

## Big Picture

{{< mermaid >}}
flowchart TD
    A["Sentence"] --> B["Grammar: allowed structures"]
    B --> C["Parser: search for structure"]
    C --> D["Parse tree"]
    D --> E["Interpretation"]

    style A fill:#E1F5FE
    style B fill:#C8E6C9
    style C fill:#FFF9C4
    style D fill:#EDE7F6
    style E fill:#E1F5FE
{{< /mermaid >}}

## 1. Why Sentence Structure Matters ☆

Knowing the meanings of individual words is not enough to determine the meaning of a complete sentence. We must also know which words belong together.

Consider:

```text
I saw the man with the telescope.
```

This sentence has at least two interpretations:

1. I used a telescope to see the man.
2. I saw a man who was carrying a telescope.

The words remain unchanged. The meaning changes because the prepositional phrase `with the telescope` attaches to a different part of the sentence.

{{% hint info %}}
Parsing makes hidden grouping visible. It answers questions such as: Which words form a phrase? What does that phrase modify? How are the phrases related?
{{% /hint %}}

### Structural ambiguity can grow rapidly

Adding modifiers can produce many more possible structures:

| Sentence pattern | Possible parses shown in the source |
|---|---:|
| I saw the man with the telescope | 2 |
| I saw the man on the hill with the telescope | 5 |
| I saw the man on the hill in Texas with the telescope | 14 |
| I saw the man on the hill in Texas with the telescope at noon | 42 |
| I saw the man on the hill in Texas with the telescope at noon on Monday | 132 |

This is why ambiguity is described as **explosive**: each additional phrase may have several possible attachment points.

Ambiguity may also depend on word meaning. In the sentence below, `bright` could mean intelligent or full of light:

```text
Why is the teacher wearing sunglasses?
Because the class is so bright.
```

## 2. Grammar and Parsing ☆

### Grammar

A **grammar** is a formal specification of the structures allowed in a language. It acts like a rule book: it describes how smaller units can combine to form larger ones.

### Parsing

**Parsing** is the process of analysing a sentence according to a grammar to determine its structure. It groups tokens into higher-level constituents and records the relationships among them.

| Concept | Main question |
|---|---|
| Grammar | What structures are allowed? |
| Parsing | Which allowed structure describes this sentence? |

{{% hint success %}}
Grammar defines the search space; parsing searches that space for a structure that generates the input sentence.
{{% /hint %}}

## 3. Formal Definition of a Grammar ☆

A grammar can be represented as a four-tuple:

{{% colour "red" %}}
{{< katex display=true >}}
G=(N,\Sigma,R,S)
{{< /katex >}}
{{% /colour %}}

Some sources use {{< katex >}} V {{< /katex >}} instead of {{< katex >}} N {{< /katex >}}, and {{< katex >}} T {{< /katex >}} instead of {{< katex >}} \Sigma {{< /katex >}}. The meanings are the same.

| Component | Meaning | Example |
|---|---|---|
| {{< katex >}} N {{< /katex >}} or {{< katex >}} V {{< /katex >}} | Finite set of non-terminal or variable symbols | `S`, `NP`, `VP` |
| {{< katex >}} \Sigma {{< /katex >}} or {{< katex >}} T {{< /katex >}} | Finite set of terminal symbols | `the`, `man`, `saw` |
| {{< katex >}} R {{< /katex >}} or {{< katex >}} P {{< /katex >}} | Set of production rules | `S → NP VP` |
| {{< katex >}} S {{< /katex >}} | Designated start symbol | `S` for sentence |

### Terminals and non-terminals

- **Terminals** are the symbols that appear in the final sentence. In an NLP grammar, these are normally words or lexical items.
- **Non-terminals** name intermediate structures such as sentence, noun phrase, and verb phrase.

By convention, capitalised symbols often represent non-terminals, although the notation depends on the grammar.

### Production rules

A production has the general form:

{{% colour "red" %}}
{{< katex display=true >}}
\alpha \rightarrow \beta
{{< /katex >}}
{{% /colour %}}

It means that an occurrence of {{< katex >}} \alpha {{< /katex >}} may be replaced by {{< katex >}} \beta {{< /katex >}} during a derivation.

For example:

```text
S  → NP VP
VP → V NP
```

The first rule says that a sentence may consist of a noun phrase followed by a verb phrase.

## 4. What Makes a Useful Grammar?

A small grammar may analyse a few examples successfully while failing when the language is extended. A more useful grammar should preserve:

- **Generality** — it should describe a broad range of valid sentences.
- **Simplicity** — it should avoid unnecessary rules and duplicated analyses.
- **Selectivity** — it should reject structures that the language does not permit.

These goals compete. A grammar that accepts too little has poor coverage; one that accepts too much produces excessive ambiguity.

## 5. Context-Free Grammars ☆

A **Context-Free Grammar**, or CFG, is a grammar in which every production has exactly one non-terminal on its left-hand side:

{{% colour "red" %}}
{{< katex display=true >}}
A \rightarrow \beta
{{< /katex >}}
{{% /colour %}}

where {{< katex >}} A {{< /katex >}} is one non-terminal and {{< katex >}} \beta {{< /katex >}} is a sequence of terminals and/or non-terminals.

The rule is context-free because {{< katex >}} A {{< /katex >}} can be replaced by {{< katex >}} \beta {{< /katex >}} without inspecting symbols around it.

Valid CFG-style rules include:

```text
S  → NP VP
NP → ART N
NP → ART ADJ N
VP → V NP
```

### CFGs in the Chomsky hierarchy

The transcript briefly places CFGs within a broader hierarchy:

| Type | Grammar class | Relative restriction |
|---:|---|---|
| 0 | Unrestricted grammar | Least restricted |
| 1 | Context-sensitive grammar | Context may constrain a replacement |
| 2 | Context-free grammar | One non-terminal on the left-hand side |
| 3 | Regular grammar | More restricted rule forms |

The focus here is the Type 2 Context-Free Grammar.

## 6. Phrase Categories

A phrase is a group of words that functions as a unit. Its **head** is the word that determines the phrase's main grammatical type.

| Phrase | Abbreviation | Head | Example |
|---|---|---|---|
| Noun phrase | NP | Noun | `the old man` |
| Verb phrase | VP | Verb | `ate the cake` |
| Adjective phrase | ADJP | Adjective | `very interesting` |
| Adverb phrase | ADVP | Adverb | `very quickly` |
| Prepositional phrase | PP | Preposition | `with the telescope` |

{{% hint info %}}
A phrase may contain other phrases. For example, a verb phrase may contain a verb followed by a noun phrase, and that noun phrase may contain an adjective phrase.
{{% /hint %}}

## 7. Derivation and Parse Trees ☆

Consider the grammar:

```text
S   → NP VP
VP  → V NP
NP  → N
NP  → Adj NP
NP  → Adj

N   → NLP
V   → is
Adj → very
Adj → interesting
```

It can generate:

```text
NLP is very interesting
```

One derivation is:

```text
S
⇒ NP VP
⇒ N VP
⇒ NLP VP
⇒ NLP V NP
⇒ NLP is NP
⇒ NLP is Adj NP
⇒ NLP is very NP
⇒ NLP is very Adj
⇒ NLP is very interesting
```

The same derivation can be shown as a parse tree:

{{< mermaid >}}
flowchart TD
    S["S"] --> NP1["NP"]
    S --> VP["VP"]
    NP1 --> N["N"]
    N --> W1["NLP"]
    VP --> V["V"]
    V --> W2["is"]
    VP --> NP2["NP"]
    NP2 --> A1["Adj: very"]
    NP2 --> NP3["NP"]
    NP3 --> A2["Adj: interesting"]

    style S fill:#E1F5FE
    style NP1 fill:#C8E6C9
    style VP fill:#FFF9C4
    style NP2 fill:#C8E6C9
    style NP3 fill:#C8E6C9
{{< /mermaid >}}

The root is the start symbol, internal nodes are non-terminals, and leaves are terminal words.

## 8. Why Parsing Is Useful

Parsing supports many NLP tasks because it reveals relationships that a flat word sequence hides.

| Application | Contribution of parsing |
|---|---|
| Sentiment analysis | Distinguishes `I like Frozen` from `I do not like Frozen` and from `I like frozen yoghurt` |
| Relation extraction | Identifies which entity participates in which relation |
| Question answering | Reveals the requested entity and the structure of the question |
| Speech recognition | Helps score candidate word sequences as structurally plausible or implausible |
| Machine translation | Helps preserve phrase structure and select among competing translations |
| Grammar checking | Detects structures that violate grammatical rules |
| OCR and text prediction | Helps combine locally recognised symbols or words into plausible sequences |

## 9. Parsing as Search ☆

Given a string and a grammar, a parser may need to:

- decide whether the grammar can generate the string
- return one parse tree
- return all valid parse trees when the sentence is ambiguous

This can be treated as a search problem:

1. Remove one state from the possibilities list.
2. Generate successor states by applying every valid option.
3. Add those states to the possibilities list.
4. Continue until a complete parse is found or no states remain.

The search direction creates two major strategies:

- **Top-down:** begin with the start symbol and try to derive the words.
- **Bottom-up:** begin with the words and combine them until reaching the start symbol.

## 10. Top-Down Parsing ☆

Top-down parsing builds a tree from the root towards the leaves.

{{< mermaid >}}
flowchart TD
    A["Start symbol S"] --> B["Choose a rule for S"]
    B --> C["Expand non-terminals"]
    C --> D["Match terminal words"]
    D --> E{"Complete match?"}
    E -->|Yes| F["Parse found"]
    E -->|No| G["Try another choice"]
    G --> C

    style A fill:#E1F5FE
    style B fill:#C8E6C9
    style C fill:#FFF9C4
    style D fill:#EDE7F6
    style F fill:#C8E6C9
    style G fill:#FFF9C4
{{< /mermaid >}}

### Worked example: `The old man smiled`

Use this grammar:

```text
1. S  → NP VP
2. NP → ART N
3. NP → ART ADJ N
4. VP → V
5. VP → V NP
```

Lexicon:

```text
The     → ART
old     → N, ADJ
man     → N, V
smiled  → V
```

The parser begins with `S`:

```text
S
⇒ NP VP
```

It may first try `NP → ART N`. This matches `The old`, because `old` can be a noun, but the remaining sequence `man smiled` cannot be completed as the required `VP`. That branch fails.

The parser then tries the alternative:

```text
S
⇒ NP VP
⇒ ART ADJ N VP
⇒ The old man VP
⇒ The old man V
⇒ The old man smiled
```

The successful parse is:

```text
S
├── NP
│   ├── ART → The
│   ├── ADJ → old
│   └── N   → man
└── VP
    └── V   → smiled
```

This example shows why a parser may need to retain **backup states**: an early rule choice may look possible but fail later.

### Depth-first and breadth-first search

| Property | Depth-first | Breadth-first |
|---|---|---|
| Possibilities structure | Stack | Queue |
| Order | Last in, first out | First in, first out |
| Behaviour | Expands one interpretation until success or failure | Expands interpretations one level at a time |
| Memory | Often stores fewer backup states | May retain many partial alternatives |
| Risk | Can follow an unproductive path deeply | Can use substantial memory |

## 11. Bottom-Up Parsing ☆

Bottom-up parsing starts with the sentence's words and works towards the grammar's start symbol.

Its main operations are:

1. Assign each word one or more possible lexical categories.
2. Find a sequence that matches the right-hand side of a grammar rule.
3. Replace that sequence with the rule's left-hand-side symbol.
4. Repeat until only the start symbol remains.

For a phrase such as `book that flight`, a bottom-up parser first identifies lexical categories and then combines them:

```text
book  that  flight
 V     DET    N
       └── NP ──┘
└─────── VP ─────┘
```

The exact reductions depend on the supplied grammar, but every reduction is justified by matching a rule's right-hand side.

### Top-down versus bottom-up

| Top-down | Bottom-up |
|---|---|
| Starts with the grammar's start symbol | Starts with the input words |
| Predicts structures that could form a sentence | Builds structures supported by the input |
| Avoids constituents that cannot lead from the start symbol | Avoids structures unrelated to the actual words |
| May predict structures that never match the input | May build constituents that can never form a complete sentence |

The amount of wasted work depends on how much the grammar branches in each direction.

## 12. Why Ordinary Search Repeats Work

Ambiguous words and recursive grammar rules cause several search branches to reuse the same partial analyses. A naive parser may recompute those analyses each time.

For example, once `the water` has been recognised as an `NP`, every larger analysis that needs that constituent should reuse it instead of deriving it again.

This motivates **chart parsing**.

## 13. Chart Parsing ☆

A chart parser stores partial and completed analyses so they can be shared across search branches.

{{% hint info %}}
A chart is the parsing equivalent of dynamic programming: solve each useful subproblem once, store the result, and reuse it.
{{% /hint %}}

### Main data structures

| Structure | Purpose |
|---|---|
| Key | The current constituent used to extend matching grammar rules |
| Active arc | A grammar rule whose right-hand side has been matched only partially |
| Agenda | Newly discovered constituents that still need processing |
| Chart | Stored constituents and arcs spanning positions in the input |

### Dotted-rule notation

A dot shows how much of a rule has already been matched.

```text
NP → ART • ADJ N
```

This means `ART` has been recognised and the parser next expects `ADJ N`.

If an adjective is found, the arc advances:

```text
NP → ART ADJ • N
```

A rule is complete when the dot reaches the end:

```text
NP → ART ADJ N •
```

The completed rule contributes a new `NP` constituent to the agenda.

### Extending an active arc

Suppose an active arc expects constituent {{< katex >}} C {{< /katex >}} next:

```text
A → X₁ ... • C ... Xₘ
```

If a key constituent `C` covers the required next input span, the dot advances:

```text
A → X₁ ... C • ... Xₘ
```

If the rule becomes complete, its left-hand-side constituent is added to the agenda. After the key has extended all relevant arcs, it is stored in the chart.

### Bottom-up chart procedure

1. Process the input from left to right.
2. Find all possible POS categories for the next word.
3. Put those lexical constituents on the agenda.
4. Remove one key from the agenda.
5. Start every grammar rule whose right-hand side begins with that key.
6. Extend every existing active arc that expects that key.
7. Add newly completed left-hand sides to the agenda.
8. Store the processed key in the chart.
9. Continue until the agenda is empty, then move to the next word.

Parsing is complete when no chart rule can add a new edge.

## 14. Worked Chart Example ☆

Consider:

```text
The large can can hold the water
```

The repeated word `can` is lexically ambiguous.

### Lexical possibilities

| Word | Possible categories |
|---|---|
| the | ART |
| large | ADJ |
| can | N, AUX, V |
| hold | N, V |
| water | N, V |

### Grammar

```text
1. S  → NP VP
2. NP → ART ADJ N
3. NP → ART N
4. NP → ADJ N
5. VP → AUX VP
6. VP → V NP
```

### Useful completed constituents

The chart can construct:

```text
The large can     → NP       using NP → ART ADJ N
the water         → NP       using NP → ART N
hold the water    → VP       using VP → V NP
can hold the water→ VP       using VP → AUX VP
whole sentence    → S        using S  → NP VP
```

The resulting structure is:

```text
S
├── NP
│   ├── ART → The
│   ├── ADJ → large
│   └── N   → can
└── VP
    ├── AUX → can
    └── VP
        ├── V  → hold
        └── NP
            ├── ART → the
            └── N   → water
```

The grammar and the complete sentence resolve the two occurrences of `can`: the first is a noun, while the second is an auxiliary.

## 15. Top-Down Chart Parsing ☆

Top-down chart parsing combines the predictiveness of top-down search with the reuse provided by a chart.

It begins by placing a prediction for the start symbol in the chart:

```text
S → • NP VP
```

Because the parser expects an `NP`, it introduces only rules that can derive an `NP`:

```text
NP → • ART ADJ N
NP → • ART N
NP → • ADJ N
```

As input constituents are discovered, compatible active arcs advance. When an arc predicts a new non-terminal, rules for that non-terminal are introduced recursively.

### High-level procedure

1. Initialise the chart with rules for the start symbol.
2. If the agenda is empty, read the next word and add its possible categories.
3. Remove one constituent from the agenda.
4. Extend compatible arcs in the chart.
5. Introduce rules predicted by newly created active arcs.
6. Put newly completed constituents on the agenda.
7. Stop when no new constituents or arcs can be added.

### Why prediction helps

A word may have several lexical categories in isolation. If some categories cannot occur in any structure currently predicted by the grammar, the parser need not explore them.

For `The large can can hold the water`, the initial prediction expects an `NP`. This guides the analysis of `The large can`; after that `NP` completes, the parser predicts a `VP` for the remaining words.

## 16. Advantages of Chart Parsing

Chart parsing offers four key benefits:

- It avoids recomputing the same subproblem.
- It handles ambiguity by storing multiple analyses compactly.
- It can deal well with left-recursive grammars.
- It removes the need for ordinary backtracking because alternatives remain represented in the chart.

| Ordinary backtracking parser | Chart parser |
|---|---|
| Recomputes repeated partial structures | Reuses stored constituents and arcs |
| Keeps alternatives as backup search states | Keeps alternatives in a shared chart |
| May repeatedly revisit failed work | Extends only newly available combinations |
| Ambiguity can multiply search effort | Ambiguity is represented without duplicating common work |

## 17. Morphological Processing ☆

**Morphology** studies the internal structure of words. A **morpheme** is the smallest unit that carries meaning or grammatical information.

A word may contain:

- one morpheme, such as `cat`
- a root and a suffix, such as `eat + en`
- a prefix and a root
- several morphemes

For example:

```text
eaten = eat + en
```

Here, `eat` is the root and `-en` marks the past-participle form.

### Why preprocess words into morphemes?

Without morphological processing, a lexicon may need separate entries for:

```text
eat, eats, eating, ate, eaten
```

With morphological processing, it can store the productive root `eat`, rules for suffixes such as `-s`, `-ing`, and `-en`, and a separate entry for the irregular form `ate`.

This reduces duplication and exposes grammatical information explicitly.

## 18. Finite-State Morphology

A finite-state model can represent valid paths between a root and its inflected form. For nouns, a simplified model may distinguish:

- a regular noun stem
- an optional plural suffix `-s`
- irregular singular forms
- irregular plural forms

The states record how much morphological structure has been recognised, while labelled transitions consume roots or affixes.

### Analysis and generation

Morphological processing can work in two directions:

| Direction | Input | Output |
|---|---|---|
| Analysis or parsing | Surface word | Lexical structure |
| Generation | Lexical structure | Surface word |

Example:

```text
cats  →  cat +N +PL
cat +N +PL  →  cats
```

`+N` identifies the noun category and `+PL` identifies plural number.

{{% hint success %}}
Syntactic parsing maps a sentence to phrase structure. Morphological parsing maps a word to morpheme structure. Both recover hidden structure from a surface form.
{{% /hint %}}

## 19. Connecting the Main Ideas

| Level | Surface input | Hidden structure | Typical mechanism |
|---|---|---|---|
| Morphology | Word | Root and affixes | Finite-state model |
| Syntax | Sentence | Phrases and relationships | CFG and parser |

The complete processing path can therefore be viewed as:

{{< mermaid >}}
flowchart TD
    A["Surface words"] --> B["Morphological analysis"]
    B --> C["Lexical categories"]
    C --> D["Syntactic parsing"]
    D --> E["Phrase structure"]

    style A fill:#E1F5FE
    style B fill:#C8E6C9
    style C fill:#FFF9C4
    style D fill:#EDE7F6
    style E fill:#E1F5FE
{{< /mermaid >}}

## Common Mistakes

{{% hint warning %}}
- A grammar is the rule system; a parser is the procedure that applies it.
- Terminals are the final symbols in the generated string; non-terminals name intermediate structures.
- A CFG requires one non-terminal on the left-hand side, not one symbol on both sides.
- Top-down parsing begins with the start symbol; bottom-up parsing begins with the input words.
- In a dotted rule, material to the left of the dot has already been matched; material to the right is still expected.
- An active arc is incomplete. When its dot reaches the end, it yields a completed constituent.
- Morphological analysis concerns structure inside words, while syntactic parsing concerns structure across words.
{{% /hint %}}

## Practice Questions

1. Explain the two interpretations of `I saw the man with the telescope`.
2. Define the four components of {{< katex >}} G=(N,\Sigma,R,S) {{< /katex >}}.
3. Why is `A → ART N` a valid CFG production?
4. Construct a parse tree for `NLP is very interesting` using the supplied grammar.
5. Compare top-down and bottom-up parsing in terms of their starting points and wasted search.
6. Trace the successful top-down parse of `The old man smiled`.
7. Interpret the dotted rule `NP → ART ADJ • N`.
8. What roles do the agenda and chart play in chart parsing?
9. Show how the two occurrences of `can` are disambiguated in `The large can can hold the water`.
10. State four advantages of chart parsing.
11. Decompose `eaten` into morphemes and explain why this reduces lexicon size.
12. Distinguish morphological analysis from morphological generation.

## Key Takeaways

{{% hint success %}}
- Sentence meaning depends on hidden phrase structure, not only on individual words.
- A grammar specifies legal structures; a parser finds structures licensed by that grammar.
- A CFG has one non-terminal on the left-hand side of every production.
- Top-down parsing predicts from the start symbol, while bottom-up parsing constructs from the input.
- A chart stores active arcs and completed constituents so repeated work can be reused.
- Top-down chart parsing combines prediction with dynamic-programming-style reuse.
- Morphological processing analyses words as roots and affixes and can be modelled with finite-state methods.
{{% /hint %}}

## Checklist

- [ ] I can explain how structural ambiguity arises.
- [ ] I can identify terminals, non-terminals, productions, and the start symbol.
- [ ] I can recognise a valid CFG production.
- [ ] I can perform a simple derivation and draw its parse tree.
- [ ] I can compare top-down and bottom-up parsing.
- [ ] I can interpret active arcs and dotted rules.
- [ ] I can explain the agenda-and-chart workflow.
- [ ] I can trace the main constituents in the chart example.
- [ ] I can explain morphological analysis and generation.

---
{{< home-link "Home" >}} | {{< section-index >}}
