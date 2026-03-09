---
title: "Hypothesis Testing"
draft: false
tags: ["AI", "Statistics"]
categories: ["AI", "Statistics"]
weight: 400
menu: main
---

# Hypothesis Testing

Hypothesis testing is a structured way to decide:
“Is what I’m seeing in the sample just random noise, or is there real evidence of a change in the population?”

This topic sits inside **inferential statistics**:
we use a **sample** to make a statement about a **population**.

---

## Big picture roadmap

{{< mermaid >}}
flowchart TD
  A["Population<br/>Unknown truth"] --> B["Draw a sample<br/>Random or stratified"]
  B --> C["Compute statistic<br/>mean, proportion, variance"]
  C --> D["Sampling distribution<br/>standard error"]
  D --> E["Estimation<br/>point and interval (CI)"]
  D --> F["Hypothesis testing<br/>z-test / t-test / proportion tests"]
  F --> G["Decision<br/>reject or fail to reject H0"]
  E --> H["Interpretation<br/>confidence level, margin of error"]
  G --> I["Report<br/>p-value, conclusion in words"]
  H --> I
{{< /mermaid >}}

{{% hint info %}}
Key takeaway:
Hypothesis testing depends on the idea of a sampling distribution.
That is why we start with sampling and the Central Limit Theorem.
{{% /hint %}}

---

## Sampling

### Population vs sample

- **Population**:
the full set of elements you care about (usually huge).
- **Sample**:
a subset of the population that you can actually measure.

Why sample?
because measuring the full population can be too expensive in:
money, manpower, material, and time.

---

## Sampling methods

### Random sampling

Random sampling means:
every unit has a known chance of being selected,
and selection is not biased.

Examples of how randomness is implemented:
- random number generator on a list (sampling frame)
- random selection of IDs (roll numbers, employee IDs)

{{% hint warning %}}
If your sample is biased, your estimate will be biased.
Sampling quality matters before any formula.
{{% /hint %}}

---

### Stratified sampling

Use stratified sampling when the population is **heterogeneous**.

Steps:
1) split the population into strata (groups) so each stratum is internally similar  
2) take random samples inside each stratum  
3) combine results using a weighted average (weights based on stratum sizes)

Why it helps:
you avoid over-sampling one group and under-sampling another.

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

## Sampling distribution

A **statistic** (like sample mean) becomes a random variable when you repeat sampling.
The probability distribution of that statistic is called the **sampling distribution**.

Examples:
- sampling distribution of the sample mean
- sampling distribution of the sample proportion

---

## Central Limit Theorem

### CLT for the sample mean

If you take a random sample of size $n$ from a population with mean $\mu$ and standard deviation $\sigma$:

- for sufficiently large $n$ (rule-of-thumb: $n\ge 30$),
the distribution of the sample mean $\bar{x}$ is approximately normal.

Also:
if the population itself is normal, then $\bar{x}$ is normal for any $n$.

Mean and standard deviation of the sampling distribution:

{{< colour "red" >}}
$$
E(\bar{x})=\mu
$$
{{< /colour >}}

{{< colour "red" >}}
$$
\sigma_{\bar{x}}=\frac{\sigma}{\sqrt{n}}
$$
{{< /colour >}}

The quantity $\sigma_{\bar{x}}$ is called the **standard error of the mean**.

---

### Z score for sample means

Once we know the sampling distribution is normal (or approximately normal),
we can convert sample means into Z scores.

{{< colour "red" >}}
$$
Z=\frac{\bar{x}-\mu}{\sigma/\sqrt{n}}
$$
{{< /colour >}}

This is used for:
- probability questions about $\bar{x}$
- confidence intervals for the mean (when $\sigma$ is known)
- z-tests for the mean (one-sample)

---

### Finite population correction

If the population is finite (fixed size $N$) and sampling is without replacement,
a correction factor can be used:

{{< colour "red" >}}
$$
\text{FPC}=\sqrt{\frac{N-n}{N-1}}
$$
{{< /colour >}}

Then the standard error becomes:

{{< colour "red" >}}
$$
\sigma_{\bar{x}}=\frac{\sigma}{\sqrt{n}}\,\sqrt{\frac{N-n}{N-1}}
$$
{{< /colour >}}

Rule of thumb:
if $\frac{n}{N}<0.05$, the correction is usually small.

---

## Sample proportion and its sampling distribution

### Sample proportion

When the data are “countable” (yes/no, success/failure),
we use the sample proportion.

Let $x$ be the number of “successes” in a sample of size $n$:

{{< colour "red" >}}
$$
\hat{p}=\frac{x}{n}
$$
{{< /colour >}}

---

### CLT for sample proportion

