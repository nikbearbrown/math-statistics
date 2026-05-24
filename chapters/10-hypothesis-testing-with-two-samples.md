# Chapter 10 — Hypothesis Testing with Two Samples
*When One Group Isn't Enough to Tell You Anything.*

The question that drives almost everything in applied statistics is not "what is this group like?" It is "are these two groups different?"

Does the drug work better than the placebo? Does the new manufacturing process produce fewer defects than the old one? Does the unemployment rate in one city genuinely exceed the other's, or did the survey just happen to catch more unemployed people in one place? Does the rehabilitation protocol actually help, or did patients improve simply because time passed?

In each case you have two groups. You measure something in each. You observe a difference. And then you face the central question: is this difference real, or is it noise?

The machinery from Chapter 9 — null hypothesis, test statistic, p-value — does not change. What changes is the thing being tested. Now the null hypothesis is not "this population has mean 50." It is "these two populations have the same mean." And that shift from one group to two introduces a new random variable that we have not seen before: the difference between two sample means.

---

## The difference as a random variable

Start with the fundamental idea. You draw a sample from Population 1 and compute its mean, $\bar{x}_1$. You draw an independent sample from Population 2 and compute $\bar{x}_2$. The difference $\bar{x}_1 - \bar{x}_2$ is a number. But it is also a random variable — if you repeated the whole process, drawing fresh samples each time, you would get a different difference. That difference has a distribution, a mean, and a standard deviation.

What is the distribution of $\bar{x}_1 - \bar{x}_2$?

![Sampling distribution of the difference of two means](images/10-hypothesis-testing-with-two-samples-fig-01.png)
*Figure 10.1 — Sampling distribution of the difference of two means*

The Central Limit Theorem tells you that each sample mean is approximately normally distributed. And there is a general rule about the sum or difference of independent normal random variables: it is also normal. So the difference of two sample means is approximately normal. Its mean is $\mu_1 - \mu_2$, the true difference between population means. Under the null hypothesis that $\mu_1 = \mu_2$, this mean is zero.

Its standard deviation — the standard error of the difference — is:

$$\text{SE}(\bar{x}_1 - \bar{x}_2) = \sqrt{\frac{s_1^2}{n_1} + \frac{s_2^2}{n_2}}$$

This formula is worth examining. The variance of a difference of independent random variables is the *sum* of their variances. Group 1 contributes variance $s_1^2 / n_1$ to the difference; Group 2 contributes $s_2^2 / n_2$. They add because the groups are independent — the random fluctuation in one sample's mean is unrelated to the random fluctuation in the other's, and independent sources of uncertainty add.

Notice that a large, tight sample contributes less uncertainty than a small, variable one. If Group 1 is measured with $n_1 = 1000$ and small variance, its contribution to the standard error is tiny. If Group 2 has $n_2 = 20$ and large variance, it dominates. The formula weights by both sample size and spread.

The test statistic then follows the same logic as Chapter 9. We standardize the observed difference by dividing by its standard error:

$$t = \frac{(\bar{x}_1 - \bar{x}_2) - \delta_0}{\sqrt{\frac{s_1^2}{n_1} + \frac{s_2^2}{n_2}}}$$

where $\delta_0$ is the hypothesized difference under the null hypothesis — usually zero, because we are testing whether the two populations are equal. This test statistic follows a t-distribution, and we find the p-value by asking how likely it is to observe a $|t|$ this large or larger if the null hypothesis is true.

---

## A concrete case: two manufacturing plants

A company makes brake pads at two plants. Quality engineers sample 35 pads from the Midwest facility (mean lifespan 49,200 miles, standard deviation 3,100) and 42 pads from the West Coast facility (mean 50,800 miles, standard deviation 2,900). The Midwest pads lasted 1,600 miles less on average. Is the Midwest plant drifting out of specification, or did the sample just happen to catch an off day?

