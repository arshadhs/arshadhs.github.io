---
title: "Statistics"
draft: false
tags: ["Statistics", "Probability", "Data Analysis"]
categories: ["AI", "ML"]
weight: 600
menu: main
bookCollapseSection: true
---

# Statistics

**Statistical methods** help you turn **raw data into reliable conclusions**, while understanding **uncertainty, variability, and confidence**.

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

| Statistics Topic | What you learn (plain English) | ML Connection |
|---|---|---|
| 1. Basic Probability & Statistics | Summarise data; understand spread; basic probability rules | Data understanding (EDA), feature sanity checks, detecting outliers, interpreting “average behaviour” |
| 2. Conditional Probability & Bayes | Update probability using new information; Bayes’ rule | Naïve Bayes, Bayesian thinking, posterior probabilities, probabilistic classification |
| 3. Probability Distributions | Model randomness with distributions; expectation/variance/covariance | Likelihood models, noise assumptions (Gaussian), sampling, probabilistic modelling foundations |
| 4. Hypothesis Testing | Sampling, CLT, confidence intervals, significance tests, ANOVA, MLE | A/B testing, evaluating model improvements, significance vs noise, parameter estimation (MLE) |
| 5. Prediction & Forecasting | Correlation, regression, time series (AR/MA/ARIMA/SARIMA etc.) | Linear regression, forecasting, sequential data modelling, baseline predictive modelling |
| 6. GMM & EM | Mixtures of Gaussians; iterative estimation with EM | Unsupervised learning (soft clustering), density estimation, latent-variable models |

---

{{< mermaid >}}
flowchart TD
  A[Intro to Statistical Methods<br/>AIML ZC418] --> B[1. Basic Probability & Statistics]
  A --> C[2. Conditional Probability & Bayes]
  A --> D[3. Probability Distributions]
  A --> E[4. Hypothesis Testing]
  A --> F[5. Prediction & Forecasting]
  A --> G[6. Gaussian Mixture Model & EM]

  %% Block 1
  B --> B1[Central Tendency<br/>Mean • Median • Mode]
  B --> B2[Variability<br/>Range • Variance • SD • Quartiles]
  B --> B3[Basic Probability Concepts]
  B3 --> B31[Axioms of Probability]
  B3 --> B32[Definition of Probability]
  B3 --> B33[Mutually Exclusive vs Independent]

  %% Block 2
  C --> C1[Conditional Probability]
  C --> C2[Independence (conditional)]
  C --> C3[Bayes' Theorem]
  C --> C4[Naïve Bayes (intro)]

  %% Block 3
  D --> D1[Random Variables<br/>Discrete & Continuous]
  D --> D2[Expectation • Variance • Covariance]
  D --> D3[Transformations of RVs]
  D --> D4[Key Distributions]
  D4 --> D41[Bernoulli]
  D4 --> D42[Binomial]
  D4 --> D43[Poisson]
  D4 --> D44[Normal (Gaussian)]
  D4 --> D45[t • Chi-square • F (intro)]

  %% Block 4
  E --> E1[Sampling<br/>Random • Stratified]
  E --> E2[Sampling Distributions<br/>CLT]
  E --> E3[Estimation<br/>Confidence Intervals]
  E --> E4[Hypothesis Tests<br/>Means • Proportions]
  E --> E5[ANOVA<br/>Single & Dual factor]
  E --> E6[Maximum Likelihood]

  %% Block 5
  F --> F1[Correlation]
  F --> F2[Regression]
  F --> F3[Time Series Basics<br/>Components]
  F --> F4[Moving Averages<br/>Simple & Weighted]
  F --> F5[Time Series Models]
  F5 --> F51[AR]
  F5 --> F52[ARMA / ARIMA]
  F5 --> F53[SARIMA / SARIMAX]
  F5 --> F54[VAR / VARMAX]
  F --> F6[Exponential Smoothing]

  %% Block 6
  G --> G1[GMM<br/>Mixture of Gaussians]
  G --> G2[EM Algorithm<br/>E-step • M-step]

  %% Learning flow emphasis
  B -.-> C
  C -.-> D
  D -.-> E
  E -.-> F
  F -.-> G
{{< /mermaid >}}

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

## Refrences

[Probability & Statistics](https://medium.com/@naghma2404/probability-and-statistics-for-data-science-4f580829ed60)

[Probability & Statistics](https://www.analyticsvidhya.com/blog/2021/04/statistics-and-probability-concepts-for-data-science/)

---

{{< home-link "Home" >}} | {{< section-index >}}