The sampling distribution of $\hat{p}$ is approximately normal when:

- $np > 15$
- $n(1-p) > 15$

Mean and standard deviation:

{{< colour "red" >}}
$$
E(\hat{p})=p
$$
{{< /colour >}}

{{< colour "red" >}}
$$
\sigma_{\hat{p}}=\sqrt{\frac{p(1-p)}{n}}
$$
{{< /colour >}}

(When $p$ is unknown in practice, we often plug in $\hat{p}$.)

---

## Estimation – Interval Estimation,Confidence level 

Statistical inference has three common forms:
- point estimation
- interval estimation (confidence intervals)
- hypothesis testing

---

### Point estimation

A **point estimate** is a single best guess for a population parameter.

Examples:
- $\bar{x}$ estimates $\mu$
- $s^2$ estimates $\sigma^2$
- $\hat{p}$ estimates $p$

{{% hint info %}}
Point estimates are easy to compute, but they do not show uncertainty.
That is why we use confidence intervals.
{{% /hint %}}

---

### Interval estimation and confidence intervals

A **confidence interval (CI)** gives a range that is likely to contain the true parameter.

Confidence level:
$100(1-\alpha)\%$

Common values:
- 90% (α = 0.10)
- 95% (α = 0.05)
- 99% (α = 0.01)

The “critical value”:
$Z_{\alpha/2}$ comes from the standard normal table.

Typical:
- for 95%: $Z_{\alpha/2}=1.96$
- for 99%: $Z_{\alpha/2}=2.58$

---

## Confidence interval for the mean

Use this when:
- population standard deviation $\sigma$ is known
- sample size is large, or population is normal

Standard error:

{{< colour "red" >}}
$$
SE(\bar{x})=\frac{\sigma}{\sqrt{n}}
$$
{{< /colour >}}

Confidence interval:

{{< colour "red" >}}
$$
\mu\in \bar{x}\pm Z_{\alpha/2}\,SE(\bar{x})
=\bar{x}\pm Z_{\alpha/2}\frac{\sigma}{\sqrt{n}}
$$
{{< /colour >}}

Margin of error:

{{< colour "red" >}}
$$
E=Z_{\alpha/2}\frac{\sigma}{\sqrt{n}}
$$
{{< /colour >}}

Interpretation:
“we are $(1-\alpha)$ confident that the interval contains $\mu$.”

{{% hint warning %}}
A 95% confidence interval does NOT mean:
“there is a 95% chance $\mu$ is in this particular interval”.
It means:
the method produces intervals that capture $\mu$ about 95% of the time in repeated sampling.
{{% /hint %}}

---

## Confidence interval for difference of two means

Use when comparing two population means.

A common large-sample form (using sample SDs):

{{< colour "red" >}}
$$
(\mu_1-\mu_2)\in (\bar{x}_1-\bar{x}_2)
\pm Z_{\alpha/2}\sqrt{\frac{s_1^2}{n_1}+\frac{s_2^2}{n_2}}
$$
{{< /colour >}}

Pooled standard deviation approach (when population SDs are assumed equal):
often used in two-sample t procedures, discussed later in more detail.

---

## Confidence interval for a proportion

Standard error (using $\hat{p}$ in practice):

{{< colour "red" >}}
$$
SE(\hat{p})=\sqrt{\frac{\hat{p}(1-\hat{p})}{n}}
$$
{{< /colour >}}

Confidence interval:

{{< colour "red" >}}
$$
p\in \hat{p}\pm Z_{\alpha/2}\,SE(\hat{p})
=\hat{p}\pm Z_{\alpha/2}\sqrt{\frac{\hat{p}(1-\hat{p})}{n}}
$$
{{< /colour >}}

---

## Confidence interval for difference in proportions

Let $\hat{p}_1=\frac{x_1}{n_1}$ and $\hat{p}_2=\frac{x_2}{n_2}$.

Standard error:

{{< colour "red" >}}
$$
SE(\hat{p}_1-\hat{p}_2)=\sqrt{\frac{\hat{p}_1(1-\hat{p}_1)}{n_1}+\frac{\hat{p}_2(1-\hat{p}_2)}{n_2}}
$$
{{< /colour >}}

Confidence interval:

{{< colour "red" >}}
$$
(p_1-p_2)\in (\hat{p}_1-\hat{p}_2)\pm Z_{\alpha/2}\,SE(\hat{p}_1-\hat{p}_2)
$$
{{< /colour >}}

---

## Testing of hypothesis

Hypothesis testing is about comparing two claims:

- $H_0$ (null hypothesis):
“nothing changed” / “no effect” / “baseline”
- $H_1$ (alternative hypothesis):
“something changed” / “effect exists”

