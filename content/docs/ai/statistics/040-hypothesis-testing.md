---
title: "Hypothesis Testing"
draft: false
tags: ["AI", "ML", "Stats", "Hypothesis Testing", "ANOVA", "Maximum Likelihood"]
categories: ["AI", "ML", "Stats"]
weight: 400
menu: main
---

# Hypothesis Testing

Hypothesis testing is a statistical decision-making method used to decide whether sample evidence is strong enough to reject an initial assumption about a population.

It connects probability, sampling distributions, confidence intervals, significance levels, and decision rules.

{{% hint info %}}
**Key takeaway:**  
Hypothesis testing is not about proving something with certainty.

It is about asking:

> If the null hypothesis were true, how surprising would this sample result be?
{{% /hint %}}

- Sampling: random and stratified sampling
- Sampling distribution
- Central Limit Theorem
- Interval estimation and confidence level
- Testing of hypothesis
- Mean-based tests
- Proportion-based tests
- ANOVA: single factor and dual factor
- Maximum likelihood

---

## Big Picture

Hypothesis testing follows a repeatable workflow.

{{< mermaid >}}
flowchart LR
    A[Research Question] --> B[Set H0 and H1]
    B --> C[Choose Test]
    C --> D[Compute Test Statistic]
    D --> E[Find p-value or Critical Value]
    E --> F[Decision]
    F --> G[Interpret in Context]

    style A fill:#E1F5FE
    style B fill:#FFF9C4
    style C fill:#EDE7F6
    style D fill:#C8E6C9
    style E fill:#FFF9C4
    style F fill:#E1F5FE
    style G fill:#C8E6C9
{{< /mermaid >}}

---

## Null and Alternative Hypotheses ☆

A **hypothesis** is a claim about a population parameter.

The two main hypotheses are:

| Hypothesis | Meaning | Example |
|---|---|---|
| Null hypothesis | Default assumption; no change, no difference, no effect | The mean waiting time is 5 minutes |
| Alternative hypothesis | What we look for evidence to support | The mean waiting time is not 5 minutes |

{{% colour "red" %}}The null hypothesis is assumed true until the sample gives enough evidence against it.{{% /colour %}}

### Common Notation

{{% colour "red" %}}
{{< katex display=true >}}
H_0 : \mu = \mu_0
{{< /katex >}}
{{% /colour %}}

{{% colour "red" %}}
{{< katex display=true >}}
H_1 : \mu \neq \mu_0
{{< /katex >}}
{{% /colour %}}

---

## Types of Tests ☆

| Test Type | Alternative Hypothesis | Meaning |
|---|---|---|
| Two-tailed test | {{< katex >}} \mu \neq \mu_0 {{< /katex >}} | Checks for any difference |
| Right-tailed test | {{< katex >}} \mu > \mu_0 {{< /katex >}} | Checks if value is greater |
| Left-tailed test | {{< katex >}} \mu < \mu_0 {{< /katex >}} | Checks if value is smaller |

{{% hint warning %}}
Do not choose the tail after seeing the data.

The direction of the test must come from the question statement.
{{% /hint %}}

---

## Significance Level and p-value ☆

The **significance level** is the allowed probability of rejecting the null hypothesis when it is actually true.

Common values are:

- 0.10
- 0.05
- 0.01

The **p-value** measures how surprising the sample result is if the null hypothesis is true.

| Rule | Decision |
|---|---|
| p-value less than or equal to significance level | Reject {{< katex >}} H_0 {{< /katex >}} |
| p-value greater than significance level | Fail to reject {{< katex >}} H_0 {{< /katex >}} |

{{% colour "red" %}}
{{< katex display=true >}}
p\text{-value} \leq \alpha \Rightarrow \text{Reject } H_0
{{< /katex >}}
{{% /colour %}}

---

## Type I and Type II Errors ☆

| True Situation | Decision | Error? |
|---|---|---|
| {{< katex >}} H_0 {{< /katex >}} is true | Reject {{< katex >}} H_0 {{< /katex >}} | Type I error |
| {{< katex >}} H_0 {{< /katex >}} is false | Fail to reject {{< katex >}} H_0 {{< /katex >}} | Type II error |

{{% colour "red" %}}
{{< katex display=true >}}
\alpha = P(\text{Type I error})
{{< /katex >}}
{{% /colour %}}

{{% colour "red" %}}
{{< katex display=true >}}
\beta = P(\text{Type II error})
{{< /katex >}}
{{% /colour %}}

