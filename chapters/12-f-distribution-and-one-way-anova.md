# Chapter 12 — The F-Distribution and One-Way ANOVA
*What wheat fields taught us about comparing everything.*

---

In the 1920s, Ronald Fisher was standing in a wheat field in Hertfordshire, England, trying to solve a problem that sounds simple but turns out to be subtle.

He wanted to know whether fertilizer choice mattered. Five plots, five fertilizers, one harvest. Measure the yield from each plot. Compare. Simple enough.

But here is the trap. The obvious approach is to compare pairs: does Plot 1 yield more than Plot 2? Does Plot 2 yield more than Plot 3? With five groups, there are ten such pairwise comparisons. If each comparison runs at a 5% significance level, then by the time you've run all ten, you expect half a false positive just from chance. Push to 20 comparisons and you're nearly certain to find something "significant" that isn't real.

Fisher didn't want to run ten tests. He wanted to run one.

His insight was to stop asking about pairs and start asking about the whole picture at once. Instead of testing whether any two fertilizers differ, ask: is there more variation between the fertilizer groups than there is within them? If fertilizer choice doesn't matter, the yields should scatter randomly — and you'd expect the variation between groups to look about the same as the variation within groups. But if fertilizer choice does matter, the between-group variation will be inflated. It will be larger than the within-group noise. The ratio of those two variance estimates is the test. That ratio follows a distribution that now bears Fisher's name.

This is ANOVA — Analysis of Variance. One test. One family-wise error rate. One decision about whether any difference exists at all.

<!-- → [CHART: Line chart — x-axis: number of groups (2 to 10), y-axis: family-wise false positive rate (probability of at least one spurious significant result). Curve rises from 0.05 at 2 groups to ~0.40 at 5 groups (10 comparisons) and ~0.63 at 7 groups (21 comparisons), computed as 1 − 0.95^(k(k−1)/2). Horizontal dashed line at 0.05 labeled "Intended α." Caption: "Each pairwise t-test adds to the chance of a false discovery. With 5 groups and 10 comparisons, the true error rate is 8× the intended level."] -->

---

## The F-distribution

Before we can use the test, we need to understand the distribution it produces.

The F-distribution is the distribution of the ratio of two independent variance estimates, both drawn from normally distributed populations. Take two samples, compute their variances, divide one by the other. The resulting number follows an F-distribution.

A few things about its shape. Since variances are always non-negative, the F-ratio is always non-negative — it can't be less than zero. The distribution is right-skewed: most F-values cluster near 1, but the right tail extends a long way. If both variance estimates come from identical populations, you'd expect their ratio to hover near 1. Occasionally sampling noise pushes it somewhat higher or somewhat lower, but rarely very far.

The exact shape of the F-distribution depends on two parameters, both degrees of freedom: one for the numerator and one for the denominator, written $F_{df_1, df_2}$. As the degrees of freedom grow, the F-distribution becomes more symmetric and approaches the normal distribution — but it never fully loses the right skew that comes from being a ratio of squared quantities.

Why does the F-ratio follow this specific distribution? The argument runs through the chi-squared distribution: each variance estimate, when properly scaled, follows a chi-squared distribution, and the ratio of two independent chi-squared variables (each divided by its degrees of freedom) is the F-distribution by definition. The derivation is mathematical statistics; for our purposes, what matters is what the distribution implies: when the null hypothesis is true (the two variance estimates come from the same population), the F-ratio should be close to 1. When the null is false (the numerator variance is genuinely larger than the denominator variance), the F-ratio climbs. Large F-values are evidence against the null.

<!-- → [IMAGE: Three F-distribution curves on the same axes. F(1,10): extremely right-skewed, tall spike near zero. F(5,10): wider, peak shifts right, less extreme skew. F(20,20): nearly symmetric bell, much closer to normal. Hard left boundary at zero visible on all. Caption: "Same family, different shapes. As degrees of freedom grow, the F-distribution becomes more symmetric — but it starts from zero and skews right."] -->

---

## A simple use: comparing two variances

Before arriving at ANOVA, it's worth seeing the F-distribution in its simplest application, because it clarifies what the ratio means.

