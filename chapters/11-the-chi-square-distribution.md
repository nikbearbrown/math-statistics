# Chapter 11 — The Chi-Square Distribution

## TL;DR

- How far from expectation is too far to believe?
- The chapter moves through Where chi-square comes from, The test statistic, Goodness of fit: Does the theory match the data?, Independence: Are these two things actually related?, and related ideas.
- Read it for the main argument, the vocabulary it introduces, and the practical judgment it asks you to develop.

*How far from expectation is too far to believe?*

---

Gregor Mendel, in his monastery garden in Brno in the 1860s, was crossing pea plants and counting their offspring. His theory said that when you cross two heterozygous yellow plants, the offspring should appear in a 3:1 ratio — three yellow for every one green. In 1,000 offspring, theory predicts roughly 750 yellow and 250 green.

He planted. He counted. He got 882 yellow and 118 green.

That's not 750 and 250. So — does this mean the theory is wrong?

Or is 882 close *enough* to 750, given the noise of real seeds growing in real soil tended by real monks? The question isn't whether the numbers match exactly. They never match exactly. The question is: how far from expectation can the data be before the deviation is too large to attribute to chance?

That is the question chi-square was built to answer.

---

## Where chi-square comes from

Every distribution is the sum of something. The normal distribution emerges when you add many small independent random contributions. The t-distribution is the ratio of a sample mean to its estimated standard error. The chi-square distribution is the sum of *squared, standardized* deviations.

Here is the exact statement. Take $k$ independent standard normal random variables — each drawn from $N(0,1)$ — and square each one and add them up:

$$\chi^2 = Z_1^2 + Z_2^2 + \cdots + Z_k^2$$

The resulting sum follows a chi-square distribution with $k$ degrees of freedom.

You won't generate $k$ normal variables every time you run a chi-square test. The formula is machinery. The conceptual point is this: chi-square measures accumulated squared deviations. When observed data fall far from expected, those deviations grow, their squares grow faster, and the sum grows fastest. A big chi-square means the observed pattern is hard to explain by chance alone.

The shape of the chi-square curve is nothing like a bell. It is right-skewed — bunched near zero, with a long tail reaching toward infinity. When $df = 1$, the curve is most sharply skewed, most of its mass sitting close to zero. As $df$ increases, the curve spreads and flattens, and by around $df = 90$ it is nearly indistinguishable from a normal distribution. For any $df$, the mean of the distribution is $\mu = df$ and the standard deviation is $\sigma = \sqrt{2 \cdot df}$.

That mean gives you immediate intuition. If you are running a test with 6 degrees of freedom, the average chi-square value — under a true null hypothesis — is 6. A test statistic of 7 is barely above average. A test statistic of 20 is well into the tail and suggests the null hypothesis is in trouble.

Two properties follow from the structure. First, chi-square is always nonnegative — it's a sum of squares, and squares cannot be negative. Second, chi-square tests are always right-tailed. You reject the null when the test statistic is *large*, because large means observed deviations from expected are large. There is no left-tailed chi-square test.

![The distribution shifts right and becomes more symmetric as degrees of freedom increase](images/11-the-chi-square-distribution-fig-01.png)
*Figure 11.1 — Overlapping chi-square density curves for df=1, df=4, df=10,*

---

## The test statistic

All chi-square tests share one formula for the test statistic:

$$\chi^2 = \sum \frac{(O - E)^2}{E}$$

$O$ is the observed count in a category. $E$ is the expected count under the null hypothesis. You compute the squared deviation, divide by the expected count to standardize it, and sum across all categories.

Why divide by $E$? Because the same absolute deviation means different things at different scales. If you expect 1,000 and observe 1,010, that's a deviation of 10 out of 1,000 — about 1%, well within normal fluctuation. If you expect 5 and observe 15, that's also a deviation of 10 — but it's twice the expected value, a complete breakdown of the model. Dividing by $E$ makes these two situations register differently in the statistic, as they should.

Why square the deviation? Two reasons. First, squaring makes every term nonnegative, so overestimates and underestimates don't cancel. Second, squaring penalizes large deviations more harshly than small ones — a deviation of 20 contributes four times as much to the statistic as a deviation of 10. The test is sensitive to big misses.

