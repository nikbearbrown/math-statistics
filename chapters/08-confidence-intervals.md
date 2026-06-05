# Chapter 8 — Confidence Intervals

## TL;DR

- On the honest way to express what you don't know.
- The chapter moves through Why one number is never enough, What "95% confident" actually means, Building a confidence interval when you know $\sigma$, The problem Gossett solved, and related ideas.
- Read it for the main argument, the vocabulary it introduces, and the practical judgment it asks you to develop.

*On the honest way to express what you don't know.*

---

Tuesday, 6:17 p.m. A woman on the phone says she's conducting a survey for the county health department. She asks how many days in the last month your mental health was not good. You pause and say seven. She thanks you and hangs up.

On the other end, 499 other people have answered the same question. She calculates the sample mean: 10.2 days. The sample standard deviation: 6.8 days. She will never know the true population mean — the average across every resident in the county. But she can build a range.

She reports to her supervisor: "We estimate with 95% confidence that the true county average is between 9.3 and 11.1 days."

That sentence is doing something very precise. It is not saying the true mean is probably in that range. It is not saying the true mean is definitely in that range. It is saying something more careful, and more useful, than either of those things — and understanding exactly what it says is the whole point of this chapter.

---

## Why one number is never enough

When you ask "what is the average?" and get a single answer back, you're looking at a point estimate. It's your best single guess. It's also, almost certainly, wrong — not because the arithmetic was bad, but because samples bounce. A different random sample of 500 people would give a different sample mean, and that one would also be your best guess, and it would be different from the first one.

The central limit theorem, from Chapter 7, told you how much the bouncing happens. If you draw repeated samples of size $n$ from a population with mean $\mu$ and standard deviation $\sigma$, the sample means distribute normally around $\mu$ with a standard deviation of $\sigma / \sqrt{n}$. That quantity — $\sigma / \sqrt{n}$ — is the standard error. It measures how much the sample mean typically strays from the true mean.

Now here's the move. You know the sampling distribution of the mean. You know that 95% of all sample means fall within $\pm 1.96$ standard errors of the true mean. So if your particular sample mean is one of that 95%, then the true mean falls within $\pm 1.96$ standard errors of your sample mean.

That's it. That's the confidence interval.

$$\bar{x} - z^* \cdot \frac{\sigma}{\sqrt{n}} \leq \mu \leq \bar{x} + z^* \cdot \frac{\sigma}{\sqrt{n}}$$

The $z^*$ is the critical value — the z-score that marks the boundary of your chosen confidence level. For 95% confidence, $z^* = 1.96$. For 90%, $z^* = 1.645$. For 99%, $z^* = 2.576$. The quantity $z^* \cdot \frac{\sigma}{\sqrt{n}}$ is the margin of error — how far you extend on each side of the sample mean.

| Confidence Level | z* critical value | Interpretation |
| --- | --- | --- |
| 90% | 1.645 | "Miss 10% of the time", 95% |
| quick lookup card students will return to throughout Chapters 8–13 | A concrete checkpoint for applying the chapter concept. | A concrete checkpoint for applying the chapter concept. |

---

## What "95% confident" actually means

Here is the misunderstanding you will see repeated in newspapers, in medical papers, in boardrooms. People look at a confidence interval — say, (9.3, 11.1) — and say: "There's a 95% chance the true mean is in there."

This is wrong. And it's wrong in a subtle way that's worth understanding completely.

Once you've built the interval, the true mean is either in it or it isn't. It's a fixed number. The interval is a fixed range. Probability doesn't apply to fixed things — probability applies to random processes before the outcome is determined.

What 95% confidence actually means is a statement about the *procedure*, not about *this* interval. If you repeated the sampling and interval-building process 100 times — 100 independent surveys of 500 people, 100 confidence intervals computed — then about 95 of those 100 intervals would contain the true mean. The other 5 would miss it entirely.

Your interval is one of those 100. It either hit or missed. You'll never know which. But if the procedure is trustworthy (random sample, correct formula), you should trust that the procedure works 95% of the time.

![95 of 100 intervals contain the true mean — but you never know which intervals those are](images/08-confidence-intervals-fig-01.png)
*Figure 8.1 — Horizontal dot plot showing 100 confidence intervals stacked*