Two professors grade the same ten student exams. One professor's grades have variance $s_1^2 = 52.3$. The other's have variance $s_2^2 = 89.9$. The question: are these two professors equally consistent, or does one grade with more spread?

The hypotheses:

$H_0: \sigma_1^2 = \sigma_2^2$ (the variances are equal)

$H_a: \sigma_1^2 \neq \sigma_2^2$ (the variances differ)

The test statistic is the ratio of the two sample variances. By convention, put the larger in the numerator so the ratio is at least 1:

$$F = \frac{s_{\text{larger}}^2}{s_{\text{smaller}}^2} = \frac{89.9}{52.3} = 1.719$$

The degrees of freedom are $n_1 - 1 = 9$ for the numerator and $n_2 - 1 = 9$ for the denominator. This is an $F_{9,9}$ distribution.

At the 1% significance level, the critical value is $F_{0.01, 9, 9} = 5.35$. Our test statistic is 1.719. Since $1.719 < 5.35$, we fail to reject $H_0$. The data don't show a meaningful difference in grading consistency between the two professors.

One caveat worth stating plainly: the F-test for comparing two variances is unusually sensitive to departures from normality. Other tests in this book tolerate skewness and outliers reasonably well. Not this one. If you run this test on data that aren't genuinely close to normal, the results can mislead in both directions. Before using it, plot your data. If the distribution looks skewed or has heavy outliers, the test is unreliable.

---

## The central idea of ANOVA: partitioning variation

Now we can turn to the more important use of the F-distribution — the one Fisher invented it for.

ANOVA's strategy is to decompose the total variation in your data into two parts: the variation that can be explained by group membership, and the variation that can't.

Imagine measuring something — call it the outcome — in $k$ different groups, with $n$ total observations. Every observation deviates from the grand mean. Some of that deviation happens because the groups have different means (the fertilizer really does affect yield). Some happens because individuals within any one group differ from each other (random noise). ANOVA separates these two contributions.

The math uses sums of squared deviations, which we write as $SS$ — sum of squares.

**Total variation** is the sum of squared deviations of every observation from the grand mean $\bar{x}_{\text{grand}}$:

$$SS_{\text{total}} = \sum_{i=1}^{n} (x_i - \bar{x}_{\text{grand}})^2$$

**Between-groups variation** is the part explained by group membership. For each group $j$ with mean $\bar{x}_j$ and $n_j$ observations, compute how far that group's mean is from the grand mean, square it, and multiply by the group's size:

$$SS_{\text{between}} = \sum_{j=1}^{k} n_j (\bar{x}_j - \bar{x}_{\text{grand}})^2$$

If all groups had the same mean, this would be zero. The larger the differences between group means, the larger $SS_{\text{between}}$.

**Within-groups variation** is everything left — the scatter of individual observations around their own group mean:

$$SS_{\text{within}} = SS_{\text{total}} - SS_{\text{between}}$$

Or computed directly:

$$SS_{\text{within}} = \sum_{j=1}^{k} \sum_{i \in \text{group } j} (x_i - \bar{x}_j)^2$$

This is the noise — the variation ANOVA cannot explain, the individual differences that would exist even if every group had exactly the same mean.

These three quantities are related by a clean identity:

$$SS_{\text{total}} = SS_{\text{between}} + SS_{\text{within}}$$

Total variation equals explained variation plus unexplained variation. Every partition in statistics has this structure. It's the same logic that appears in regression (Chapter 13), where total variation in the outcome splits into variation explained by the regression line and variation that remains as residuals. The idea is always the same: account for what you can, measure what's left.

<!-- → [INFOGRAPHIC: Bar diagram showing one wide bar labeled "SS_total" splitting into two stacked segments: upper segment labeled "SS_between — variation explained by group membership" (lighter color) and lower segment labeled "SS_within — residual noise within groups" (darker color). Arrow annotation: "ANOVA asks whether SS_between is large relative to SS_within." Caption: "Partition of variation. When groups are genuinely different, the explained piece grows. When groups are identical, virtually everything is residual noise."] -->

