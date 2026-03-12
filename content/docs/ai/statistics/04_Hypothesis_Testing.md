---
title: "Hypothesis Testing"
date: 2026-03-12
draft: false
tags: ["AI", "Statistics"]
categories: ["AI", "Statistics"]
weight: 400
menu: main
---

# Hypothesis Testing

Hypothesis testing is a structured way to decide:

Is what we see in a sample just random variation,
or is there evidence of a real effect in the population?

Hypothesis Testing topic sits inside **inferential statistics**:
we use a **sample** to make a statement about a **population**.

- Sampling (random and stratified)
- Sampling distribution and Central Limit Theorem
- Estimation (confidence intervals and confidence level)
- Testing hypotheses (mean, proportion, ANOVA)
- Maximum likelihood (MLE)

{{% hint info %}}
Key takeaway:
The logic is always the same:

1) define a claim (null) and a competing claim (alternative)  
2) compute a test statistic from the sample  
3) use a threshold (critical value) or a p-value  
4) make a decision and write the conclusion in words
{{% /hint %}}

---

## Big picture roadmap

{{< mermaid >}}
%%{init: {'theme':'base','themeVariables': {
  'fontFamily':'Inter, ui-sans-serif, system-ui',
  'primaryColor':'#E8F1FF',
  'primaryTextColor':'#1F2937',
  'primaryBorderColor':'#A7C7FF',
  'lineColor':'#94A3B8',
  'tertiaryColor':'#F8FAFC'
}}}%%
flowchart TD
  A["Population<br/>unknown truth"] --> B["Sampling<br/>random / stratified"]
  B --> C["Statistic from sample<br/>mean / proportion / variance"]
  C --> D["Sampling distribution<br/>standard error"]
  D --> E["Estimation<br/>confidence intervals"]
  D --> F["Hypothesis test<br/>Z / t / proportion / F"]
  E --> G["Interpretation<br/>confidence level, margin of error"]
  F --> H["Decision<br/>reject H0 or fail to reject H0"]
  H --> I["Report<br/>p-value + conclusion in words"]
  G --> I
{{< /mermaid >}}

{{% hint info %}}
Key takeaway:
Hypothesis testing depends on the idea of a sampling distribution.
That is why we start with sampling and the Central Limit Theorem.
{{% /hint %}}

---

## 1) Sampling

### 1.1 Population vs sample

- **Population**:
the full set of elements you care about (usually huge).
- **Sample**:
a subset of the population that you can actually measure.

Why sample?
because measuring the full population can be too expensive in:
money, manpower, material, and time.

---

### 1.2 Random sampling

Random sampling means:
each element has a known chance of selection,
and selection is not biased.

Typical approach:
- assign IDs to the population list (sampling frame)
- use a random number generator to pick IDs

---

### 1.3 Stratified sampling

Use stratified sampling when the population is heterogeneous.

Steps:
1) split population into strata (groups)
2) take a random sample within each stratum
3) combine results using weights proportional to stratum sizes

Why it helps:
it prevents under-representing important subgroups.

---

## Sampling error and sampling variation

Sampling is random, so estimates vary from sample to sample.

- **Sampling variation**:
the natural variability of sample statistics across different samples.
- **Sampling error**:
the difference between a sample estimate and the true (unknown) population parameter.

A useful mental model:

Observed data = Truth + Bias + Random error
Sampling design tries to reduce bias and control random error.

---
## 2) Sampling distributions and the Central Limit Theorem

### 2.1 Sampling distribution

A statistic (like the sample mean) becomes a random variable if we repeat sampling.
Its distribution over repeated samples is the sampling distribution.

This is the key reason we can quantify uncertainty.

---

### 2.2 Central Limit Theorem for the sample mean

If you take a random sample of size {{< katex >}}n{{< /katex >}} from a population with mean {{< katex >}}\mu{{< /katex >}} and standard deviation {{< katex >}}\sigma{{< /katex >}}:

- for sufficiently large {{< katex >}}n{{< /katex >}} (rule of thumb: {{< katex >}}n\ge 30{{< /katex >}}),
the distribution of the sample mean {{< katex >}}\bar{x}{{< /katex >}} is approximately normal
- if the population itself is normal, then {{< katex >}}\bar{x}{{< /katex >}} is normal for any {{< katex >}}n{{< /katex >}}

Mean and standard deviation of the sampling distribution:

{{< colour "red" >}}
{{< katex display=true >}}
E(\bar{x})=\mu
{{< /katex >}}
{{< /colour >}}

{{< colour "red" >}}
{{< katex display=true >}}
\sigma_{\bar{x}}=\frac{\sigma}{\sqrt{n}}
{{< /katex >}}
{{< /colour >}}

The quantity {{< katex >}}\sigma_{\bar{x}}{{< /katex >}} is the standard error of the mean.

---

### 2.3 Z-score for sample means

{{< colour "red" >}}
{{< katex display=true >}}
Z=\frac{\bar{x}-\mu}{\sigma/\sqrt{n}}
{{< /katex >}}
{{< /colour >}}

This lets you convert a sample mean into a standard normal value,
so you can compute probabilities and thresholds.