Let me be concrete. Suppose the true county average really is 10.7 days. The survey produced an interval of (9.3, 11.1). That interval contains 10.7, so it hit. But the survey team doesn't know 10.7 is the true value — they never will. They know only that they used a procedure that works 95% of the time. That's what they mean when they say "95% confidence."

This distinction matters practically. When a pharmaceutical company reports that a drug reduces blood pressure by 5 to 15 mmHg with 95% confidence, they're not saying the true effect is probably between 5 and 15. They're saying they used a procedure that, in 95% of trials, captures the true effect. The drug could be one of the 5% who missed. The data doesn't tell you which.

---

## Building a confidence interval when you know $\sigma$

Let me work through the mechanics once, slowly, so the structure is clear.

A streaming company surveys 100 random users and finds a sample mean of $\bar{x} = 2.0$ songs downloaded per month. The company knows from historical data that the population standard deviation is $\sigma = 1$ song. They want a 95% confidence interval for the true mean.

**Step 1.** The standard error: $SE = \sigma / \sqrt{n} = 1 / \sqrt{100} = 0.1$.

**Step 2.** The critical value: $z^* = 1.96$ for 95% confidence.

**Step 3.** The margin of error: $EBM = 1.96 \times 0.1 = 0.196$.

**Step 4.** The interval: $2.0 \pm 0.196$, which is $(1.804, 2.196)$.

Interpretation: we are 95% confident — meaning, we used a procedure that captures the true mean 95% of the time — that the true average number of songs downloaded per month is between 1.8 and 2.2.

Notice two things. Larger samples narrow the interval, because $\sigma / \sqrt{n}$ shrinks as $n$ grows. Higher confidence levels widen the interval, because $z^*$ grows when you demand more confidence. These are the two levers you control. They pull in opposite directions: more confidence means a wider net; more data means a tighter net.

![2×2 grid showing four interval widths for the](images/08-confidence-intervals-fig-02.png)
*Figure 8.2 — 2×2 grid showing four interval widths for the*

---

## The problem Gossett solved

The formula above requires knowing $\sigma$, the population standard deviation. In practice, you almost never know it. You know only $s$, the standard deviation of your sample.

The obvious move: just substitute $s$ for $\sigma$. And indeed, that's essentially what you do. But there's a catch that a brewer from Dublin discovered in 1908.

William Gossett worked at the Guinness brewery running small experiments on barley and hops. His sample sizes were rarely more than ten or fifteen. He built confidence intervals using the normal distribution, substituting $s$ for $\sigma$. And his intervals kept failing: the true parameter fell outside his interval more than 5% of the time. Something was wrong.

The problem: $s$ itself is random. It's computed from the same sample as $\bar{x}$, so it bounces around. When $n$ is small, $s$ can stray substantially from $\sigma$. Using the normal distribution as if $s$ were a constant underestimates the true uncertainty.

Gossett worked out the correct distribution. It looks like the normal curve — symmetric, bell-shaped, centered at zero — but with heavier tails. More probability sits in the extremes. This makes sense: because $s$ is uncertain, the interval needs to extend farther to maintain its coverage guarantee. The extra width is the honest price of not knowing $\sigma$.

He published his finding under the pen name "Student" — Guinness didn't want competitors knowing that their brewers were doing statistics. The distribution is now called the **Student's t-distribution**, and the critical values come from a t-table rather than a z-table.

The t-distribution has one parameter: the **degrees of freedom**, equal to $n - 1$.

Why $n - 1$? When you compute the sample standard deviation $s$, you first compute the sample mean $\bar{x}$, then measure how far each observation strays from $\bar{x}$. Those $n$ deviations must sum to exactly zero — that's a constraint baked into the definition of the mean. So the last deviation is completely determined by the first $n - 1$. Only $n - 1$ values are free to vary. One degree of freedom was spent calculating the mean.

The t-distribution with $df$ degrees of freedom has heavier tails than the normal, and the difference is most pronounced when $df$ is small. As $df$ increases — as $n$ gets larger — the t-distribution converges to the standard normal. By around $df = 30$, the two are nearly identical. This is why practitioners sometimes say "use t for small samples and z for large ones" — but the more precise rule is: use t whenever you don't know $\sigma$. For large $n$, it doesn't much matter, but t is always correct.

![Overlapping density curves ](images/08-confidence-intervals-fig-03.png)
*Figure 8.3 — Overlapping density curves *