---

## Mean squares: turning sums into variance estimates

Raw sums of squares grow with sample size — a dataset of 1,000 observations will have larger sums than a dataset of 10, just because there are more terms. To get comparable variance estimates, divide each sum of squares by its degrees of freedom.

The degrees of freedom make sense if you think about them carefully. For the between-groups variation, you have $k$ group means. But they're constrained: they must average to the grand mean. So there are $k - 1$ independent pieces of information. For the within-groups variation, you have $n$ total observations. But $k$ group means constrain them — each group's observations average to their group mean. That leaves $n - k$ free pieces.

Divide each sum of squares by its degrees of freedom to get **mean squares** — which are proper variance estimates:

$$MS_{\text{between}} = \frac{SS_{\text{between}}}{k - 1}$$

$$MS_{\text{within}} = \frac{SS_{\text{within}}}{n - k}$$

---

## The F-statistic: the ratio that does the work

Here is the key move. Under the null hypothesis — all group means are equal — both $MS_{\text{between}}$ and $MS_{\text{within}}$ are estimating the same underlying population variance. They're both measuring random scatter. Their ratio should be near 1.

But if the null hypothesis is false — if some groups genuinely have different means — then $MS_{\text{between}}$ gets inflated. It now contains the population variance plus an additional component from the real differences between group means. $MS_{\text{within}}$, measuring scatter within groups, doesn't pick up that between-group signal. It stays near the population variance. The ratio:

$$F = \frac{MS_{\text{between}}}{MS_{\text{within}}}$$

climbs above 1 when real differences exist. How far above 1 depends on how large the differences are relative to the within-group noise.

This F-statistic follows an $F_{k-1,\, n-k}$ distribution under the null hypothesis. Large values — in the right tail — are evidence against the null. The test is always right-tailed, because under the null the ratio is near 1, and evidence against the null always pushes the ratio up, never down.

<!-- → [IMAGE: F-distribution curve with right tail shaded as rejection region beyond the critical value F_crit. Two vertical lines: one near 1 labeled "F ≈ 1 (H₀ true — groups are the same)" and one far to the right labeled "F >> 1 (H_a: group means genuinely differ, MS_between inflated)." Caption: "ANOVA is always right-tailed. Under the null, F ≈ 1. Real group differences push F into the right tail."] -->

---

## A worked example: three diet plans

Three diet plans are tested for weight loss. Four people follow Plan 1, three follow Plan 2, three follow Plan 3. Weight losses in pounds:

Plan 1: 5, 4.5, 4, 3 — mean = 4.125

Plan 2: 3.5, 7, 4.5 — mean = 5.0

Plan 3: 8, 4, 3.5 — mean = 5.167

Grand mean = $(16.5 + 15 + 15.5)/10 = 4.7$

$SS_{\text{between}}$: how far do group means sit from the grand mean?

$$SS_{\text{between}} = 4(4.125 - 4.7)^2 + 3(5.0 - 4.7)^2 + 3(5.167 - 4.7)^2 \approx 2.246$$

$SS_{\text{total}}$: how far do all observations sit from the grand mean?

$$SS_{\text{total}} = (5 - 4.7)^2 + (4.5 - 4.7)^2 + \cdots \approx 23.1$$

$SS_{\text{within}} = 23.1 - 2.246 = 20.854$

Degrees of freedom: $df_{\text{between}} = 3 - 1 = 2$, $df_{\text{within}} = 10 - 3 = 7$

Mean squares:

$$MS_{\text{between}} = 2.246 / 2 = 1.123$$

$$MS_{\text{within}} = 20.854 / 7 = 2.979$$

Test statistic:

$$F = 1.123 / 2.979 = 0.377$$

The ANOVA table:

| Source | SS | df | MS | F |
|---|---|---|---|---|
| Between | 2.246 | 2 | 1.123 | 0.377 |
| Within | 20.854 | 7 | 2.979 | |
| Total | 23.1 | 9 | | |

