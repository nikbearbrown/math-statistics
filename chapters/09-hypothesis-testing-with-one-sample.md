# Chapter 9 — Hypothesis Testing with One Sample
*The logic of being hard to convince.*

---

In May 2004, an FDA advisory committee sat down to vote on a drug.

The company had run a clinical trial. Patients taking the drug improved more than patients taking a placebo. The difference was real — the numbers were right there in the data. The company wanted approval. The committee had to decide whether the evidence was strong enough to change what doctors would prescribe to millions of people.

The question the committee was actually asking was not "did patients improve?" They did. The question was: could this improvement have happened by random chance, even if the drug does nothing? And if the answer is "yes, it could have happened by chance," how likely is it? Once in twenty trials? Once in a hundred?

This is what hypothesis testing is. It is not a formula. It is a standard for evidence. The committee's job was to demand that the drug company meet that standard before overturning the default: no approval unless proven otherwise. The asymmetry — the null position has the advantage, the challenger must clear a high bar — is not bias. It is rationality about stakes.

This chapter teaches you that logic and the machinery that implements it.

---

## The presumption of innocence

Hypothesis testing was modeled, consciously, on a legal proceeding.

A defendant stands trial. The court begins with a presumption of innocence. The prosecution must present evidence strong enough to overcome that presumption — "beyond reasonable doubt." If the evidence fails to reach that threshold, the verdict is "not guilty," which does not mean the court has declared the defendant innocent. It means: the evidence wasn't strong enough to convict.

The logic is asymmetric by design. The status quo — innocence — has the advantage. Changing that status quo requires meeting a high bar. The defense doesn't have to prove anything; the prosecution must prove nearly everything.

Hypothesis testing works exactly this way. Every test has a **null hypothesis**, written $H_0$ — the status quo, the default, the claim we start from. And it has an **alternative hypothesis**, written $H_a$ — the challenger, the new claim, what we'd conclude if the evidence against the null is strong enough.

The null hypothesis is always a claim of no effect, no change, no difference. "The drug does nothing." "The machine is properly calibrated." "The proportion of voters who support the measure is 50%." The null is the thing you continue believing in the absence of evidence. It always includes an equality sign — $\mu = 150$, $p = 0.50$, $\mu \leq 40$ — because you're claiming the parameter sits at or below some specific value.

The alternative is the contender. It never includes an equals sign. It says: the parameter is not that value, or it's higher, or it's lower.

The question the whole procedure asks is: are the data consistent with the null hypothesis, or have they strayed so far from what the null predicts that the null becomes unbelievable?

![The null starts with the advantage. The alternative must earn the win.](images/09-hypothesis-testing-with-one-sample-fig-01.png)
*Figure 9.1 — Two-column diagram titled "The asymmetry of the burden*

---

## Innocent until the evidence is extreme enough

Here is how you decide whether the evidence is extreme enough.

You assume the null hypothesis is true. Under that assumption, you ask: how likely is it that you'd observe data this extreme, or more extreme, just by chance?

That probability is the **p-value**.

A small p-value means: if the null were true, data this extreme would be very rare. So either something very unusual happened, or the null is wrong. We tend to conclude the null is wrong.

A large p-value means: this data is not surprising under the null. No reason to abandon the null.

The threshold for "small enough" is $\alpha$, the **significance level**. You set it before you look at the data. The most common choice is $\alpha = 0.05$: you will reject the null if the probability of data this extreme (under the null) is less than 5%.

Decision rule: if p-value $< \alpha$, reject $H_0$. If p-value $\geq \alpha$, fail to reject $H_0$.

The language matters. You never say "accept the null." Failing to reject the null means the data are consistent with it — not that the null is true. The null might be false, but you don't have enough evidence to say so. The court verdict is "not guilty," not "innocent."

---

## What the p-value is not

The most common misunderstanding about p-values is worth addressing directly, because it leads to real errors in real science.

The p-value is the probability of observing data this extreme, **given that the null hypothesis is true**. It is a conditional probability in a specific direction: $P(\text{data this extreme} \mid H_0 \text{ true})$.

It is not the probability that the null hypothesis is true given the data. That would be $P(H_0 \text{ true} \mid \text{data this extreme})$ — the reversed conditional, entirely different, and exactly what you'd want to know but cannot compute from the test alone.