The confidence interval formula for a mean with unknown $\sigma$:

$$\mu = \bar{x} \pm t_{n-1, \, \alpha/2} \cdot \frac{s}{\sqrt{n}}$$

Everything is the same as the z-formula except: $s$ replaces $\sigma$, and the critical value comes from the t-table at $df = n-1$ and the chosen confidence level.

Let me show you the difference in a concrete case. A financial analyst samples 10 stocks from the Dow Jones Industrial Average and finds a mean earnings per share of $\bar{x} = 1.85$ with sample standard deviation $s = 0.395$. They want a 99% confidence interval.

$df = 9$. From the t-table at $df = 9$ and 99% confidence, the critical value is $t_{9, \, 0.005} = 3.250$.

Standard error: $SE = 0.395 / \sqrt{10} = 0.125$.

Margin of error: $EBM = 3.250 \times 0.125 = 0.406$.

Interval: $1.85 \pm 0.406$, giving $(1.444, 2.256)$.

If you mistakenly used the z-table and $z^* = 2.576$, you'd get a narrower interval: $1.85 \pm 0.322$, giving $(1.528, 2.172)$. That narrower interval is overconfident — it claims more precision than the data actually support, because it ignores the uncertainty in $s$. Gossett's contribution was recognizing that this extra overconfidence is not a small rounding error; it's a systematic underestimate that compounds across small samples.

The wider t-interval isn't a failure. It's an honest accounting of what you actually know.

---

## Proportions: The same logic, different shape

Everything we've done so far estimates a mean — an average of numerical measurements. But sometimes you want to estimate a proportion: what fraction of voters support this candidate, what fraction of devices fail within a year, what fraction of households own a tablet.

The sample proportion is $\hat{p} = x / n$, where $x$ is the number of successes and $n$ is the sample size. This is your point estimate of the true population proportion $p$.

By the central limit theorem, when $n$ is large enough, $\hat{p}$ is approximately normally distributed with mean $p$ and standard deviation $\sqrt{p(1-p)/n}$. Since we don't know $p$, we substitute $\hat{p}$:

$$SE = \sqrt{\frac{\hat{p}(1 - \hat{p})}{n}}$$

The confidence interval:

$$p = \hat{p} \pm z^* \sqrt{\frac{\hat{p}(1 - \hat{p})}{n}}$$

Two things to notice. First, this uses $z^*$, not $t$ — because the formula is an approximation to the normal that works when $n$ is large, and for large $n$, $z$ and $t$ agree. Second, the approximation only works when both $n\hat{p} \geq 5$ and $n(1 - \hat{p}) \geq 5$. These conditions ensure the sampling distribution is close enough to normal. If you're estimating a rare event — say, a 1% failure rate — you need a much larger sample before the normal approximation is trustworthy.

Let me work through one example. A market research firm surveys 500 adults in a city. Of them, 421 own a smartphone. Build a 95% confidence interval.

$\hat{p} = 421/500 = 0.842$. Check conditions: $500 \times 0.842 = 421 \geq 5$ and $500 \times 0.158 = 79 \geq 5$. Both satisfied.

Standard error: $SE = \sqrt{0.842 \times 0.158 / 500} = \sqrt{0.000266} = 0.0163$.

Margin of error: $EBM = 1.96 \times 0.0163 = 0.032$.

Interval: $0.842 \pm 0.032$, which is $(0.810, 0.874)$.

We are 95% confident that between 81.0% and 87.4% of adults in the city own smartphones.

One interesting quirk: the margin of error is largest when $\hat{p} = 0.5$, because the product $\hat{p}(1 - \hat{p})$ is maximized at $0.5$. A 50-50 split has the most uncertainty; an extreme proportion (near 0 or 1) has less, because when almost everyone or almost no one has the characteristic, your estimate is inherently more precise. The 95% split in our smartphone example gives a narrower interval than you'd get if the split were 50-50.

![Margin of error is widest when the split is closest to 50-50](images/08-confidence-intervals-fig-04.png)
*Figure 8.4 — Parabolic curve of p̂(1-p̂) as a function of*

---

## How many people do you need?

The question that always follows a confidence interval: "Was my sample big enough? If I want more precision, how much more data do I need?"

Start from the margin of error formula: $EBM = z^* \cdot \sqrt{\hat{p}(1-\hat{p})/n}$. Solve for $n$:

