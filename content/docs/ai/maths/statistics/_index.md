---
title: "Statistical Methods"
draft: false
tags: ["Statistics", "Probability", "Data Analysis"]
categories: ["AI", "ML"]
weight: 2000
menu: main
bookCollapseSection: true
---

# Statistical Methods

Statistical methods help you turn **raw data into reliable conclusions**, while understanding **uncertainty, variability, and confidence**.

Statistics provides the **language and tools** for reasoning about data, uncertainty, and inference.

ML needs **understanding data behaviour**, drawing conclusions, and validating machine learning models.


- Collect Data
- Present & Organise Data (in a systematic manner)
- Alalyse Data
- Infer about the Data
- Take Decision from the Data

---

{{< section_tree >}}

---

## Data - Types

{{< mermaid >}}
flowchart TD
	A[(Data)] --> B["Categorical (Qualitative)"]
    A --> C["Numerical (Quantitative)"]

    B --> B1[Nominal]
    B --> B2[Ordinal]

    C --> C1[Discrete]
    C --> C2[Continuous]

    C2 --> C21[Interval]
    C2 --> C22[Ratio]

    %% Styling
    style A fill:#E1F5FE,stroke:#333
    style B fill:#90CAF9,stroke:#333
    style B1 fill:#90CAF9,stroke:#333
    style B2 fill:#90CAF9,stroke:#333
    style C fill:#FFF9C4,stroke:#333
    style C1 fill:#FFF9C4,stroke:#333
    style C2 fill:#FFF9C4,stroke:#333
    style C21 fill:#FFF9C4,stroke:#333
    style C22 fill:#FFF9C4,stroke:#333
{{< /mermaid >}}

{{% steps %}}

1. ## Categorical (Qualitative)
	express a qualitative attribute
	e.g. hair color, eye color
	- Nominal: **no natural ordering** is possible
				e.g. hair color, eye color
	- Ordinal:  a **meaningful order** is possible
				e.g. health, which can take values such as poor, reasonable, good, or excellent

2. ## Numerical (Quantitative)
	measured using numbers
	e.g. height, weight, number of people. 
	- Discrete:
		**countable** and typically whole numbers
				e.g. number of people
	- Continuous: 
		**measured**, not counted, and can take infinitely many values in a range
				e.g. height
		- INTERVAL: ratio of values of variable do not have any meaning and it does not have an inherently defined zero value
				e.g. temperature
		- RATIO: ratio of values of variable have  meaning and it have an inherently defined zero value such as weight

{{% /steps %}}

{{% hint info %}}

If the data is a **label** → categorical.  
If the data is a **count** → discrete.  
If the data is a **measurement** → continuous.  
If “twice as much” makes sense → ratio scale.

{{% /hint %}}

---

## Role of Statistics in AI and ML

- Helps understand **data distributions**
- Enables **model evaluation**
- Supports **decision-making under uncertainty**
- Forms the basis of **many ML algorithms**

---

## Core

### 1. Descriptive Statistics
Summarises and describes data.

- Mean, median, mode
- Variance and standard deviation
- Histograms and distributions

---

### 2. Probability Distributions
Models how data behaves.

- Normal (Gaussian) distribution
- Binomial and Poisson distributions
- Continuous vs discrete variables

---

### 3. Statistical Inference
Draws conclusions from data samples.

- Sampling techniques
- Confidence intervals
- Hypothesis testing
- p-values

---

### 4. Regression and Correlation
Analyses relationships between variables.

- Linear regression
- Correlation coefficients
- Causation vs correlation

---

### 5. Model Evaluation
Assesses performance and reliability.

- Bias and variance
- Overfitting and underfitting
- Error estimation

---

## Conceptual View

{{< mermaid >}}
flowchart LR
    DATA[Observed Data]
    SAMPLE[Sampling]
    ANALYSIS[Statistical Analysis]
    INFER[Inference & Decisions]

    DATA --> SAMPLE
    SAMPLE --> ANALYSIS
    ANALYSIS --> INFER
{{< /mermaid >}}

---

## Outcome

- Analyse datasets statistically
- Make data-driven inferences
- Evaluate ML models rigorously
- Understand uncertainty and variability in predictions

---

{{< home-link "Home" >}} | {{< section-index >}}