Think about what this means. A p-value of 0.04 says: if the null is true, there is a 4% chance of seeing data like mine. It does not say: there is a 4% chance the null is true. Those are different statements about different probabilities pointing in opposite directions.

The doctor who interprets a p-value of 0.04 as "96% confidence that the drug works" has made a version of the same error the mammogram-reading doctor made in Chapter 3. That chapter showed that $P(\text{positive} \mid \text{cancer}) \neq P(\text{cancer} \mid \text{positive})$. Here: $P(\text{data} \mid H_0) \neq P(H_0 \mid \text{data})$.

Hold that distinction. It will matter every time you read a scientific result for the rest of your life.

![The p-value runs in the wrong direction for the question you actually want answered.](images/09-hypothesis-testing-with-one-sample-fig-02.png)
*Figure 9.2 — Two-arrow diagram side by side*

---

## Two kinds of error, one unavoidable trade-off

There are two ways a hypothesis test can go wrong.

You can reject the null when it's actually true. This is a **Type I error** — a false positive. You convicted an innocent defendant. The probability of committing this error is $\alpha$. You control it directly by your choice of significance level.

You can fail to reject the null when it's actually false. This is a **Type II error** — a false negative. You let a guilty defendant go free. The probability of committing this error is $\beta$. The probability of correctly rejecting a false null is $1 - \beta$, called the **power** of the test.

Here is the uncomfortable truth: you cannot drive both $\alpha$ and $\beta$ to zero simultaneously. If you make it harder to reject the null (lowering $\alpha$), you make it more likely you'll fail to reject a false null (raising $\beta$). The errors are in tension. You can trade one for the other, but you cannot eliminate both.

This is why the choice of $\alpha$ is not arbitrary. It depends on the consequences of each error.

A worked example that makes this concrete. Suppose the null hypothesis is: "This sleeping bag is rated to withstand $-15°$F." You're testing whether to trust the rating before a winter camping trip.

A Type I error: you conclude the bag fails when it actually would have kept you warm. You leave your gear at home. You're cold, but alive.

A Type II error: you conclude the bag is fine when it cannot actually handle $-15°$F. You bring the bag. You sleep outside in extreme cold with inadequate equipment. This is a life-threatening mistake.

The consequences are not symmetric. The Type II error is far more dangerous. Rationally, you should set $\alpha$ high — maybe 0.20 — to make it easy to reject the bag's rating. A high $\alpha$ means more false rejections (Type I errors), but it drives down $\beta$ (Type II errors). You accept the trade-off deliberately, because the two errors have different consequences.

Medical testing works the same way, in reverse. For a screening test, a Type II error — missing a disease that's present — is usually worse than a Type I error — flagging someone for further testing who turns out to be fine. So you set $\alpha$ relatively high, accepting more false positives to minimize false negatives.

For drug approval, the calculation flips. Approving a drug that doesn't work (and might have side effects) is a Type I error. Not approving a drug that does work is a Type II error. Regulators typically set $\alpha = 0.05$ or even $0.01$, demanding strong evidence before approval, accepting some false negatives as the price of controlling false positives.

The framework doesn't tell you which error is worse. That's a judgment about stakes, not statistics. What the framework does is make the trade-off explicit and give you the tools to control it deliberately.

| H₀ actually true | H₀ actually false |
| --- | --- |
| "Reject H₀ | Fail to reject H₀." Columns: "H₀ actually true |
| Reject, False) = "Correct decision | power = 1−β" |
| Fail to reject, True) = "Correct decision | probability 1−α" |
| Fail to reject, False) = "Type II error | false negative |

---

## Building the machinery: the test statistic

Now we can build the mechanics.

Under the null hypothesis, you assume the population mean equals $\mu_0$ (the hypothesized value). The central limit theorem, from Chapter 7, tells you that if you take many samples of size $n$ from this population, the sample means will be normally distributed with mean $\mu_0$ and standard error $\sigma/\sqrt{n}$.

Your actual sample gives you a sample mean $\bar{x}$. The question is: how far is $\bar{x}$ from $\mu_0$, measured in standard errors? Far enough to be surprising? Or is it the kind of variation you'd expect just from random sampling?

The answer is the **test statistic**:

$$Z = \frac{\bar{x} - \mu_0}{\sigma/\sqrt{n}}$$