One assumption underlies the whole procedure: the expected count in each cell must be at least 5. When expected counts are very small, the chi-square approximation breaks down. The test statistic no longer follows the chi-square distribution, and the p-value is unreliable. When you encounter small expected counts, the remedy is to combine categories until each expected count clears 5.

![Two-column diagram labeled "Why E < 5 breaks](images/11-the-chi-square-distribution-fig-02.png)
*Figure 11.2 — Two-column diagram labeled "Why E < 5 breaks*

---

## Goodness of fit: Does the theory match the data?

The first application is the one Mendel needed. You have a theory that predicts a particular distribution across categories. You collect data. Does the observed distribution fit the theoretical one, or are the deviations too large to be noise?

The null hypothesis is always the same structure: *the data fit the proposed distribution*. The alternative: *they don't*. You compute expected counts from the theory, compare them to the observed counts using the formula above, and ask whether the resulting statistic is unusually large.

The degrees of freedom for a goodness-of-fit test: $df = k - 1$, where $k$ is the number of categories. Why $k - 1$? Because if you know the counts in $k - 1$ categories and you know the total, the last count is determined. It's not free. You lose one degree of freedom for that constraint.

Let me work through a clean example. Company managers want to know whether employees call in sick equally often across the five days of the work week. They survey 60 managers and record which weekday saw the most absences at their workplace: Monday 15, Tuesday 12, Wednesday 9, Thursday 9, Friday 15.

Under the null hypothesis — absences distributed uniformly — each day should account for one-fifth of 60 absences: $E = 12$ for every day.

| Day | $O$ | $E$ | $(O-E)^2/E$ |
|---|---|---|---|
| Monday | 15 | 12 | 0.75 |
| Tuesday | 12 | 12 | 0.00 |
| Wednesday | 9 | 12 | 0.75 |
| Thursday | 9 | 12 | 0.75 |
| Friday | 15 | 12 | 0.75 |

$\chi^2 = 3.00$, with $df = 4$.

At $\alpha = 0.05$ with 4 degrees of freedom, the critical value from a chi-square table is 9.488. Our test statistic is 3.00, well below that. We do not reject the null. The pattern — Monday and Friday slightly higher — is consistent with what random variation would produce. There's no evidence that some days genuinely attract more absences than others.

Now notice what happened. The observed counts weren't equal. They never will be. The question was never "are the counts exactly equal?" — that's a question with an obvious answer. The question was: is the inequality we see bigger than what chance alone would generate? The answer here is no.

Now return to Mendel. He observed 882 yellow and 118 green from 1,000 offspring. Theory predicts 750 and 250.

$$\chi^2 = \frac{(882-750)^2}{750} + \frac{(118-250)^2}{250} = \frac{17424}{750} + \frac{17424}{250} = 23.23 + 69.70 = 92.93$$

With $df = 1$, the critical value at $\alpha = 0.05$ is 3.841. The statistic 92.93 is enormous. Mendel's data deviate dramatically from the 3:1 prediction. His theory, as stated, does not fit this particular set of counts.

This is historically interesting because Mendel's published data are notoriously too clean — subsequent analysis by Ronald Fisher suggested the data may have been adjusted to better match the theory. The chi-square test, in this light, isn't just about whether a theory fits the data; it can also ask whether data are suspiciously well-fitted, which is a different kind of anomaly.

![Chi-square distribution curve for df=1, horizontal axis 0](images/11-the-chi-square-distribution-fig-03.png)
*Figure 11.3 — Chi-square distribution curve for df=1, horizontal axis 0*

---

## Independence: Are these two things actually related?

The second application asks a different question. You have two categorical variables measured on the same sample. You want to know whether they're associated — whether knowing the value of one tells you something about the likely value of the other — or whether they're genuinely independent.

The setup is a contingency table. Rows represent one variable, columns the other, and each cell contains a count.

Under the null hypothesis of independence, the probability of landing in cell $(i,j)$ is the product of the probabilities of the two categories separately: $P(A_i \text{ and } B_j) = P(A_i) \times P(B_j)$. This is the definition of statistical independence. From this, you can compute what the cell counts *should* be if the variables are truly independent:

$$E_{ij} = \frac{(\text{row total}_i) \times (\text{column total}_j)}{\text{grand total}}$$

The test statistic is the same formula — $\sum (O-E)^2/E$ — summed over every cell. The degrees of freedom change:

$$df = (\text{rows} - 1) \times (\text{columns} - 1)$$

Why this formula? In a table with $r$ rows and $c$ columns, the row totals and column totals are fixed by the data. Once you fill in $(r-1) \times (c-1)$ cells, all remaining cells are determined by subtraction. Those are the cells that are genuinely free to vary.

![Only the shaded free cells contribute independent information — the rest are forced by the margins](images/11-the-chi-square-distribution-fig-04.png)
*Figure 11.4 — 3×3 contingency table skeleton *

A survey of 400 moviegoers classified by age group and favorite genre. Three age groups (18–30, 31–50, 50+), three genres (Action, Comedy, Drama). Does preference depend on age?

| | Action | Comedy | Drama | Total |
|---|---|---|---|---|
| 18–30 | 40 | 50 | 35 | 125 |
| 31–50 | 45 | 60 | 70 | 175 |
| 50+ | 30 | 40 | 30 | 100 |
| Total | 115 | 150 | 135 | 400 |

Expected count for cell (18–30, Action): $\frac{125 \times 115}{400} = 35.94$.

You compute expected counts for all nine cells, then calculate $\chi^2 = \sum (O-E)^2/E$ across all nine. Suppose you get $\chi^2 = 8.7$.

$df = (3-1)(3-1) = 4$. Critical value at $\alpha = 0.05$: 9.488.

$8.7 < 9.488$. We do not reject the null. The data are consistent with movie preference being independent of age group.

But suppose you got $\chi^2 = 14.3$. Now $14.3 > 9.488$ and we reject the null. We conclude that preference and age are associated. And this is where a critical point arrives: association is not causation. The test tells you the variables are not independent. It does not tell you *why*. 

Maybe age affects taste directly — people genuinely prefer different things at different life stages. Or maybe it's generational — people who grew up watching certain movies in their formative years have permanent preferences shaped by that history. Or maybe it's economic — older moviegoers can afford drama tickets, which are more expensive in some markets. Or maybe it's selection bias — the survey was conducted at a multiplex whose customers skew young, and older respondents were recruited differently.

The test finds association. It cannot distinguish among these explanations. That work requires thinking about mechanism, and thinking about mechanism is science, not statistics.

---

## The lottery: Putting it together

A lottery commission wants to verify that their drawing machine is fair. Each of six numbers should appear equally often — a uniform distribution over six outcomes. They run 6,000 draws and observe: 980, 1,010, 1,020, 995, 990, 1,005.

Expected count for each number: $6000/6 = 1000$.

$$\chi^2 = \frac{(980-1000)^2}{1000} + \frac{(1010-1000)^2}{1000} + \frac{(1020-1000)^2}{1000} + \frac{(995-1000)^2}{1000} + \frac{(990-1000)^2}{1000} + \frac{(1005-1000)^2}{1000}$$

$$= \frac{400 + 100 + 400 + 25 + 100 + 25}{1000} = \frac{1050}{1000} = 1.05$$

$df = 5$. Critical value at $\alpha = 0.05$: 11.07.

$1.05 \ll 11.07$. We do not reject the null. The machine is producing results consistent with fair draws.

Now here's a thing worth noticing. The mean of a chi-square distribution with 5 degrees of freedom is 5. Our test statistic of 1.05 is well *below* the mean. The fit is actually suspiciously good. The 6,000 draws produced results very close to the theoretical distribution — almost too close.

This is the kind of observation that caught Fisher's attention when he examined Mendel's data. A chi-square statistic that's extremely small — much less than its degrees of freedom — suggests the data may have been adjusted or selected for agreement with the theory. Real data is messy; it almost always produces some deviation. Data that's too clean is its own kind of anomaly.

![Both extremes are informative — very large statistics suggest bad fit; very small statistics suggest suspiciously good fit](images/11-the-chi-square-distribution-fig-05.png)
*Figure 11.5 — Chi-square distribution for df=5 *

---

## One distribution, three questions

The machinery is the same in every application: compute expected counts from the null hypothesis, compute $(O-E)^2/E$ for each cell, sum, compare to the chi-square distribution at the appropriate degrees of freedom.

The questions differ.

Goodness-of-fit asks: does a single categorical variable follow a specific proposed distribution? You specify the expected proportions, convert them to expected counts, and test whether the data fit.

Independence asks: are two categorical variables unrelated? You estimate expected counts from the marginal totals, assuming independence, and test whether the actual cell counts are consistent with that assumption.

A third application exists — testing whether a single population variance equals a specific claimed value — but the structure is the same: squared deviations from a model, summed and compared to the chi-square distribution.

The common thread is that all three tests start with *counts* and ask whether those counts are consistent with a theory. This is the fundamental difference between chi-square and everything you've done before. The z-test and t-test work with measurements — heights, incomes, test scores — and ask whether means differ. Chi-square works with counts in categories and asks whether the distribution of those counts matches a model.

When you have measurements, use t or z. When you have counts, use chi-square. The distinction is not about sample size or precision — it's about what the data *are*.

---

## What chi-square doesn't tell you

A significant chi-square test tells you the observed counts are inconsistent with the null hypothesis at your chosen significance level. It does not tell you which cells are causing the problem. It does not tell you the direction of any association. It does not tell you the size of the effect — a test statistic of 20 in a 2×3 table with 2 degrees of freedom is significant, but 20 in the same table could arise from a strong association or a weak one depending on sample size.

For the independence test, effect size is often quantified by Cramér's V:

$$V = \sqrt{\frac{\chi^2}{n \cdot \min(r-1, c-1)}}$$

where $n$ is the total sample size and $\min(r-1, c-1)$ is the smaller of rows-minus-1 and columns-minus-1. $V$ ranges from 0 (no association) to 1 (perfect association). A chi-square test with a tiny p-value but a small $V$ tells you the association is real but weak — which is what happens when sample sizes are enormous and you can detect very small effects. Always report both.

| V range | Strength of association |
| --- | --- |
| V < 0.10 | Negligible, 0.10–0.30 |
| second column adds a note that p-value tells you the association is real, V tells you whether it matters practically | Audience, stakes, timing, and platform conventions shape the choice. |
| clear reference for the reporting discipline introduced in this section | A concrete checkpoint for applying the chapter concept. |

And the caution about causation deserves repeating. A chi-square test of independence that rejects the null tells you two variables are associated. It does not tell you why. The relationship could be direct, it could be mediated by a third variable, it could be an artifact of how the sample was collected. The test is the beginning of the analysis, not the end of it.

---

## What would change my mind

The conventional rule that expected counts must be at least 5 is widely applied but imperfectly justified. Different statisticians cite different thresholds — some say 3, some say 5, some say 10 in certain configurations. The threshold depends on the shape of the underlying distribution and the specific test structure in ways that aren't fully captured by a single cutoff. If a careful simulation study showed that the test is reliable at expected counts of 3 or even 2 in specific configurations, I would revise my practice accordingly. For now, 5 is the conventional floor because it's conservative and well-supported by the textbook literature.

The deeper uncertainty: when exactly does a suspiciously small chi-square (suggesting data are too clean) become grounds for concern versus grounds for celebration? Fisher's analysis of Mendel is the famous case, but the threshold for "too good to be true" has no clean formula. This would benefit from explicit decision criteria.

---

## Connections forward

Chapter 12 introduces the F-distribution and ANOVA — a test for whether three or more group means differ from each other. Chi-square tested categorical counts; ANOVA tests continuous measurements across categorical groups. Both compare observed variation to what chance would produce. Both are right-tailed. Both ask a version of the same question: is the pattern we see too large to be noise?

Chapter 13 covers linear regression, which quantifies association between two continuous variables. Chi-square quantifies association in contingency tables. Both tools are doing the same conceptual work — measuring how much knowing one variable helps you predict another — but through different mathematical machinery suited to different data types.

And if you encounter Bayesian methods later, you'll see chi-square again as a tool for model criticism: propose a model, generate the expected data it predicts, compare to actual data, and measure the discrepancy. The formula and the logic are identical to what you've learned here. The philosophical framing is different; the machinery is the same.

---

## Exercises

### Warm-up

**11.1** *(Chi-square distribution properties.)* (a) A chi-square distribution has $df = 8$. What is its mean? What is its standard deviation? (b) A test produces a statistic of $\chi^2 = 9.5$ with $df = 8$. Is this above or below the mean of the distribution? Does this give you reason to reject the null? (c) Why can a chi-square statistic never be negative?

**11.2** *(Recognizing the right test.)* For each of the following, state whether you would use a chi-square goodness-of-fit test, a chi-square test of independence, or neither (and name what you would use instead). (a) A coin is flipped 200 times. Are the frequencies of heads and tails consistent with a fair coin? (b) A survey of 500 students records their major and whether they work part-time. Are major and employment status associated? (c) You want to know whether the mean exam score of two classes differs. (d) A quality inspector checks 1,000 items and records them as defective or not. Is the defect rate consistent with the manufacturer's claimed 2%?

**11.3** *(Expected counts.)* A survey of 120 people records favorite season: Spring 25, Summer 40, Autumn 35, Winter 20. Under the null hypothesis that all seasons are equally preferred, what is the expected count for each? Are all expected counts at least 5?

### Application

**11.4** *(Goodness-of-fit: full test.)* A die is rolled 120 times. The observed frequencies for faces 1–6 are: 25, 17, 22, 18, 21, 17. Test whether the die is fair at $\alpha = 0.05$. Show the full calculation table, state the degrees of freedom, identify the critical value, make a decision, and write a conclusion in plain language.

**11.5** *(Test of independence: expected counts.)* A survey of 300 employees records job level (entry, mid, senior) and whether they have completed a training program (yes/no). The contingency table:

| | Completed | Not completed | Total |
|---|---|---|---|
| Entry | 40 | 60 | 100 |
| Mid | 55 | 45 | 100 |
| Senior | 70 | 30 | 100 |
| Total | 165 | 135 | 300 |

(a) Calculate the expected count for the "Entry, Completed" cell. (b) Calculate expected counts for all six cells. (c) Check whether all expected counts meet the minimum-of-5 condition.

**11.6** *(Test of independence: full test.)* Using the data from Exercise 11.5, compute the chi-square test statistic, state the degrees of freedom, find the critical value at $\alpha = 0.05$, and draw a conclusion. Is job level independent of training completion? If you reject the null, does this mean completing training causes promotion? Explain.

### Synthesis

**11.7** *(Effect size matters.)* Two researchers study the same question — whether gender is associated with preference for remote vs. in-person work. Researcher A surveys 50 people and gets $\chi^2 = 4.2$, $df = 1$, $p = 0.04$. Researcher B surveys 5,000 people and gets $\chi^2 = 4.5$, $df = 1$, $p = 0.03$. (a) Both reject the null at $\alpha = 0.05$. Compute Cramér's V for each. (b) What does the difference in V tell you about the practical meaning of the two results? (c) Why does a large sample size allow detection of a very weak association?

**11.8** *(Suspicious data.)* A researcher claims her experiment produced counts of exactly 150, 150, 150, 150 across four categories, each with expected count 150. Compute the chi-square statistic. What does this value tell you, and why should you be suspicious rather than reassured?

**11.9** *(Connecting to earlier chapters.)* In Chapter 8, you built a 95% confidence interval for a population proportion. Now suppose you want to test whether the proportion of defective items in a batch equals the manufacturer's claimed rate of 5%. You could do this as a z-test for a proportion (Chapter 9) or as a goodness-of-fit test with two categories (defective, not defective). (a) For a sample of 400 items with 28 defectives, set up both tests. (b) Both should give the same conclusion at $\alpha = 0.05$. Why? (c) What is the relationship between a z-test statistic $z$ and a chi-square statistic $\chi^2$ in the two-category case?

### Challenge

**11.10** *(Science and statistics.)* In Mendel's data, the chi-square of 92.93 leads to rejection of the 3:1 hypothesis. Fisher later argued that Mendel's data were suspiciously well-fitted to his theories in other experiments — the statistics were too good to be naturally generated. (a) Explain the difference between a test statistic that is too large (suggests the theory is wrong) and one that is too small (suggests the data may have been manipulated). (b) What additional information would you want to evaluate whether Mendel's data are fraudulent versus whether his theory was simply wrong? (c) Can chi-square alone tell you whether to distrust a researcher? What does and doesn't it establish?

---

## LLM Exercise — Chapter 11: The Chi-Square Distribution (Analyze One Dataset Project)

**Project:** Analyze One Real Dataset.  
**What you're building this chapter:** chi-square tests for categorical relationships in your data.  
**Tool:** **Claude Code** + **Claude Project**.

---

**The Prompt:**

```
Chapter 11 of my Analyze-One-Dataset project. Chapter 11 covered:
the chi-square distribution; three named chi-square tests:
1. Goodness-of-fit: does a single categorical variable's
   distribution match a theoretical or claimed distribution?
2. Independence: are two categorical variables independent (in
   a single sample)?
3. Homogeneity: are the distributions of one categorical variable
   the same across multiple groups?
The test statistic χ² = Σ (Observed − Expected)² / Expected; the
degrees of freedom depend on the test type; rejection when
χ² > critical value (or equivalently p < α).

Write the brief's "Chi-Square Analysis" section in 500-700 words.

1. **Pick TWO categorical variables** (from Ch 3's work; ideally
   the same pair you analyzed there). Build a contingency table.

2. **Compute expected counts** under H₀ (independence):
   Expected[i,j] = (Row_i_total × Column_j_total) / Grand_total.

3. **Check expected-count rule.** Chi-square assumes expected
   counts ≥ 5 for the approximation to work. If some cells have
   expected < 5, consider Fisher's exact test instead.

4. **Run the chi-square test of independence.**
   - State H₀: variables are independent.
   - Compute χ² = Σ (O − E)² / E across all cells.
   - Degrees of freedom = (rows − 1)(columns − 1).
   - Use scipy.stats.chi2_contingency(table) to get χ², p-value,
     df.

5. **Interpret.**
   - If p < α: reject independence; the two variables are
     associated.
   - If p ≥ α: insufficient evidence to reject independence;
     they may be related or may not — we can't conclude
     independence.

6. **Effect size: Cramér's V.** A small p-value tells you the
   variables are related; Cramér's V tells you how strongly.
   - V = √(χ² / (n × (min(r,c) − 1))).
   - V = 0 means no association; V = 1 means perfect association.
   - Rules of thumb: V < 0.1 is weak; 0.1-0.3 is moderate;
     > 0.3 is strong.

7. **Goodness-of-fit (optional).** If you have a "claimed
   distribution" for a single variable (e.g., "the proportion of
   X should be 25%"), test it against the observed.

End with: how do Ch 3's conditional probabilities (which suggested
dependence visually) align with Ch 11's formal test (which
quantifies it)? Did the chi-square reject what your eyes
suspected?
```

---

**What this produces:** A 500-700 word section with chi-square test + Cramér's V + interpretation.

**How to adapt this prompt:**

- *For your own project:* If your dataset has only two categorical variables, chi-square is fully exercised on them. If multiple, pick the pair most relevant to your big question.
- *For ChatGPT / Gemini:* Works as written.
- *For Claude Code:* `scipy.stats.chi2_contingency` is the one-liner.
- *For a Claude Project:* Append.

**Connection to previous chapters:** Ch 3 (conditional probabilities) → Ch 11 (formal test of independence) close the loop.

**Preview of next chapter:** Chapter 12 covers ANOVA — comparing 3+ group means. You'll pick a categorical variable with multiple levels and run ANOVA on a quantitative outcome.

---

##  AI Wayback Machine
**Karl Pearson** introduced the chi-square test in 1900 — and built the foundations of biometric statistics, with a controversial eugenics legacy.

**Run this:**

```
Who is Karl Pearson, and how does their work connect to the chi-square distribution we covered in this chapter? Keep it to three paragraphs. End with the single most surprising thing about their career or ideas.
```

→ Search **"Karl Pearson"** on Wikipedia.

**Now make the prompt better.** Try one of these:

- Ask it to apply Karl Pearson's framework to a small dataset you actually have.
- Add a constraint: "Answer including criticisms or limits of Karl Pearson's framework."

What changes? What gets better? What gets worse?