The F-statistic is 0.377 — below 1. This means the between-group variation is actually smaller than the within-group variation. There is no signal in the direction we'd need for evidence of a diet difference. At any reasonable significance level, we fail to reject $H_0$. The data do not show that the three diets produce different weight losses.

Notice something important about this result: the F below 1 is itself informative. It's telling you that the group means are less spread out than you'd expect from random sampling even if there were no real differences. With only 10 observations split across three groups, there simply isn't enough data to detect a difference even if one existed.

<!-- → [IMAGE: Strip plot (dot plot) of the diet plan data — x-axis: three diet plans, y-axis: weight loss in pounds. Individual data points shown as dots per group. Group means marked with a horizontal line. Grand mean marked with a dashed horizontal line. Student should see: the within-group scatter for each plan is large relative to how far apart the group means sit — F < 1 becomes visually obvious. Caption: "The group means (Plan 1: 4.1, Plan 2: 5.0, Plan 3: 5.2) are nearly indistinguishable from the grand mean once you see the within-group scatter."] -->

---

## Reading the ANOVA table

Software produces the ANOVA table automatically. Understanding each column tells you how to interpret the result.

The **SS** column shows raw variation in each component. On its own, it doesn't tell you much — it grows with sample size.

The **df** column shows degrees of freedom. Between groups: $k - 1$. Within groups: $n - k$. Total: $n - 1$. These must add up correctly.

The **MS** column is the variance estimate — $SS/df$. This is what actually compares apples to apples.

The **F** column is $MS_{\text{between}} / MS_{\text{within}}$. This is the test statistic.

The **p-value** is the probability of observing an F-statistic at least this large, assuming the null hypothesis is true. This is the probability you compare to your significance level $\alpha$ to make the decision.

A full example. You're comparing four farming methods. The ANOVA table shows $F = 4.2$ with $df_{\text{between}} = 3$ and $df_{\text{within}} = 16$. Software reports $p = 0.0207$. At $\alpha = 0.05$: since $0.0207 < 0.05$, reject $H_0$. There is sufficient evidence that at least one farming method produces a different mean yield than the others.

---

## What ANOVA does not tell you

Here is the crucial limitation, and it's easy to miss in the excitement of a significant result.

ANOVA's null hypothesis is $H_0: \mu_1 = \mu_2 = \mu_3 = \cdots = \mu_k$ — all group means are equal. The alternative is: at least one pair of means differs. When you reject the null, you know that at least one difference exists somewhere. You do not know which groups are responsible.

Farming Method 1 might be the same as Methods 2, 3, and 4. Method 4 might differ from all the others. Methods 2 and 3 might be identical to each other but different from 1 and 4. The ANOVA table cannot tell you any of this. It is an **omnibus test** — a joint test over all groups at once.

To find out which specific pairs differ, you need a **post-hoc test**: a procedure that runs pairwise comparisons while controlling the family-wise error rate. The most common is Tukey's HSD (honestly significant difference). Others include the Bonferroni correction and the Scheffé method. Each makes slightly different assumptions and has slightly different power. What they share is the goal: identifying which pairs are genuinely different while keeping the probability of any false positive at or below $\alpha$ across all comparisons.

The sequence is: first run ANOVA. If the F-test is significant, then run a post-hoc test to identify which pairs. Running post-hoc tests without a significant ANOVA first — fishing for pairs before confirming that any differences exist — is the multiple-comparisons problem that Fisher's test was designed to prevent.

<!-- → [INFOGRAPHIC: Two-step flowchart. Step 1 box: "Run one-way ANOVA — F-test. Question: do any group means differ?" Arrow labeled "F significant (p < α)" leads to Step 2 box: "Run post-hoc test (Tukey's HSD). Question: which specific pairs differ?" Arrow labeled "F not significant (p ≥ α)" leads to "Stop — no evidence of any difference." Caption: "ANOVA gates the post-hoc analysis. Skipping the gate and running pairwise comparisons directly is the multiple-comparisons problem."] -->

---

## Assumptions and when to worry

ANOVA rests on three assumptions. How seriously you need to take them depends on your data.