This is a z-score. It tells you how many standard errors your sample mean is from the hypothesized population mean. If $Z = 0.5$, your sample mean is half a standard error above $\mu_0$ — unremarkable, consistent with random sampling. If $Z = 3.2$, your sample mean is 3.2 standard errors above $\mu_0$ — quite far out in the tail of the distribution.

In practice, you rarely know the population standard deviation $\sigma$. You estimate it with the sample standard deviation $s$. When you do this substitution, the test statistic follows not the normal distribution but the **t-distribution**:

$$t = \frac{\bar{x} - \mu_0}{s/\sqrt{n}}$$

with $n - 1$ degrees of freedom. The t-distribution is wider and flatter than the normal — it has heavier tails — because you're carrying extra uncertainty from estimating $\sigma$. As sample size grows, the t-distribution converges to the normal. With $n \geq 30$, the difference is small.

When to use which:

- Know $\sigma$, or $n \geq 30$ and population is approximately normal: use the z-test.
- Don't know $\sigma$ and $n < 30$: use the t-test.
- Don't know $\sigma$ but $n \geq 30$: either works; the t-test is technically more correct, the difference is negligible.

![The t-distribution accounts for uncertainty in estimating σ. With small samples, the tails are heavier. By n=30, the difference is negligible.](images/09-hypothesis-testing-with-one-sample-fig-03.png)
*Figure 9.3 — Overlapping density curves on the same axes*

---

## The five-step procedure

Every hypothesis test follows the same structure. Learn the structure once and you can apply it to any test you ever conduct.

**Step 1 — State the hypotheses.** Write $H_0$ and $H_a$ explicitly. Identify whether the test is one-tailed (the alternative specifies a direction: $\mu > \mu_0$ or $\mu < \mu_0$) or two-tailed (the alternative allows deviation in either direction: $\mu \neq \mu_0$). This choice changes where the rejection region lives and how you interpret the test statistic.

**Step 2 — Set the significance level.** Choose $\alpha$ before looking at the data. 0.05 is conventional. But the right choice depends on the consequences of each error type, not on convention.

**Step 3 — Compute the test statistic.** Calculate $Z$ or $t$ from your sample data. This locates your sample mean on the distribution of sample means under the null.

**Step 4 — Find the p-value.** The p-value is the area in the tail of the distribution beyond your test statistic. For a two-tailed test, you double the one-tail area (since deviation in either direction counts). Compare the p-value to $\alpha$.

**Step 5 — Conclude.** If p-value $< \alpha$, reject $H_0$ and state what you've found in plain language. If p-value $\geq \alpha$, fail to reject $H_0$ and note that the data are consistent with the null. Always state what the test was about and what the conclusion means for the real question you were trying to answer.

A worked example that follows all five steps.

Jeffrey, age eight, has been swimming the 25-yard freestyle in a mean time of 16.43 seconds with standard deviation 0.8 seconds. His father buys him goggles and times 15 swims. The new mean is 16.0 seconds. Did the goggles help?

*Step 1 — Hypotheses:*

$H_0: \mu = 16.43$ (the goggles had no effect; any difference is random)

$H_a: \mu < 16.43$ (the goggles made him faster)

One-tailed test, lower tail.

*Step 2 — Significance level:* $\alpha = 0.05$.

*Step 3 — Test statistic:* $n = 15$, $\bar{x} = 16.0$, $\sigma = 0.8$ known from previous swims.

$$Z = \frac{16.0 - 16.43}{0.8/\sqrt{15}} = \frac{-0.43}{0.2065} = -2.08$$

*Step 4 — P-value:* The area to the left of $Z = -2.08$ on the standard normal is approximately 0.019.

*Step 5 — Conclude:* Since $0.019 < 0.05$, reject $H_0$. At the 5% significance level, there is sufficient evidence to conclude that Jeffrey's mean swimming time is less than 16.43 seconds with the goggles. Frank's belief is supported by the data.

![The test statistic lands in the rejection region. The data are rare enough under the null to reject it.](images/09-hypothesis-testing-with-one-sample-fig-04.png)
*Figure 9.4 — Standard normal curve for Jeffrey's one-tailed test*

---

## Testing proportions

The same logic applies when the parameter of interest is a proportion rather than a mean.