---

### 2.4 CLT for the sample proportion

For a binary variable (success/failure),
let {{< katex >}}\hat{p}=x/n{{< /katex >}}.

Approximate normality holds when:
{{< katex >}}np{{< /katex >}} and {{< katex >}}n(1-p){{< /katex >}} are both sufficiently large.

Mean and standard error:

{{< colour "red" >}}
{{< katex display=true >}}
E(\hat{p})=p
{{< /katex >}}
{{< /colour >}}

{{< colour "red" >}}
{{< katex display=true >}}
\sigma_{\hat{p}}=\sqrt{\frac{p(1-p)}{n}}
{{< /katex >}}
{{< /colour >}}

In practice, when {{< katex >}}p{{< /katex >}} is unknown, we often plug in {{< katex >}}\hat{p}{{< /katex >}}.

---

## 3) Estimation (confidence intervals)

### 3.1 Confidence level

A confidence level is written as:
{{< katex >}}100(1-\alpha)\%{{< /katex >}}.

Common values:
- 90% ({{< katex >}}\alpha=0.10{{< /katex >}})
- 95% ({{< katex >}}\alpha=0.05{{< /katex >}})
- 99% ({{< katex >}}\alpha=0.01{{< /katex >}})

---

### 3.2 Confidence interval for the mean (sigma known)

Standard error:

{{< colour "red" >}}
{{< katex display=true >}}
SE(\bar{x})=\frac{\sigma}{\sqrt{n}}
{{< /katex >}}
{{< /colour >}}

Confidence interval:

{{< colour "red" >}}
{{< katex display=true >}}
\mu\in \bar{x}\pm Z_{\alpha/2}\,\frac{\sigma}{\sqrt{n}}
{{< /katex >}}
{{< /colour >}}

Margin of error:

{{< colour "red" >}}
{{< katex display=true >}}
E=Z_{\alpha/2}\,\frac{\sigma}{\sqrt{n}}
{{< /katex >}}
{{< /colour >}}

Interpretation:
the method produces intervals that capture {{< katex >}}\mu{{< /katex >}} about {{< katex >}}100(1-\alpha)\%{{< /katex >}} of the time under repeated sampling.

---

### 3.3 Confidence interval for a proportion

Standard error (using {{< katex >}}\hat{p}{{< /katex >}}):

{{< colour "red" >}}
{{< katex display=true >}}
SE(\hat{p})=\sqrt{\frac{\hat{p}(1-\hat{p})}{n}}
{{< /katex >}}
{{< /colour >}}

Confidence interval:

{{< colour "red" >}}
{{< katex display=true >}}
p\in \hat{p}\pm Z_{\alpha/2}\sqrt{\frac{\hat{p}(1-\hat{p})}{n}}
{{< /katex >}}
{{< /colour >}}

---

### 3.4 Difference of two means (large-sample form)

{{< colour "red" >}}
{{< katex display=true >}}
(\mu_1-\mu_2)\in (\bar{x}_1-\bar{x}_2)\pm Z_{\alpha/2}\sqrt{\frac{s_1^2}{n_1}+\frac{s_2^2}{n_2}}
{{< /katex >}}
{{< /colour >}}

---

### 3.5 Difference of two proportions

{{< colour "red" >}}
{{< katex display=true >}}
SE(\hat{p}_1-\hat{p}_2)=\sqrt{\frac{\hat{p}_1(1-\hat{p}_1)}{n_1}+\frac{\hat{p}_2(1-\hat{p}_2)}{n_2}}
{{< /katex >}}
{{< /colour >}}

{{< colour "red" >}}
{{< katex display=true >}}
(p_1-p_2)\in (\hat{p}_1-\hat{p}_2)\pm Z_{\alpha/2}\,SE(\hat{p}_1-\hat{p}_2)
{{< /katex >}}
{{< /colour >}}

---

## 4) Testing of hypothesis

### 4.1 Hypotheses

- Null hypothesis {{< katex >}}H_0{{< /katex >}}:
baseline claim (no change / no effect)
- Alternative hypothesis {{< katex >}}H_1{{< /katex >}}:
competing claim (change / effect)

A hypothesis test outputs:
- a test statistic ({{< katex >}}Z{{< /katex >}}, {{< katex >}}t{{< /katex >}}, {{< katex >}}\chi^2{{< /katex >}}, {{< katex >}}F{{< /katex >}})
- a p-value (or a critical region decision)
- a final conclusion

---

### 4.2 Errors in testing

- Type I error:
reject {{< katex >}}H_0{{< /katex >}} when {{< katex >}}H_0{{< /katex >}} is true  
probability = {{< katex >}}\alpha{{< /katex >}}

- Type II error:
fail to reject {{< katex >}}H_0{{< /katex >}} when {{< katex >}}H_1{{< /katex >}} is true  
probability = {{< katex >}}\beta{{< /katex >}}

Power of a test:
{{< katex >}}1-\beta{{< /katex >}}

---

### 4.3 Test workflow