**Normality** within each group. In practice, ANOVA is fairly robust to departures from normality, especially when sample sizes are large and roughly equal across groups. With small samples, the normality assumption matters more. Plot your data before analyzing. If histograms look roughly bell-shaped or if Q-Q plots stay near the diagonal, you're in reasonable shape.

**Equal variances** across groups — called homogeneity of variance. If one group has a variance ten times larger than another, the within-group mean square is a poor estimate of any single population variance. The F-test can give misleading results. Check variances visually (boxplots) or use Levene's test. If variances are unequal and sample sizes differ, Welch's ANOVA is safer — it relaxes the equal-variance assumption at the cost of some power.

**Independence** of observations. This is the most important assumption and the one that can't be salvaged by any adjustment. If observations within groups are correlated — siblings measured together, repeated measurements of the same person, students in the same classroom — the standard ANOVA is wrong. The fix depends on the structure of the dependence.

---

## Why this works where t-tests don't

It's worth returning to why Fisher invented this procedure, because the reason illuminates what the test is actually doing.

If you have $k$ groups and run all $\binom{k}{2}$ pairwise t-tests, each at $\alpha = 0.05$, the probability that at least one gives a false positive grows rapidly. With 4 groups there are 6 comparisons, and the family-wise false positive rate climbs to around 26%. With 6 groups and 15 comparisons, it approaches 54%.

ANOVA fixes this by running a single test. The F-statistic aggregates information from all groups simultaneously and evaluates it against a single critical value or p-value. The probability of a false rejection is exactly $\alpha$, regardless of how many groups you have.

This is why the F-statistic's numerator is $MS_{\text{between}}$ and not something simpler. Fisher constructed the numerator to be inflated specifically when any group mean departs from the common mean — without caring which group, or in which direction. The test is sensitive to the general question "are these groups all the same?" without being committed to any particular pair.

The cost is that ANOVA won't tell you which groups differ. But that's the point of running ANOVA first and post-hoc tests second: confirm that something is different, then find what.

---

## The same logic at every scale

Fisher's wheat fields had five plots and a few dozen observations. Modern clinical trials have dozens of treatment arms and thousands of patients. The ANOVA table grows; degrees of freedom multiply; the computation moves to software. But the logic doesn't change.

A pharmaceutical company testing three doses of a drug across four hospitals produces 12 cells in the ANOVA table. The F-statistic still asks: is the between-group variation large relative to the within-group noise? The answer is still compared to a critical value from the F-distribution with the appropriate degrees of freedom.

Meta-analyses compare findings across dozens of studies. Each study is a group; the outcome of interest might be the effect size. ANOVA asks whether the effect varies systematically across studies — whether something about the studies themselves (sample size, population, design) explains the variation in outcomes. The question is the same; the scale is different.

And in Chapter 13, you'll see the partitioning of variation again, now applied to a continuous predictor rather than discrete groups. The total variation in the outcome splits into variation explained by the regression line and unexplained residual variation. An F-test evaluates whether the explained portion is large enough to reject the null that the predictor has no relationship with the outcome. The ANOVA table and the regression table are the same table, seen from different angles.

Fisher, standing in his wheat field in 1920, was developing the skeleton of a method that would run, in one form or another, through virtually every statistical analysis for the next hundred years.

---

## Exercises

### Warm-up

**12.1** *(F-distribution basics.)* A researcher compares five keyboard layouts using 8 typists per layout. (a) How many pairwise t-tests would be needed to compare all pairs? (b) At $\alpha = 0.05$, what is the approximate family-wise false positive rate for all those pairwise tests? (Use $1 - 0.95^m$ where $m$ is the number of comparisons.) (c) What are $df_{\text{between}}$ and $df_{\text{within}}$ for an ANOVA on this data?

**12.2** *(Reading an ANOVA table.)* An ANOVA table has $df_{\text{total}} = 23$ and $df_{\text{between}} = 3$. (a) How many groups were tested? (b) What is $df_{\text{within}}$? (c) If $SS_{\text{between}} = 144$ and $SS_{\text{within}} = 360$, compute $MS_{\text{between}}$, $MS_{\text{within}}$, and the F-statistic.