Suppose a bank believes that 50% of first-time borrowers take out smaller loans than repeat borrowers. They sample 100 first-time borrowers and find 53 took out smaller loans. Is 53% different enough from 50% to reject the claim?

The test statistic for a proportion is:

$$Z = \frac{p' - p_0}{\sqrt{p_0 q_0 / n}}$$

where $p'$ is the sample proportion, $p_0$ is the hypothesized proportion, and $q_0 = 1 - p_0$. This works when $np'$ and $nq'$ are both at least 5 — the sample is large enough that the normal approximation to the binomial is reasonable.

For the bank example:

$H_0: p = 0.50$, $H_a: p \neq 0.50$ (two-tailed, since we're asking whether the proportion differs in either direction).

$$Z = \frac{0.53 - 0.50}{\sqrt{(0.50)(0.50)/100}} = \frac{0.03}{0.05} = 0.60$$

The p-value for a two-tailed test is the area in both tails beyond $|Z| = 0.60$: approximately $2 \times 0.274 = 0.548$.

Since $0.548 > 0.05$, fail to reject $H_0$. The data are consistent with the bank's claim that 50% of first-time borrowers take out smaller loans. A difference of 3 percentage points in a sample of 100 is well within what random sampling can produce.

![Z = 0.60 barely moves from center. With a p-value of 0.548, the data offer no reason to doubt the null.](images/09-hypothesis-testing-with-one-sample-fig-05.png)
*Figure 9.5 — Standard normal curve for the bank's two-tailed proportion*

---

## When the machinery gets misused

Hypothesis testing is genuinely powerful. It is also among the most misused tools in science, and you will encounter the misuse constantly if you read research.

The specific problem is called **p-hacking**, and it works like this.

Imagine you conduct 20 hypothesis tests, each on a null hypothesis that is actually true. By definition, $\alpha = 0.05$ means you expect to reject a true null 5% of the time just by chance. In 20 tests, that means you expect roughly one false rejection — one "statistically significant" result that doesn't reflect any real effect.

If you publish all 20 tests, this is fine. Readers see the false positive in context. But if you publish only the one "significant" result and quietly file away the other 19, readers see what looks like strong evidence of an effect that doesn't exist.

This is p-hacking. It happens not just through deliberate concealment but through a hundred small decisions: which outcome variables to examine, which subgroups to analyze, whether to include or exclude outliers, when to stop collecting data. Each decision is a fork in the analysis, and the researcher can (consciously or not) follow the forks that produce smaller p-values.

The result is the **replication crisis**: a set of high-profile findings in psychology, medicine, and other fields that turned out not to replicate when independent labs tried to reproduce them. The original studies had p < 0.05. The replications did not. The most likely explanation is p-hacking, combined with the publication bias that favors significant results.

How to protect against it:

Pre-register your analysis. Before collecting data, write down exactly what hypothesis you will test, what test statistic you will use, and what significance level you will apply. A p-value from a pre-registered test accurately describes the probability of that data under the null. A p-value from an analysis that tried many things means less.

Report actual p-values, not just whether you rejected or failed to reject. A p-value of 0.049 and a p-value of 0.0003 are both "significant" at $\alpha = 0.05$, but they tell very different stories about how far the data are from the null.

Ask about effect size. A drug that reduces pain by 0.2 points on a 100-point scale might have p < 0.05 with a large enough sample. Statistical significance and practical significance are not the same thing. The p-value measures rarity; the effect size measures magnitude. Both matter.

Demand replication. One study with p < 0.05 is interesting. The same result from three independent labs is convincing.

![Running 20 tests at α = 0.05 gives roughly a 64% chance of at least one false positive — even when no real effects exist. This is the arithmetic of p-hacking.](images/09-hypothesis-testing-with-one-sample-fig-06.png)
*Figure 9.6 — Line chart *

---

## What the FDA committee understood

Return to the advisory committee in 2004. They are not running a hypothesis test mechanically. They understand the logic behind it.

They understand that $\alpha = 0.05$ is a policy choice, not a natural law. Setting it at 0.05 means: we will accept a 5% chance of approving a drug that doesn't work. That's the Type I error rate the regulatory system has chosen to tolerate. Different standards might demand 0.01 — a stricter threshold, fewer false approvals, but also longer wait times for drugs that really work.

They understand that a p-value less than 0.05 doesn't tell them how large the effect is. A drug might be statistically significant but clinically useless. They need to see the effect size too: how much did patients actually improve, not just whether the improvement was unlikely under a null of zero effect.

And they understand that the burden of proof is asymmetric deliberately. The status quo — no approval — protects patients from ineffective or harmful drugs. Overturning that status quo requires meeting a high standard of evidence. This is not skepticism about science; it is a rational policy about what errors are acceptable.

The entire framework of hypothesis testing is designed to implement that rationality. It does not tell you whether a drug works. It tells you whether the evidence is strong enough, by a specified standard, to reject the claim that it doesn't. That conditional structure — testing against a null, not proving a hypothesis — is not a limitation. It's the whole point.

You can now set up that test, compute the statistic, interpret the p-value correctly, understand what errors you might be making and why, and recognize when someone else is using the machinery dishonestly. That is a lot to have added to your toolkit in one chapter. The next chapter extends it: two populations instead of one, and the same five steps.

---

## Exercises

### Warm-up

**9.1** *(Stating hypotheses.)* For each scenario, write $H_0$ and $H_a$, and state whether the test is one-tailed or two-tailed. (a) A battery manufacturer claims its product lasts at least 40 hours. A consumer testing lab wants to verify the claim. (b) A university researcher tests whether students at their college sleep a different number of hours per night than the national average of 7 hours. (c) A quality control engineer tests whether a machine filling cereal boxes is dispensing exactly 18 oz on average.

**9.2** *(Type I and Type II errors in context.)* For scenario 9.1(c): (a) Describe a Type I error in plain language. What happens as a result? (b) Describe a Type II error. What happens? (c) Which error is more costly for the cereal company? Explain your reasoning.

**9.3** *(Computing a z-statistic.)* A sample of 49 students has a mean exam score of 74. The population standard deviation is known to be 7. Test whether the mean differs from 70 at the 5% significance level. Compute the z-statistic and determine whether to reject $H_0$.

### Application

**9.4** *(Full five-step t-test.)* A coffee shop claims its employees prepare cappuccinos in a mean of 3.5 minutes. An auditor samples 16 cappuccinos and finds a mean of 3.8 minutes with standard deviation 0.6 minutes. At $\alpha = 0.05$, test whether preparation time exceeds 3.5 minutes. Write out all five steps.

**9.5** *(Proportion test.)* A school district believes 60% of its students walk or bike to school. In a survey of 200 students, 108 report walking or biking. At $\alpha = 0.05$, test whether the true proportion differs from 60%. Show the test statistic calculation and state your conclusion.

**9.6** *(Interpreting a p-value.)* A researcher reports: "We found a statistically significant effect (p = 0.031, α = 0.05)." (a) What does the p-value of 0.031 mean, precisely? (b) Does it tell you the probability the null hypothesis is true? Why or why not? (c) Does it tell you whether the effect is practically important?

### Synthesis

**9.7** *(Choosing α deliberately.)* A hospital is evaluating a new rapid test for a serious infection. The test will be used to decide whether to isolate patients immediately. (a) What is the Type I error in this context? What does it cost? (b) What is the Type II error? What does it cost? (c) Would you set $\alpha = 0.01$ or $\alpha = 0.10$ for this test? Justify your choice in terms of the error trade-off.

**9.8** *(Full five-step t-test — student sleep data.)* A researcher claims that college students sleep fewer than 7 hours per night on average. A sample of 22 students yields a mean of 6.4 hours and a standard deviation of 1.8 hours. At $\alpha = 0.05$, conduct the full five-step hypothesis test. Does the evidence support the researcher's claim?

### Challenge

**9.9** *(p-hacking and multiple comparisons.)* A marketing team runs 15 independent A/B tests simultaneously, each at $\alpha = 0.05$, all on campaigns that have no real effect. (a) What is the probability that at least one test produces a "significant" result by chance alone? (Use $P(\text{at least one false positive}) = 1 - 0.95^{15}$.) (b) The team reports the one "winning" campaign. What is wrong with this? (c) Suggest two changes to their process that would make the result more trustworthy.

**9.10** *(Statistical vs. practical significance.)* A pharmaceutical company tests a new pain medication on 10,000 patients. The drug reduces pain scores by an average of 0.3 points on a 100-point scale. The p-value is 0.0001. (a) Is the result statistically significant at $\alpha = 0.05$? (b) Is a 0.3-point improvement on a 100-point scale clinically meaningful? (c) What does this example reveal about the relationship between sample size, p-values, and practical importance? What additional information would you want before recommending this drug?

---

## LLM Exercise — Chapter 9: Hypothesis Testing with One Sample (Analyze One Dataset Project)

**Project:** Analyze One Real Dataset.
**What you're building this chapter:** the first hypothesis test — testing a specific claim about your dataset.
**Tool:** **Claude Code** + **Claude Project**.

---

**The Prompt:**

```
Chapter 9 of my Analyze-One-Dataset project. Chapter 9 covered
the four-step hypothesis-test procedure:
1. State H₀ and H_a (and decide one-tailed vs. two-tailed).
2. Compute the test statistic (t for means; z for proportions).
3. Find the p-value (probability of test statistic this extreme
   or more, under H₀).
4. Compare p-value to α (typically 0.05); reject H₀ if p < α,
   else fail to reject.
Type I error (false positive: reject H₀ when it's true) probability
= α. Type II error (false negative: fail to reject H₀ when it's
false) probability = β. Power = 1 − β. The p-value is NOT the
probability H₀ is true; it's the probability of seeing data this
extreme IF H₀ were true.

Write the brief's "One-Sample Hypothesis Test" section in 500-800
words.

1. **State a specific claim to test.** Pick a defensible
   benchmark — a population mean someone has claimed, a
   proportion threshold (e.g., is the proportion of X different
   from 0.50?), a regulatory standard, a previously-published
   result.

2. **State the hypotheses formally.**
   - H₀: parameter = specific value (the no-effect statement).
   - H_a: parameter ≠ specific value (two-tailed) OR > / <
     (one-tailed, justified by directional theory).
   - Significance level α (typically 0.05).

3. **Compute the test statistic.**
   - For one-sample t-test (mean): t = (x̄ − μ₀) / (s/√n).
   - For one-sample z-test (proportion): z = (p̂ − p₀) /
     √(p₀(1−p₀)/n).
   - Show the computation with actual numbers.

4. **Find the p-value.**
   - For t-test: use scipy.stats.t (sf for upper, 2× for two-
     tailed).
   - For z-test: use scipy.stats.norm.
   - Report the p-value to 3-4 decimal places.

5. **Make and interpret the decision.**
   - "Reject H₀" or "Fail to reject H₀."
   - State the conclusion in plain language: "the data provide
     [strong / moderate / weak / no] evidence against the
     null hypothesis that [restate]."
   - Do NOT say "accept H₀" — failing to reject is not the same
     as proving the null.

6. **Discuss error risk.** With α = 0.05, what's the probability
   we made a Type I error (assuming H₀ is actually true)? What
   could we say about Type II error (we'd need to specify the
   true parameter to compute it)?

End with: how would the conclusion change if we used α = 0.01
instead? What does the interplay between α and conclusion say
about "significance" as a discipline?
```

---

**What this produces:** A 500-800 word section with one formal hypothesis test. The p-value-interpretation discipline is the most consequential output.

**How to adapt this prompt:**

- *For your own project:* The test should answer a real question. A "test for the sake of testing" produces sterile output.
- *For ChatGPT / Gemini:* Works as written.
- *For Claude Code:* `scipy.stats.ttest_1samp(data, popmean)` for the one-liner. Building from scratch teaches the underlying mechanics.
- *For a Claude Project:* Append.

**Connection to previous chapters:** Ch 8's CIs and Ch 9's tests are dual: if the null value is outside the 95% CI, then p < 0.05. Verify this on your data.

**Preview of next chapter:** Chapter 10 covers two-sample tests. You'll compare two groups in your dataset.

---

## AI Wayback Machine

**William Gosset** wrote as "Student" while working at Guinness — and introduced the t-test in 1908 to handle small-sample inference.

**Run this:**

```
Who is William Gosset, and how does their work connect to hypothesis testing we covered in this chapter? Keep it to three paragraphs. End with the single most surprising thing about their career or ideas.
```

→ Search **"William Gosset"** on Wikipedia.

**Now make the prompt better.** Try one of these:

- Ask it to apply William Gosset's framework to a small dataset you actually have.
- Add a constraint: "Answer including criticisms or limits of William Gosset's framework."

What changes? What gets better? What gets worse?
