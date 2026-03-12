---
title: 'Decision Tree'
draft: false
tags: ["AI", "ML"]
categories: ["AI", "ML"]
weight: 500
---
# Decision Tree

A decision tree classifies an example by asking a sequence of questions about its attributes until it reaches a leaf (final decision).

{{% hint info %}}
Key takeaway:
A decision tree grows by repeatedly splitting the training data into **purer** subsets using an impurity measure (entropy / Gini / classification error).
{{% /hint %}}

---

## Information Theory

Decision trees use **information theory** ideas to measure how “mixed” a node is.

### Impurity (how mixed are the labels?)

A node is:
- **pure** if all records belong to the same class → impurity = 0
- **most impure** if classes are evenly split

### Entropy

Entropy measures impurity/uncertainty in the class distribution at a node.

{{< colour "blue" >}}
{{< katex display=true >}}
H(S)=-\sum_{c=1}^{C} p(c)\log_2 p(c)
{{< /katex >}}
{{< /colour >}}

Binary case ($p_+$ positives, $p_-$ negatives):

{{< colour "blue" >}}
{{< katex display=true >}}
H(S)= -p_{+}\log_2(p_{+}) - p_{-}\log_2(p_{-})
{{< /katex >}}
{{< /colour >}}

Interpretation:
- $H(S)=0$ when the node is pure
- $H(S)$ is maximum when classes are evenly distributed

### Information Gain (entropy reduction)

Information gain is the **expected reduction in entropy** after splitting on attribute $A$.

{{< colour "blue" >}}
{{< katex display=true >}}
\mathrm{Gain}(S,A)=H(S)-\sum_{v\in \mathrm{Values}(A)}\frac{|S_v|}{|S|}H(S_v)
{{< /katex >}}
{{< /colour >}}

Where:
- $S$ is the set of examples at the node
- $S_v$ is the subset where attribute $A$ takes value $v$

Decision rule:
- pick the attribute with **maximum** information gain
- equivalently: pick the attribute with **minimum weighted child entropy**

### Alternative impurity measures

#### Gini Index (used in CART)

{{< colour "blue" >}}
{{< katex display=true >}}
\mathrm{Gini}(S)=1-\sum_{c=1}^{C}p(c)^2
{{< /katex >}}
{{< /colour >}}

Lower Gini = purer node.

#### Classification Error

{{< colour "blue" >}}
{{< katex display=true >}}
\mathrm{Err}(S)=1-\max_c p(c)
{{< /katex >}}
{{< /colour >}}

Note:
classification error is less sensitive than entropy/Gini, so it is often not used for growing the tree (more for pruning/evaluation).

### Gain Ratio (fixes information gain bias)

Information gain can prefer attributes with many distinct values (e.g. CustomerID).
Gain ratio penalises such splits.

Split information:

{{< colour "blue" >}}
{{< katex display=true >}}
\mathrm{SplitInfo}(S,A)= -\sum_{v\in \mathrm{Values}(A)}\frac{|S_v|}{|S|}\log_2\left(\frac{|S_v|}{|S|}\right)
{{< /katex >}}
{{< /colour >}}

Gain ratio:

{{< colour "blue" >}}
{{< katex display=true >}}
\mathrm{GainRatio}(S,A)=\frac{\mathrm{Gain}(S,A)}{\mathrm{SplitInfo}(S,A)}
{{< /katex >}}
{{< /colour >}}

---

## Entropy Based Decision Tree Construction

A decision tree is grown recursively by splitting the data into purer subsets.

### Tree structure

- Root node: no incoming edges
- Internal node: test condition on one attribute (two or more outgoing edges)
- Leaf node: class label (no outgoing edges)

### Hunt’s Algorithm (high-level)

At each node:

1) If all records belong to the same class → make it a leaf.
2) Otherwise:
   - choose the best attribute test condition (using Gain / Gini gain / etc.)
   - split records into children
   - repeat recursively for each child