**12.3** *(F-test for two variances.)* Two quality inspectors each measure the same 12 products. Inspector A's measurements have variance $s_A^2 = 4.2$; Inspector B's have variance $s_B^2 = 9.8$. (a) State the hypotheses for testing whether the inspectors have equal measurement variance. (b) Compute the F-statistic. (c) At $\alpha = 0.05$ with $F_{0.05, 11, 11} = 2.82$, what is your conclusion?

### Application

**12.4** *(Computing SS from raw data.)* Three study groups record exam scores: Group A: 78, 82, 80 (mean = 80); Group B: 65, 70, 68 (mean = 67.67); Group C: 90, 88, 92 (mean = 90). Grand mean = 79.22. (a) Compute $SS_{\text{between}}$. (b) Compute $SS_{\text{within}}$ for each group and sum them. (c) Verify that $SS_{\text{between}} + SS_{\text{within}} = SS_{\text{total}}$ by computing $SS_{\text{total}}$ directly.

**12.5** *(Full ANOVA and decision.)* A nutritionist tests four protein supplement brands on 6 participants each. The ANOVA table shows $SS_{\text{between}} = 315$, $SS_{\text{within}} = 840$, $k = 4$, $n = 24$. (a) Complete the ANOVA table (compute all df, MS, and F values). (b) The critical value $F_{0.05, 3, 20} = 3.10$. Do you reject $H_0$? (c) Write a conclusion in plain language about the protein supplements.

**12.6** *(Interpreting a significant result.)* An ANOVA comparing five teaching methods yields $F = 6.8$, $p = 0.001$ at $\alpha = 0.05$. A student concludes: "Method 1 is the best because it has the highest group mean." What is wrong with this reasoning? What additional analysis is needed?

### Synthesis

**12.7** *(Effect size and practical significance.)* A study compares mean salaries across three industries. ANOVA gives $F = 4.9$, $p = 0.012$, $SS_{\text{between}} = 1{,}200$, $SS_{\text{total}} = 15{,}000$. (a) Compute $\eta^2$ (eta-squared) = $SS_{\text{between}} / SS_{\text{total}}$. (b) Using the benchmarks small $\approx 0.01$, moderate $\approx 0.06$, large $\approx 0.14$: how large is this effect? (c) The result is statistically significant. Does that mean the salary differences are practically meaningful? What else would you want to know?

**12.8** *(Checking assumptions before running ANOVA.)* You're preparing to run ANOVA on test scores from four classrooms. Before computing the ANOVA table, describe the three checks you should perform. For each check, name the assumption being tested, how you'd check it, and what you'd do if the assumption appears violated.

### Challenge

**12.9** *(Post-hoc logic.)* An ANOVA compares mean blood pressure across five medication dosages. The result is significant at $\alpha = 0.05$. A colleague suggests running all 10 pairwise t-tests at $\alpha = 0.05$ to find which doses differ. (a) What is the family-wise error rate for 10 tests at $\alpha = 0.05$? (b) Explain why Tukey's HSD is preferable. (c) If Tukey's HSD controls the family-wise error rate at 0.05 across all 10 comparisons, what effective significance level is each individual comparison being held to? (Hint: think about what family-wise control means.)

**12.10** *(Connecting to regression.)* Chapter 13 will show that linear regression also partitions variation into explained (by the regression line) and unexplained (residuals), and uses an F-test to evaluate whether the line fits better than the grand mean alone. For a simple linear regression with one predictor and $n = 25$ observations: (a) What are the degrees of freedom for the regression (between) and residual (within) components? (b) Why does the logic of the F-test in regression — "is the explained variation large relative to the unexplained?" — mirror the logic of ANOVA exactly? (c) What is the key difference between what the categorical variable does in ANOVA and what a continuous predictor does in regression?

---

## LLM Exercise — Chapter 12: F-Distribution and One-Way ANOVA (Analyze One Dataset Project)

**Project:** Analyze One Real Dataset.
**What you're building this chapter:** one-way ANOVA comparing 3+ groups on a quantitative outcome.
**Tool:** **Claude Code** + **Claude Project**.