| Item | Meaning |
| --- | --- |
| sample size (n | A concrete checkpoint for applying the chapter concept. |
| sample mean (x̄ | A concrete checkpoint for applying the chapter concept. |
| sample standard deviation (s | A concrete checkpoint for applying the chapter concept. |
| contribution to SE (s² | n). Last row: observed difference and computed SE. Helps students see the formula populated with real numbers before the arithmetic. |

The standard error of the difference is:

$$\text{SE} = \sqrt{\frac{3100^2}{35} + \frac{2900^2}{42}} = \sqrt{274,571 + 200,238} = \sqrt{474,809} \approx 689 \text{ miles}$$

The test statistic:

$$t = \frac{(49200 - 50800) - 0}{689} = \frac{-1600}{689} \approx -2.32$$

With approximately 75 degrees of freedom, a two-tailed p-value for $|t| = 2.32$ is about 0.023. Since $0.023 < 0.05$, we reject the null hypothesis. The Midwest plant's pads are running measurably shorter, and the difference is too large to attribute to sampling luck.

What does this conclusion rest on? It rests on the sampling distribution of the difference. If the two plants truly produced identical pads, how often would random sampling generate a 1,600-mile gap between the sample means? About 2.3% of the time. That is unusual enough — given our threshold of 5% — to conclude the plants are genuinely different.

---

## Pooled versus unpooled: a choice about assumptions

The test above used *unpooled* variances — treating the two population standard deviations as potentially different, and letting each sample's variance speak for itself. This is the conservative, safer approach. It is sometimes called the Welch t-test.

The alternative is *pooling*: assuming the two populations have the same standard deviation, and combining the two sample variances into a single estimate. The pooled variance is a weighted average:

$$s_p^2 = \frac{(n_1 - 1)s_1^2 + (n_2 - 1)s_2^2}{n_1 + n_2 - 2}$$

This gives a slightly simpler test statistic and exactly $n_1 + n_2 - 2$ degrees of freedom, with no approximation needed. When the two population standard deviations are truly equal, pooling is slightly more powerful — it uses the information from both samples more efficiently.

The problem is the assumption. You rarely know in advance whether the two population standard deviations are equal. If you assume they are equal and you are wrong, the pooled test can mislead you. The unpooled approach requires no such assumption and pays only a small cost in power. For this reason, most statisticians now recommend the unpooled approach as the default, reserving pooling for situations where equal variances are strongly supported by prior knowledge or preliminary tests.

| property | pooled | Welch unpooled |
| --- | --- | --- |
| assumption required, degrees of freedom formula, when to use, risk if assumption violated, software default. Student should see at a glance why Welch is the safer default. | It makes the underlying reasoning visible instead of implied. | A concrete checkpoint for applying the chapter concept. |

This is not a religious question. The two tests give similar results when sample sizes are roughly equal and sample variances are similar. They can diverge when one group is much larger or much more variable than the other. In those cases, the unpooled approach is more trustworthy.

---

## When the groups are not independent: the paired design

So far we have assumed the two groups are independent — the observations in one group bear no special relationship to any particular observation in the other. But sometimes the design creates natural pairs.

A physical therapist tests a rehabilitation protocol on twelve patients. She measures how far each patient can walk before the protocol begins, and again after six weeks of treatment. These are not two independent groups. Each patient appears twice — once in the before column and once in the after column. Patient 3's before measurement and Patient 3's after measurement are paired because they belong to the same person.

The independence assumption is violated. But something better is available.

For each patient, compute the difference: walking distance after minus walking distance before. Now you have twelve differences — a single sample of numbers representing individual change. The question transforms: is the mean of these differences equal to zero?

This is a one-sample t-test, applied to the differences. The test statistic is:

$$t = \frac{\bar{d} - 0}{s_d / \sqrt{n}}$$

where $\bar{d}$ is the mean of the twelve differences and $s_d$ is their standard deviation. Degrees of freedom is $n - 1 = 11$, where $n$ is the number of pairs.

Suppose the twelve differences (after minus before, in meters) are: 90, 60, 20, 90, 30, 100, −10, 50, 80, 40, 70, 60. Their mean is $\bar{d} = 760/12 \approx 63.3$ meters. Their standard deviation is approximately 38.2 meters. The standard error is $38.2 / \sqrt{12} \approx 11.0$ meters. The t-statistic is $63.3 / 11.0 \approx 5.75$.

| Patient ID | Before (meters) | After (meters) | Difference (after − before) |
| --- | --- | --- | --- |
| mean of differences (63.3 m) and SD of differences (38.2 m). Student should see how the two-column problem collapses to a single column of differences. | A concrete checkpoint for applying the chapter concept. | A concrete checkpoint for applying the chapter concept. | A concrete checkpoint for applying the chapter concept. |

With 11 degrees of freedom, a t-statistic of 5.75 gives a p-value far below 0.001. The protocol significantly improved walking distance.

Why does pairing help? Because people differ enormously from each other. Some patients start stronger, some weaker. If you ignored the pairing — treating the twelve before-measurements and twelve after-measurements as two independent groups — that between-person variability would swamp the signal. One patient who starts at 150 meters and ends at 240 is buried among others who start at 100 or 180. The mean of the after group would be compared to the mean of the before group, and all that individual variation would make the comparison noisy.

By looking only at the *change within each person*, pairing removes the between-person noise entirely. The test becomes sharper. The same true effect produces a larger t-statistic and a smaller p-value when the design exploits pairing.

![Illustration of pairing vs](images/10-hypothesis-testing-with-two-samples-fig-02.png)
*Figure 10.2 — Illustration of pairing vs*

The flip side: pairing only helps if the pairs are genuinely related. If you sort two unrelated groups by some irrelevant criterion and call them "paired," you impose a structure that does not exist, waste a degree of freedom for every pair, and make your test worse. Pairing is a design feature, not a post-hoc trick.

---

## Comparing proportions

The same logic extends to proportions. Two cities survey unemployment. City A contacts 400 households and finds 48 where someone is currently seeking work — a sample proportion of 0.12. City B contacts 450 households and finds 45 unemployed — a proportion of 0.10. Is City A's unemployment rate genuinely higher, or is the 2-percentage-point gap within the range of sampling noise?

The null hypothesis is that both cities have the same true unemployment rate, $p_1 = p_2$. Under this null, the best estimate of the common rate is the pooled proportion, combining both samples:

$$p_c = \frac{48 + 45}{400 + 450} = \frac{93}{850} \approx 0.109$$

The standard error of the difference in proportions, under the null:

$$\text{SE} = \sqrt{p_c(1 - p_c)\!\left(\frac{1}{n_1} + \frac{1}{n_2}\right)} = \sqrt{0.109 \cdot 0.891 \cdot (0.0025 + 0.0022)} \approx 0.0214$$

The test statistic:

$$z = \frac{(0.12 - 0.10) - 0}{0.0214} = \frac{0.02}{0.0214} \approx 0.93$$

This is a z-test rather than a t-test because proportions, unlike means, have a known relationship between variance and mean ($\sigma^2 = np(1-p)$), so there is no free parameter to estimate. A z-statistic of 0.93 gives a two-tailed p-value of about 0.35.

$0.35 > 0.05$: fail to reject the null. The 2-percentage-point difference is unremarkable — you would see a gap this large or larger about 35% of the time from pure sampling variation, even if the cities had identical unemployment rates.

The pooled proportion appears in the denominator specifically because we are testing the null hypothesis that $p_1 = p_2$. Under that null, both samples are drawing from the same population, and the best estimate of that common proportion combines all the available data. If instead you were constructing a confidence interval for $p_1 - p_2$ — not testing equality but estimating the gap — you would use the unpooled version, treating each sample proportion as an independent estimate.

---

## Statistical significance versus practical importance

There is a trap that becomes particularly dangerous with two-sample tests, and it is worth naming directly.

A small p-value tells you the observed difference is unlikely to be noise. It does not tell you the difference is important. With large enough samples, you can achieve statistical significance for differences that are trivially small in practice.

Consider a drug trial enrolling 10,000 patients per arm. The treatment group shows a mean reduction in systolic blood pressure of 1.2 mmHg; the control group shows 0.9 mmHg. The difference is 0.3 mmHg. With samples this large, the standard error of the difference is tiny, and the t-statistic is enormous. The p-value is essentially zero. But no cardiologist considers a 0.3 mmHg reduction clinically meaningful — it is below the measurement error of most blood pressure cuffs and has no discernible effect on patient outcomes.

This is why effect size must accompany every p-value. For two means, the standard measure is **Cohen's d**: the difference between the means divided by the pooled standard deviation.

$$d = \frac{\bar{x}_1 - \bar{x}_2}{s_p}$$

Cohen's rough benchmarks: $d = 0.2$ is small, $d = 0.5$ is medium, $d = 0.8$ is large. For the brake pads, the effect size was about $d = 0.53$ — medium, practically meaningful for warranty and customer satisfaction purposes. For the blood pressure drug above, $d$ would be tiny, and no amount of statistical significance changes that.

![Scatter plot or dot plot with two axes](images/10-hypothesis-testing-with-two-samples-fig-03.png)
*Figure 10.3 — Scatter plot or dot plot with two axes*

The p-value answers: "Could this have happened by chance?" Effect size answers: "Does it matter?" Both questions are worth asking. A result that is statistically significant and practically important is compelling. A result that is statistically significant but practically negligible is technically real and practically useless.

---

## Three tests, one structure

The three tests in this chapter — independent means, paired means, proportions — look different on the surface but have the same skeleton.

In each case, you identify a quantity that represents the difference between two groups (the difference of sample means, the mean of paired differences, the difference of sample proportions). You compute its standard error. You standardize by dividing the observed difference by the standard error, producing a test statistic. You ask how extreme that test statistic is under the appropriate distribution (t or z). And you compare the resulting p-value to your significance threshold.

![Decision tree for choosing among the three two-sample](images/10-hypothesis-testing-with-two-samples-fig-04.png)
*Figure 10.4 — Decision tree for choosing among the three two-sample*

The choice of which test to use comes down to three questions. First: are the samples independent or paired? Paired data collapse the problem to a one-sample test on differences. Independent data require the full two-sample machinery. Second: are you comparing means or proportions? Means use the t-distribution; proportions use the z. Third, for means: do you assume equal population variances? Unpooled (Welch) is the safer default.

Get those three questions right and you know which formula to use. Get the first one wrong — treating paired data as independent — and your test will be noisier than it needs to be, potentially missing a real effect. The structure is more important than the formulas.

What you are ultimately doing in every case is the same thing. You are asking: given a world where these two populations are identical, how surprised should I be by the difference I actually observed? If the answer is "very surprised" — if the p-value is small — you have evidence that the world is not one where the populations are identical. If the answer is "not particularly surprised," the data are consistent with the null, and you have not found a difference worth reporting.

---

## LLM Exercise — Chapter 10: Hypothesis Testing with Two Samples (Analyze One Dataset Project)

**Project:** Analyze One Real Dataset.  
**What you're building this chapter:** a two-sample comparison test.  
**Tool:** **Claude Code** + **Claude Project**.

---

**The Prompt:**

```
Chapter 10 of my Analyze-One-Dataset project. Chapter 10 covered:
two independent samples (compare two distinct groups) → two-sample
t-test for means, two-proportion z-test for proportions; two paired
samples (same subjects measured twice, or matched pairs) → paired
t-test; the pooled vs. unpooled variance choice (Welch's t-test is
the safer default — doesn't assume equal variances); assumption
checking (independence within and between samples; approximate
normality OR large n).

Write the brief's "Two-Sample Comparison" section in 500-800 words.

1. **Identify a meaningful comparison.** Two groups in your data
   where the comparison answers a real question. Examples:
   - Two demographic groups (male/female; control/treatment).
   - Two time periods (pre/post intervention; weekday/weekend).
   - Two categories of a categorical variable, on some
     quantitative outcome.

2. **State the hypotheses.**
   - H₀: μ₁ = μ₂ (or p₁ = p₂) — no difference between groups.
   - H_a: μ₁ ≠ μ₂ (two-sided default, unless theory dictates
     direction).

3. **Choose the test.**
   - Independent two-sample t-test (Welch's preferred): two
     distinct groups.
   - Paired t-test: same subjects measured twice.
   - Two-proportion z-test: comparing proportions.

4. **Check assumptions.**
   - Independence: were the two samples drawn independently?
   - Normality OR large n: if both groups have n > 30, CLT covers
     us.

5. **Run the test.**
   - For independent t-test: `scipy.stats.ttest_ind(group1,
     group2, equal_var=False)` for Welch's.
   - For paired t-test: `scipy.stats.ttest_rel(before, after)`.
   - For two-proportion z-test: build it from formulas or use
     `statsmodels.stats.proportion.proportions_ztest`.

6. **Compute the effect size.** A significant test result with a
   tiny effect size is statistically detectable but practically
   meaningless.
   - For means: Cohen's d = (x̄₁ − x̄₂) / pooled SD.
   - For proportions: difference in proportions; risk ratio;
     odds ratio.

7. **Interpret.** Both the p-value AND the effect size matter.
   The p-value says "the difference exists"; the effect size says
   "by how much."

End with: based on this comparison, does your big question (from
Ch 1) become clearer? What does the comparison say about your
dataset's central story?
```

---

**What this produces:** A 500–800 word section with a complete two-sample test + effect size + interpretation. The effect-size discipline is what distinguishes useful inference from p-value-hunting.

**How to adapt this prompt:**

- *For your own project:* The choice of comparison shapes the story. Pick one that's defensible AND interesting.
- *For ChatGPT / Gemini:* Works as written.
- *For Claude Code:* The right tool. Effect size doesn't have a one-liner — compute it explicitly.
- *For a Claude Project:* Append.

**Connection to previous chapters:** Ch 8's CI logic generalizes — the two-sample test is equivalent to a CI for the difference of means.

**Preview of next chapter:** Chapter 11 covers chi-square tests for categorical relationships. You'll formally test independence between two categorical variables in your dataset (extending Ch 3's contingency-table work).

---

## AI Wayback Machine

**Frank Wilcoxon** was a chemist whose rank-based two-sample test (1945) became one of the most-used nonparametric techniques in modern statistics.

**Run this:**

```
Who is Frank Wilcoxon, and how does their work connect to
two-sample tests we covered in this chapter? Keep it to three
paragraphs. End with the single most surprising thing about their
career or ideas.
```

→ Search **"Frank Wilcoxon"** on Wikipedia.

**Now make the prompt better.** Try one of these:

- Ask it to apply Frank Wilcoxon's framework to a small dataset you actually have.
- Add a constraint: "Answer including criticisms or limits of Frank Wilcoxon's framework."

What changes? What gets better? What gets worse?