$$n = \frac{(z^*)^2 \cdot \hat{p}(1 - \hat{p})}{e^2}$$

where $e$ is the desired margin of error.

There's a circular problem: to compute $n$, you need $\hat{p}$, but you're computing $n$ to determine how many samples to take to find $\hat{p}$. Three ways around it: use a prior estimate from a previous study, use the worst case $\hat{p} = 0.5$ (which gives the largest possible $n$ and hence the most conservative estimate), or run a small pilot study to get a rough $\hat{p}$ first.

The county health department wants a margin of error of $\pm 2.5\%$ at 95% confidence. No prior data. Use $\hat{p} = 0.5$.

$$n = \frac{(1.96)^2 \times 0.25}{(0.025)^2} = \frac{0.9604}{0.000625} = 1537$$

They need to survey about 1,537 people.

Now here's the relationship that surprises most people the first time they see it. To cut the margin of error in half — from $\pm 2.5\%$ to $\pm 1.25\%$ — you don't need twice as many people. You need *four times* as many. The margin of error goes as $1/\sqrt{n}$, so halving it requires multiplying $n$ by four.

$$n = \frac{(1.96)^2 \times 0.25}{(0.0125)^2} = \frac{0.9604}{0.000156} = 6,157$$

This is why precision is expensive. A poll accurate to $\pm 3\%$ takes about 1,000 people. A poll accurate to $\pm 1\%$ takes about 9,600. News organizations don't run 10,000-person polls; they accept a $\pm 3\%$ margin and live with the uncertainty. That's not laziness — it's economics.