{{% colour "red" %}}
{{< katex display=true >}}
\text{Power} = 1 - \beta
{{< /katex >}}
{{% /colour %}}

---

## Sampling Distribution and Central Limit Theorem ☆

A sampling distribution describes the behaviour of a statistic, such as the sample mean, over repeated samples.

The **Central Limit Theorem** says that for a sufficiently large sample size, the distribution of the sample mean becomes approximately normal, even if the original data is not perfectly normal.

{{% colour "red" %}}
{{< katex display=true >}}
\bar{X} \sim N\left(\mu, \frac{\sigma^2}{n}\right)
{{< /katex >}}
{{% /colour %}}

The standard error of the mean is:

{{% colour "red" %}}
{{< katex display=true >}}
SE = \frac{\sigma}{\sqrt{n}}
{{< /katex >}}
{{% /colour %}}

---

## Confidence Interval ☆

A confidence interval gives a plausible range for a population parameter.

For a population mean with known standard deviation:

{{% colour "red" %}}
{{< katex display=true >}}
\bar{x} \pm z_{\alpha/2}\frac{\sigma}{\sqrt{n}}
{{< /katex >}}
{{% /colour %}}

For a population mean with unknown standard deviation, use the t-distribution:

{{% colour "red" %}}
{{< katex display=true >}}
\bar{x} \pm t_{\alpha/2,n-1}\frac{s}{\sqrt{n}}
{{< /katex >}}
{{% /colour %}}

{{% hint info %}}
A wider confidence interval means more uncertainty.

A larger sample size normally gives a narrower interval.
{{% /hint %}}

---

## One-Sample Mean Test ☆

Use this when checking whether a population mean differs from a claimed value.

### z-test for Mean

Use when population standard deviation is known or sample size is large.

{{% colour "red" %}}
{{< katex display=true >}}
z = \frac{\bar{x} - \mu_0}{\sigma / \sqrt{n}}
{{< /katex >}}
{{% /colour %}}

### t-test for Mean

Use when population standard deviation is unknown and the sample standard deviation is used.

{{% colour "red" %}}
{{< katex display=true >}}
t = \frac{\bar{x} - \mu_0}{s / \sqrt{n}}
{{< /katex >}}
{{% /colour %}}

Degrees of freedom:

{{% colour "red" %}}
{{< katex display=true >}}
df = n - 1
{{< /katex >}}
{{% /colour %}}

---

## Two-Sample Mean Test ☆

Use this when comparing two population means.

### Independent Samples

{{% colour "red" %}}
{{< katex display=true >}}
t = \frac{\bar{x}_1 - \bar{x}_2}{\sqrt{\frac{s_1^2}{n_1} + \frac{s_2^2}{n_2}}}
{{< /katex >}}
{{% /colour %}}

### Paired Samples

Use when observations are naturally paired, such as before-and-after measurements.

{{% colour "red" %}}
{{< katex display=true >}}
t = \frac{\bar{d}}{s_d / \sqrt{n}}
{{< /katex >}}
{{% /colour %}}

---

## One-Proportion Test ☆

Use this when testing a claim about a population proportion.

{{% colour "red" %}}
{{< katex display=true >}}
z = \frac{\hat{p} - p_0}{\sqrt{\frac{p_0(1-p_0)}{n}}}
{{< /katex >}}
{{% /colour %}}

Where:

- {{< katex >}} \hat{p} {{< /katex >}} is the sample proportion
- {{< katex >}} p_0 {{< /katex >}} is the claimed population proportion
- {{< katex >}} n {{< /katex >}} is sample size

---

## Several Proportions and Chi-Square Test ☆

When comparing categorical distributions, use a chi-square test.

{{% colour "red" %}}
{{< katex display=true >}}
\chi^2 = \sum \frac{(O - E)^2}{E}
{{< /katex >}}
{{% /colour %}}

Where:

- {{< katex >}} O {{< /katex >}} is observed frequency
- {{< katex >}} E {{< /katex >}} is expected frequency

Expected count in a contingency table:

{{% colour "red" %}}
{{< katex display=true >}}
E = \frac{(\text{row total})(\text{column total})}{\text{grand total}}
{{< /katex >}}
{{% /colour %}}

---

## ANOVA: Analysis of Variance ☆

ANOVA compares means across more than two groups.

Instead of doing many pairwise t-tests, ANOVA checks whether at least one group mean differs.