---

**The Prompt:**

```
Chapter 12 of my Analyze-One-Dataset project. Chapter 12 covered:
the F-distribution (ratio of variances; right-skewed; depends on
two df parameters); one-way ANOVA — compare means across 3+
groups; the ANOVA table:
- SSB (Sum of Squares Between groups) = Σ n_i (x̄_i − x̄)²
- SSW (Sum of Squares Within groups) = Σ Σ (x_ij − x̄_i)²
- MSB = SSB / (k − 1), where k = number of groups
- MSW = SSW / (n − k), where n = total sample size
- F = MSB / MSW
- df₁ = k − 1; df₂ = n − k
Big F means between-group variance dominates within-group variance
(groups are genuinely different). Multiple-comparisons problem:
running many t-tests inflates Type I error; ANOVA is the joint
test that handles this.

Write the brief's "ANOVA Analysis" section in 500-700 words.

1. **Pick a categorical variable with 3+ levels and a quantitative
   outcome.** Examples: comparing exam scores across 4 schools;
   comparing salary across 5 industries; comparing reaction time
   across 3 conditions.

2. **State the hypotheses.**
   - H₀: μ₁ = μ₂ = μ₃ = ... = μ_k (all group means equal)
   - H_a: at least one μ_i differs from another.

3. **Check assumptions.**
   - Independence of observations.
   - Approximate normality within each group (or large group n).
   - Roughly equal variances across groups (Levene's test, or
     visual check via boxplot).
   - If unequal variances and unequal n: consider Welch's ANOVA.

4. **Build the ANOVA table.**
   - Compute group means and overall mean.
   - Compute SSB, SSW, MSB, MSW, F.
   - Show the table.

5. **Test the result.** Compare F to F_α; or report p-value from
   scipy.stats.f_oneway(group1, group2, group3, ...).

6. **Post-hoc tests if significant.** If ANOVA rejects, you know
   AT LEAST ONE pair differs but not which. Run Tukey's HSD
   (`statsmodels.stats.multicomp.pairwise_tukeyhsd`) to identify
   the specific pairs. Tukey's HSD controls family-wise error
   rate (handles multiple-comparisons honestly).

7. **Effect size.** η² (eta-squared) = SSB / SS_Total tells you
   what fraction of variance is between-groups. Small effect:
   ~0.01; moderate: ~0.06; large: ~0.14.

End with: which groups in your data are significantly different
from each other, and which aren't? The ANOVA + Tukey combination
gives the full picture.
```

---

**What this produces:** A 500-700 word section with ANOVA table, F-test result, post-hoc analysis. The "which pairs differ" output from Tukey is the actionable finding.

**How to adapt this prompt:**

- *For your own project:* If your dataset only has 2-level categorical variables, this section becomes a 2-sample t-test (already done in Ch 10). Find a 3+ level variable if possible.
- *For ChatGPT / Gemini:* Works as written.
- *For Claude Code:* `scipy.stats.f_oneway` for ANOVA; `statsmodels.stats.multicomp.pairwise_tukeyhsd` for Tukey.
- *For a Claude Project:* Append.

**Connection to previous chapters:** Ch 10's two-sample t-test generalizes to Ch 12's ANOVA for 3+ groups.

**Preview of next chapter:** Chapter 13 is the closer — linear regression and correlation. You'll fit a regression line to two quantitative variables in your dataset, plus compile the full analysis report.

---

## AI Wayback Machine

**Ronald Fisher** invented analysis of variance, the F-distribution, and the core machinery of modern experimental statistics — with an entangled eugenics legacy.

**Run this:**

```
Who is Ronald Fisher, and how does their work connect to ANOVA and the F-distribution we covered in this chapter? Keep it to three paragraphs. End with the single most surprising thing about their career or ideas.
```

→ Search **"Ronald Fisher"** on Wikipedia.

**Now make the prompt better.** Try one of these:

- Ask it to apply Ronald Fisher's framework to a small dataset you actually have.
- Add a constraint: "Answer including criticisms or limits of Ronald Fisher's framework."

What changes? What gets better? What gets worse?