![Halving the margin of error quadruples the required sample — the cost of precision is not linear](images/08-confidence-intervals-fig-05.png)
*Figure 8.5 — Curve of required sample size n (vertical axis,*

The same relationship holds for means. The sample size needed to achieve a margin of error $e$ for a mean is:

$$n = \left(\frac{z^* \cdot \sigma}{e}\right)^2$$

Again, halving the margin of error requires quadrupling the sample size.

---

## The shape of what you know

There's a clean way to picture everything in this chapter. Imagine the sampling distribution of your estimator — it's a bell curve centered at the true parameter, with spread equal to the standard error. Your sample gave you one point on that curve. The confidence interval is a window centered at your point, wide enough to capture the true parameter in most repetitions of the sampling procedure.

Narrowing the window: larger $n$ shrinks the standard error, pulling the bell curve tighter, so a narrower window still captures the center most of the time.

Widening the window: higher confidence level means you need the window to succeed in more repetitions, so it has to be wider.

The t-distribution adds one more wrinkle: when $\sigma$ is unknown, you're building the window with a ruler (the sample standard deviation $s$) that itself has some measurement error. The t-distribution's heavier tails are the honest accounting for that extra source of uncertainty. As $n$ grows, your ruler becomes more precise, the t and z curves converge, and the extra width disappears.

One final thought. Every margin of error you'll read in a newspaper, every "plus or minus" attached to a poll, every "confidence interval" in a medical journal — they are all doing exactly what this chapter describes. They are converting a sample into an honest expression of what you know and how much you're guessing. The 95% confidence level doesn't mean you're 95% right. It means you're using a procedure that's right 95% of the time, and you can't know whether this particular application is one of the 95% or one of the 5%.

That honest uncertainty is not a failure of statistics. It's what statistics is for.

---

## What would change my mind

If I repeatedly observed that my 95% confidence intervals contained the true parameter far less than 95% of the time — say, only 80% — I would look first for violations of assumptions: non-random sampling, non-normal populations with small samples, or underestimates of $\sigma$. If those were all ruled out and the shortfall persisted, I would need to revise my understanding of where the coverage guarantee breaks down.

The derivation that replacing $\sigma$ with $s$ requires exactly the t-distribution rather than some other heavy-tailed distribution is mathematically clean — it follows from the ratio of a standard normal to a chi-squared variable — but I don't yet have a geometric or physical picture of *why* that particular ratio captures the extra uncertainty. The result is right; the necessity still feels more algebraic than inevitable.

---

## Connections forward

Chapter 9 turns the logic around. Instead of building an interval around a sample statistic and asking "where is the parameter?", you'll fix a hypothesized parameter value and ask "how consistent is this sample with that hypothesis?" The sampling distributions are the same; the question is different. The margin of error you computed in this chapter becomes the standard for measuring how surprising your data would be if the null hypothesis were true.

Chapter 10 extends confidence intervals to two samples — estimating the difference between two population means or two proportions. The structure is identical: point estimate $\pm$ margin of error. The point estimate is now a difference, and the standard error combines variability from both samples.

In applied work — clinical trials, public opinion polling, quality control — confidence intervals are the working language of uncertainty. The width of the interval tells you how much precision you bought with your sample size. The level tells you how often you'll be right when you build intervals this way. Together, they let you make decisions under uncertainty without pretending the uncertainty isn't there.

---

## Exercises

### Warm-up

**8.1** *(CI for a mean, $\sigma$ known.)* A sample of 64 college students reports a mean weekly study time of $\bar{x} = 14.5$ hours. The population standard deviation is known to be $\sigma = 4$ hours. Build a 95% confidence interval for the true mean weekly study time. Interpret the result using the correct procedural language — not "there is a 95% chance."

**8.2** *(Reading critical values.)* For each confidence level, state the z* critical value and explain in one sentence what it means geometrically on the standard normal curve: (a) 90%, (b) 95%, (c) 99%. Why does the critical value increase as the confidence level increases?

**8.3** *(CI for a proportion.)* In a random sample of 300 registered voters, 162 say they plan to vote in the next election. Check the conditions for normal approximation. Build a 95% confidence interval for the true proportion of registered voters who plan to vote. Interpret the result.

### Application

**8.4** *(t-distribution and degrees of freedom.)* A nutrition researcher measures the daily caloric intake of 15 adults: $\bar{x} = 2,180$ calories, $s = 310$ calories. Build a 90% confidence interval for the true mean daily caloric intake. (a) What is the degrees of freedom? (b) What t* value do you use? (c) How would the interval change if the sample size were 50 instead of 15, everything else equal?

**8.5** *(z vs. t comparison.)* Using the data from Exercise 8.4 ($n = 15$, $\bar{x} = 2,180$, $s = 310$), compute both a z-based and a t-based 90% confidence interval, treating $s$ as if it were $\sigma$ for the z calculation. Compare the two intervals. Which is wider? Why is the wider one the honest choice?

**8.6** *(Sample size planning.)* A market researcher wants to estimate the proportion of households that subscribe to a streaming service. She wants a margin of error of $\pm 4\%$ at 95% confidence. (a) With no prior estimate of the proportion, how many households must she survey? (b) If a previous study found $\hat{p} = 0.35$, how many are needed? (c) Why does using a prior estimate reduce the required sample size compared to the worst-case assumption?

### Synthesis

**8.7** *(Interpreting a published margin of error.)* A news article reports: "A new poll finds 58% of Americans approve of the Supreme Court, with a margin of error of ±3.1 percentage points at 95% confidence." (a) Construct the confidence interval. (b) Is it correct to say "there is a 95% probability the true approval rate is between 54.9% and 61.1%"? Explain the distinction. (c) If the true approval rate is 55%, did this poll's interval hit or miss? How would you know?

**8.8** *(Two confidence levels, same data.)* A clinical trial of 25 patients shows a mean reduction in LDL cholesterol of 18 mg/dL with $s = 12$ mg/dL. Build both a 90% and a 99% confidence interval. (a) Which is wider, and by how much? (b) A regulator says: "The 90% interval looks better — it's narrower and still suggests the drug works." Explain the trade-off the regulator is accepting. (c) If the drug will be prescribed to millions of patients, which interval level gives you more comfort, and why?

**8.9** *(The cost of precision.)* A polling firm currently surveys 1,000 people and achieves a margin of error of $\pm 3.1\%$ at 95% confidence. Their client wants a margin of error of $\pm 1\%$. (a) How many people would the firm need to survey to achieve this? (b) If each survey respondent costs \$12 to contact and interview, what is the additional cost of the tighter margin? (c) The client says "just cut the margin in half, not all the way to 1%." How many people are needed for $\pm 1.55\%$, and what does this cost?

### Challenge

**10** *(Procedural honesty.)* A pharmaceutical company builds a 95% confidence interval for the effectiveness of a new drug and reports the interval $(2.1, 8.4)$ mg/dL reduction. A skeptic says: "Given how pharmaceutical trials work — selective reporting, publication bias, small $n$ — I don't believe the claimed 95% coverage is real." Identify two specific ways the coverage guarantee could be systematically violated in clinical research. For each, explain which assumption of the confidence interval procedure is being broken and what direction the bias would push the true coverage rate.

---

## LLM Exercise — Chapter 8: Confidence Intervals (Analyze One Dataset Project)

**Project:** Analyze One Real Dataset.  
**What you're building this chapter:** CIs for mean and proportion on key variables.  
**Tool:** **Claude Code** + **Claude Project**.

---

**The Prompt:**

```
Chapter 8 of my Analyze-One-Dataset project. Chapter 8 covered:
CI for one mean using t-distribution when σ is unknown (CI = x̄ ±
t*·s/√n; t* depends on n-1 degrees of freedom and the confidence
level); CI for one proportion (CI = p̂ ± z*·√(p̂(1-p̂)/n));
interpretation (95% CI: if we repeated the sampling many times,
95% of the resulting CIs would contain the true parameter —
NOT "95% probability the true parameter is in this interval");
sample-size formulas to achieve a target margin of error.

Write the brief's "Confidence Intervals" section in 500-800 words.

1. **CI for a mean.** Pick a quantitative variable. Compute:
   - Sample mean x̄.
   - Sample SD s.
   - Sample size n.
   - For 95% CI: t* with n-1 df (use scipy.stats.t.ppf(0.975,
     df=n-1)).
   - Margin of error = t* · s / √n.
   - Lower bound = x̄ − ME; Upper bound = x̄ + ME.
   - Interpret: "We are 95% confident that the true population
     mean of [variable] lies between [lower] and [upper]."
   - Also compute the 90% and 99% CIs for comparison.

2. **CI for a proportion.** Pick a categorical variable's specific
   level (e.g., proportion of items that meet a particular
   criterion). Compute:
   - Sample proportion p̂.
   - Standard error = √(p̂(1-p̂)/n).
   - For 95% CI: z* = 1.96.
   - Margin of error = z* · SE.
   - Lower and upper bounds.
   - Check the rule of thumb (np̂ ≥ 10 AND n(1-p̂) ≥ 10) for
     normal approximation validity.

3. **Sample-size calculation.** If you wanted a 95% CI for the
   proportion with margin of error ≤ 0.02 (2 percentage points),
   how large would n need to be? Use n = (z*/ME)² · p̂(1-p̂).
   - If your current n is smaller than required, note that and
     interpret accordingly.
   - If your current n is much larger than needed, your study
     could have been smaller.

4. **The interpretation discipline.** The common mistake is
   saying "there's a 95% probability the parameter is in the
   interval." That's wrong. The parameter is fixed; the interval
   is random. State the correct interpretation explicitly.

End with the practical conclusion: given the CIs, what can you
say about [your big question from Ch 1] with calibrated
confidence?
```

---

**What this produces:** A 500-800 word section with 2-3 confidence intervals, sample-size calculation, and a calibrated interpretation. The "what we can say with confidence" framing is the discipline this chapter trains.

**How to adapt this prompt:**

- *For your own project:* Pick variables whose CIs are operationally meaningful — the interval's width tells you how precisely you've measured the parameter.
- *For ChatGPT / Gemini:* Works as written.
- *For Claude Code:* `scipy.stats.t.interval(0.95, df, loc=mean, scale=SE)` is the one-liner; building from scratch with formulas is the pedagogically valuable version.
- *For a Claude Project:* Append.

**Connection to previous chapters:** Ch 7's standard error feeds directly into Ch 8's margin of error.

**Preview of next chapter:** Chapter 9 covers hypothesis testing with one sample. You'll test one specific claim about your dataset (e.g., "the mean is X" or "the proportion is Y").

---

##  AI Wayback Machine

**Run this:**

```
Who is Jerzy Neyman, and how does their work connect to confidence intervals we covered in this chapter? Keep it to three paragraphs. End with the single most surprising thing about their career or ideas.
```

→ Search **"Jerzy Neyman"** on Wikipedia.

**Now make the prompt better.** Try one of these:

- Ask it to apply Jerzy Neyman's framework to a small dataset you actually have.
- Add a constraint: "Answer including criticisms or limits of Jerzy Neyman's framework."

What changes? What gets better? What gets worse?