### One-Way ANOVA

Use one categorical factor and one numerical response.

Example:

- Factor: teaching method
- Response: exam score

Hypotheses:

{{% colour "red" %}}
{{< katex display=true >}}
H_0 : \mu_1 = \mu_2 = \cdots = \mu_k
{{< /katex >}}
{{% /colour %}}

{{% colour "red" %}}
{{< katex display=true >}}
H_1 : \text{At least one mean is different}
{{< /katex >}}
{{% /colour %}}

### F Statistic

{{% colour "red" %}}
{{< katex display=true >}}
F = \frac{MSB}{MSW}
{{< /katex >}}
{{% /colour %}}

Where:

- {{< katex >}} MSB {{< /katex >}} is mean square between groups
- {{< katex >}} MSW {{< /katex >}} is mean square within groups

{{% colour "red" %}}
{{< katex display=true >}}
MSB = \frac{SSB}{k-1}
{{< /katex >}}
{{% /colour %}}

{{% colour "red" %}}
{{< katex display=true >}}
MSW = \frac{SSW}{N-k}
{{< /katex >}}
{{% /colour %}}

### Two-Way ANOVA

Two-way ANOVA studies two factors at the same time.

It can check:

- effect of factor A
- effect of factor B
- interaction between A and B

{{% hint warning %}}
In ANOVA, rejecting the null hypothesis tells you that at least one mean differs.

It does not automatically tell which groups differ.

For that, post-hoc tests are needed.
{{% /hint %}}

---

## Maximum Likelihood Estimation ☆

Maximum Likelihood Estimation is a method for estimating parameters by choosing the parameter values that make the observed data most likely.

If data points are independent:

{{% colour "red" %}}
{{< katex display=true >}}
L(\theta) = \prod_{i=1}^{n} f(x_i \mid \theta)
{{< /katex >}}
{{% /colour %}}

The log-likelihood is usually easier to optimise:

{{% colour "red" %}}
{{< katex display=true >}}
\ell(\theta) = \log L(\theta) = \sum_{i=1}^{n} \log f(x_i \mid \theta)
{{< /katex >}}
{{% /colour %}}

MLE chooses:

{{% colour "red" %}}
{{< katex display=true >}}
\hat{\theta}_{MLE} = \arg\max_{\theta} \ell(\theta)
{{< /katex >}}
{{% /colour %}}

### Example: Bernoulli Parameter

For Bernoulli data, the MLE of probability of success is the sample proportion.

{{% colour "red" %}}
{{< katex display=true >}}
\hat{p}_{MLE} = \frac{\sum_{i=1}^{n} x_i}{n}
{{< /katex >}}
{{% /colour %}}

---

## Exam Workflow ☆

Use this decision table in exams.

| Question Clue | Likely Test |
|---|---|
| One sample mean, population standard deviation known | z-test |
| One sample mean, population standard deviation unknown | t-test |
| Two group means | two-sample t-test |
| Before-and-after data | paired t-test |
| One population proportion | one-proportion z-test |
| Categorical counts | chi-square test |
| More than two group means | ANOVA |
| Estimate parameter from likelihood | MLE |

---

## Common Mistakes ☆

{{% hint warning %}}
- Saying “accept the null hypothesis” instead of “fail to reject the null hypothesis”.
- Confusing confidence level with significance level.
- Using z-test when t-test is required.
- Forgetting degrees of freedom.
- Treating correlation as causation.
- Running multiple t-tests instead of ANOVA for more than two means.
{{% /hint %}}

---

## Why It Matters in AI and ML

Hypothesis testing is used in AI and ML to:

- compare two models statistically
- check whether a new model improvement is significant
- evaluate A/B tests
- assess feature importance
- validate assumptions in regression
- analyse experiment results

{{% hint success %}}
In ML, a higher score from one model is not always meaningful.

Hypothesis testing helps decide whether the improvement is likely real or just due to random sampling variation.
{{% /hint %}}

---

## Revision Checklist ☆

- Can I define {{< katex >}} H_0 {{< /katex >}} and {{< katex >}} H_1 {{< /katex >}} correctly?
- Do I know whether the test is one-tailed or two-tailed?
- Do I know which statistic to use?
- Did I compute the standard error correctly?
- Did I compare p-value with {{< katex >}} \alpha {{< /katex >}}?
- Did I write the final conclusion in the language of the problem?

---
{{< home-link "Home" >}} | {{< section-index >}}