A hypothesis test outputs:
- a **test statistic** (like $Z$)
- a **p-value**
- a decision:
reject $H_0$ or fail to reject $H_0$

---

## The test workflow (exam-friendly)

{{< mermaid >}}
flowchart TD
  A["1. Define H0 and H1"] --> B["2. Choose alpha<br/>0.10, 0.05, 0.01"]
  B --> C["3. Pick test statistic<br/>Z or t or chi-square or F"]
  C --> D["4. Compute statistic from sample"]
  D --> E["5. Compute p-value or critical region"]
  E --> F{"6. Decision"}
  F -->|p <= alpha| G["Reject H0"]
  F -->|p > alpha| H["Fail to reject H0"]
  G --> I["7. Write conclusion in words"]
  H --> I
{{< /mermaid >}}

---

## Errors in testing

Two key risks:

- Type I error:
reject $H_0$ when $H_0$ is true  
Probability = $\alpha$

- Type II error:
fail to reject $H_0$ when $H_1$ is true  
Probability = $\beta$

Power of the test:
$1-\beta$

---

## Mean-based hypothesis tests (z-test idea)

Use a one-sample mean test when:
- you test a claim about $\mu$
- $\sigma$ is known (or $n$ is large and you use $s$ as an approximation)

Test statistic:

{{< colour "red" >}}
$$
Z=\frac{\bar{x}-\mu_0}{\sigma/\sqrt{n}}
$$
{{< /colour >}}

Where $\mu_0$ is the mean claimed by $H_0$.

Decision approaches:
- critical value method:
compare $Z$ to $Z_{\alpha/2}$ (two-tailed) or $Z_{\alpha}$ (one-tailed)
- p-value method:
compute p-value from the Z table

---

## Proportion-based hypothesis tests (z-test idea)

Use when:
- binary outcomes
- testing a claim about $p$

Test statistic (basic large-sample form):

{{< colour "red" >}}
$$
Z=\frac{\hat{p}-p_0}{\sqrt{\frac{p_0(1-p_0)}{n}}}
$$
{{< /colour >}}

Where $p_0$ is the proportion claimed by $H_0$.

---

## ANOVA

ANOVA is used when:
you compare **more than two means** at once.

Instead of doing many pairwise tests (which increases Type I error),
ANOVA uses an **F statistic**.

- Single-factor ANOVA:
one categorical factor (one “grouping variable”)
- Two-factor ANOVA:
two factors (and possibly interaction effects)

High-level idea:
compare variation **between groups** to variation **within groups**.

{{% hint info %}}
If “between-group variation” is much larger than “within-group variation”,
it suggests at least one group mean is different.
{{% /hint %}}

---

## Maximum likelihood (MLE)

Maximum likelihood is a method for estimating model parameters.

Idea:
choose the parameter values that make the observed data most probable.

Let the data be $x_1,\dots,x_n$ and the model have parameter $\theta$.

Likelihood:

{{< colour "red" >}}
$$
L(\theta)=\prod_{i=1}^{n} f(x_i\mid \theta)
$$
{{< /colour >}}

Log-likelihood (often easier):

{{< colour "red" >}}
$$
\ell(\theta)=\log L(\theta)=\sum_{i=1}^{n} \log f(x_i\mid \theta)
$$
{{< /colour >}}

MLE:

{{< colour "red" >}}
$$
\hat{\theta}_{\text{MLE}}=\arg\max_{\theta}\; L(\theta)
=\arg\max_{\theta}\; \ell(\theta)
$$
{{< /colour >}}

Quick examples you should remember:
- Bernoulli trials:
MLE for $p$ is $\hat{p}=x/n$
- Normal with known $\sigma$:
MLE for $\mu$ is $\bar{x}$

MLE is important in ML because many models are trained by:
maximising likelihood (or equivalently minimising negative log-likelihood).

---

## Mini-check (self-test)

1) What does “95% confidence” mean in repeated sampling?  
2) What is the standard error of the mean?  
3) In hypothesis testing, what does $\alpha$ represent?  
4) What is the difference between “reject $H_0$” and “fail to reject $H_0$”?

{{% hint success %}}
1) The method captures the true parameter about 95% of the time over repeated samples.  
2) $\sigma/\sqrt{n}$ (or with FPC for finite populations).  
3) Probability of Type I error.  
4) “Fail to reject” is not the same as “accept”; it means the sample evidence is not strong enough.
{{% /hint %}}

---

## Reference

- [Hypothesis Testing](https://www.geeksforgeeks.org/data-science/understanding-hypothesis-testing/)

---

{{< home-link "Home" >}} | {{< section-index >}}