{{< mermaid >}}
%%{init: {'theme':'base','themeVariables': {
  'fontFamily':'Inter, ui-sans-serif, system-ui',
  'primaryColor':'#FDEBFF',
  'primaryTextColor':'#1F2937',
  'primaryBorderColor':'#F5B7FF',
  'lineColor':'#94A3B8',
  'tertiaryColor':'#F8FAFC'
}}}%%
flowchart TD
  A["Define hypotheses<br/>H0 and H1"] --> B["Choose significance level<br/>alpha = 0.10 or 0.05 or 0.01"]
  B --> C["Select test statistic<br/>Z or t or chi-square or F"]
  C --> D["Compute statistic<br/>from sample data"]
  D --> E["Compute p-value<br/>or compare with critical value"]
  E --> F["Decision"]
  F --> G["If p <= alpha<br/>Reject H0"]
  F --> H["If p > alpha<br/>Fail to reject H0"]
  G --> I["Conclusion in words"]
  H --> I
{{< /mermaid >}}

---

## 5) Mean-based tests (one mean)

Use this when testing a claim about {{< katex >}}\mu{{< /katex >}}.

A common Z-form (when {{< katex >}}\sigma{{< /katex >}} is known):

{{< colour "red" >}}
{{< katex display=true >}}
Z=\frac{\bar{x}-\mu_0}{\sigma/\sqrt{n}}
{{< /katex >}}
{{< /colour >}}

Where {{< katex >}}\mu_0{{< /katex >}} is the mean stated in {{< katex >}}H_0{{< /katex >}}.

Decision approaches:
- critical value method:
compare {{< katex >}}Z{{< /katex >}} to {{< katex >}}Z_{\alpha/2}{{< /katex >}} (two-tailed) or {{< katex >}}Z_{\alpha}{{< /katex >}} (one-tailed)
- p-value method:
compute the p-value from the Z table

---

## 6) Proportion-based tests (one proportion)

Use when outcomes are binary and you test a claim about {{< katex >}}p{{< /katex >}}.

{{< colour "red" >}}
{{< katex display=true >}}
Z=\frac{\hat{p}-p_0}{\sqrt{\frac{p_0(1-p_0)}{n}}}
{{< /katex >}}
{{< /colour >}}

Where {{< katex >}}p_0{{< /katex >}} is the proportion stated in {{< katex >}}H_0{{< /katex >}}.

---

## 7) ANOVA (single and dual factor)

ANOVA is used when comparing more than two means.

Instead of running many pairwise tests (which inflates Type I error),
ANOVA uses an {{< katex >}}F{{< /katex >}} statistic.

- Single-factor ANOVA:
one categorical factor (one grouping variable)
- Two-factor ANOVA:
two factors (and possibly an interaction effect)

Core idea:
compare variation between groups to variation within groups.
If between-group variation is much larger, at least one mean is different.

---

## 8) Maximum likelihood (MLE)

Maximum likelihood estimates model parameters by choosing values that make the observed data most probable.

Let the data be {{< katex >}}x_1,\dots,x_n{{< /katex >}} and parameter {{< katex >}}\theta{{< /katex >}}.

Likelihood:

{{< colour "red" >}}
{{< katex display=true >}}
L(\theta)=\prod_{i=1}^{n} f(x_i\mid \theta)
{{< /katex >}}
{{< /colour >}}

Log-likelihood:

{{< colour "red" >}}
{{< katex display=true >}}
\ell(\theta)=\log L(\theta)=\sum_{i=1}^{n} \log f(x_i\mid \theta)
{{< /katex >}}
{{< /colour >}}

MLE:

{{< colour "red" >}}
{{< katex display=true >}}
\hat{\theta}_{\text{MLE}}=\arg\max_{\theta}\; L(\theta)
=\arg\max_{\theta}\; \ell(\theta)
{{< /katex >}}
{{< /colour >}}

Quick examples:
- Bernoulli trials:
{{< katex >}}\hat{p}=x/n{{< /katex >}}
- Normal with known {{< katex >}}\sigma{{< /katex >}}:
{{< katex >}}\hat{\mu}=\bar{x}{{< /katex >}}

---

## Mini-check (self-test)

1) What does “95% confidence” mean in repeated sampling?  
2) What is the standard error of the mean?  
3) In hypothesis testing, what does {{< katex >}}\alpha{{< /katex >}} represent?  
4) What is the difference between “reject {{< katex >}}H_0{{< /katex >}}” and “fail to reject {{< katex >}}H_0{{< /katex >}}”?  
5) Why is ANOVA preferred over multiple pairwise mean tests?

Answers:
1) The method captures the true parameter about 95% of the time over repeated samples.  
2) {{< katex >}}\sigma/\sqrt{n}{{< /katex >}} (or with finite population correction if needed).  
3) Probability of Type I error.  
4) Fail to reject means evidence is not strong enough; it does not mean {{< katex >}}H_0{{< /katex >}} is proven true.  
5) It controls Type I error inflation when comparing several means.

---

## Reference

- [Hypothesis Testing](https://www.geeksforgeeks.org/data-science/understanding-hypothesis-testing/)

---

{{< home-link "Home" >}} | {{< section-index >}}

---