### ID3 summary (entropy + information gain)

ID3 builds the tree using information gain.

Stopping / base cases:
- if all examples at the node are the same class → leaf
- if no attributes remain → leaf with **most common** class (majority vote)
- if a split produces an empty subset → leaf with **most common** class of the parent node

### Exam note: “min entropy” vs “max gain”

Sometimes a question may explicitly ask you to:
- **compute entropy only** and pick the split with **minimum entropy**, or
- **compute information gain** and pick the split with **maximum gain**

Always follow the wording of the question.

---

## Avoiding Overfitting

### Underfitting vs overfitting

- **Underfitting**: model too simple → high training error and high test error
- **Overfitting**: model too complex → low training error but high test error

In decision trees:
- very deep trees can memorise noise/outliers
- the tree may look “perfect” on training data but perform poorly on unseen data

### How to reduce overfitting

Common approaches:
- pruning
- constrain the depth of the tree
- constrain the minimum samples allowed in a leaf (minimum records per node)

### Pre-pruning (early stopping)

Stop tree growth early using stopping conditions, such as:
- stop if Gain (or Gini gain) is below a threshold
- stop if node has fewer than a minimum number of samples
- stop if depth exceeds a maximum level
- stop if improvement in generalisation is not significant

Trade-off:
- too strict → shallow tree → underfitting
- too loose → deep tree → overfitting

### Post-pruning (grow then prune)

Idea:
1) grow the tree fully (or until minimal leaf size)
2) scan the tree bottom-up
3) at each decision node:
   - evaluate the subtree on a prune/validation set
   - remove the node (replace by a leaf using majority vote) and re-evaluate
   - if error reduces, prune; otherwise keep
4) repeat on other branches

Error rate = % misclassified tuples on the prune/validation set.

---

## Minimum Description Length

MDL is an information-theoretic way to trade off:
- tree complexity
- training errors

Interesting fact:
the optimal (shortest expected) coding length for an event with probability $p$ is $-\log_2(p)$ bits.

MDL prefers the hypothesis $h$ that minimises:

{{< colour "blue" >}}
{{< katex display=true >}}
LC(h)+LC(D\mid h)
{{< /katex >}}
{{< /colour >}}

Where:
- $LC(h)$ = number of bits to describe the tree (structure, attributes, thresholds)
- $LC(D\mid h)$ = number of bits to describe the mistakes/exceptions made by the tree

Intuition (Occam’s razor):
prefer shorter trees, as long as they still classify reasonably well.

---

## Handling Continuous valued attributes, missing attributes

### Attribute test conditions (categorical vs continuous)

- Binary attribute: two-way split
- Nominal attribute: multi-way split or binary split by grouping categories
- Continuous attribute: choose a threshold and split

### Continuous-valued attributes (choose threshold)

Given a continuous attribute $A$, create a binary attribute $A_c$:

{{< colour "blue" >}}
{{< katex display=true >}}
A_c=
\begin{cases}
\text{True} & A<c\\
\text{False} & A\ge c
\end{cases}
{{< /katex >}}
{{< /colour >}}

How to find $c$:

#### Way 1: Multi-way split (PlayTennis-style)
- sort examples by the attribute value
- identify candidate thresholds at points where the class label changes
- candidate threshold = average of the two consecutive values

#### Way 2: Binary splits (scan all candidates)
- sort values
- take midpoints between adjacent sorted values as candidates
- evaluate impurity for each candidate split
- pick the threshold that gives the best impurity reduction

### Missing attribute values (practical handling)

Situations you’ll see in problems:
- After splitting on an attribute, a branch may receive **no samples**.
  - Make that branch a leaf with the **majority class** from the parent node.
- If no attributes remain for further splitting:
  - stop and apply **majority voting** to label the leaf.

In prediction:
- if a required attribute value is missing for a test record, common strategies include using the majority class at that node (or following the most frequent branch).

---

{{< home-link "Home" >}} | {{< section-index >}}
