# Chapter 6 — The Normal Distribution
*The bell that rings everywhere.*

---

In 1875, Francis Galton was plotting dots on graph paper.

Each dot represented a father and a son. The horizontal position was the father's height; the vertical position was the son's. He was trying to understand heredity — whether tall fathers produced tall sons, whether short fathers produced short sons. What he found was more interesting than what he was looking for.

The cloud of dots had a shape. It wasn't random scatter. It swelled in the middle and thinned toward the edges, symmetrically, as if someone had designed it. And Galton recognized the shape. He had seen it before — in chest measurements of Scottish soldiers, in the diameters of acorns from the same oak tree, in the errors that artillery officers made trying to hit a target. The same curve, appearing again and again in completely unrelated phenomena.

He began to suspect he was seeing something fundamental. Not a statistical artifact. Not a coincidence. Something deep — a shape that emerges whenever many small, independent causes add up to produce a single measurable outcome.

He was right. That shape is the normal distribution. This chapter teaches you to read it.

![Galton's dots. The cloud has a shape — and that shape is the bell.](images/06-the-normal-distribution-fig-01.png)
*Figure 6.1 — Recreation of Galton's 1875 bivariate scatter plot *

---

## Why the bell curve appears

There's a thought experiment that makes the normal distribution feel inevitable rather than mysterious.

Imagine flipping 100 fair coins and counting the number of heads. You could get anywhere from 0 to 100. But most of the time, you'll get somewhere around 50. Values far from 50 — say, 10 heads or 90 heads — are extremely rare. Values near 50 are common.

Now plot the probability of each possible outcome. What shape do you get? A bell. Peaked at 50, tapering symmetrically toward both extremes.

Why? Because getting exactly 50 heads requires that about half your coins land heads, and there are an enormous number of ways for that to happen — more ways than there are for exactly 10 or exactly 90. The count near the middle is reachable by many different arrangements of coins; the count near the extremes is reachable by very few.

Now extend the logic. A person's height is determined by hundreds of genes, each contributing a small increment up or down, plus nutrition, plus a dozen environmental factors. If each factor contributes roughly independently and roughly equally, the total is like flipping many coins. Most people land near the middle. Very tall people and very short people are rare.

This is why the normal distribution appears whenever a quantity is the sum of many small independent effects. The causes don't need to be identical to each other. They just need to be numerous and independent. The central limit theorem (which you'll meet in Chapter 7) makes this precise. For now, trust the intuition: sums of many independent small things converge to a bell shape. That's not a coincidence. It's a theorem.

![Bar chart of the binomial distribution for n=100](images/06-the-normal-distribution-fig-02.png)
*Figure 6.2 — Bar chart of the binomial distribution for n=100*

---

## The shape and its two parameters

The formula for the normal distribution is:

$$f(x) = \frac{1}{\sigma \sqrt{2\pi}} \cdot e^{-\frac{1}{2}\left(\frac{x-\mu}{\sigma}\right)^2}$$

Don't memorize this. The point isn't to work with the formula — it's to understand what it says.

The formula depends on exactly two numbers: $\mu$ (pronounced "mu") and $\sigma$ (pronounced "sigma"). Every bell-shaped distribution is completely determined by these two parameters.

$\mu$ is the mean — the center of the distribution, the value at the peak of the bell. The entire curve is symmetric about $\mu$.

$\sigma$ is the standard deviation — the measure of spread. A small $\sigma$ produces a tall, narrow bell. A large $\sigma$ produces a short, wide bell. But no matter the size of $\sigma$, the shape is always the same: it's always the same proportions, the same taper, the same family of curves.

We write $X \sim N(\mu, \sigma)$ to say that a quantity $X$ follows a normal distribution with mean $\mu$ and standard deviation $\sigma$. The tilde means "is distributed as."

![Same mean, different spread. The shape is always the bell — only the width changes.](images/06-the-normal-distribution-fig-03.png)
*Figure 6.3 — Three normal curves drawn on the same axes,*

Three facts about the normal curve that are worth having in your head:

The mean, median, and mode are all the same number — they all sit at the peak, because the curve is symmetric.

The curve never quite touches zero. It has infinite tails. The probability of values arbitrarily far from the mean is not zero; it's just very, very small.

The total area under the curve equals 1. This is the probability interpretation: if you want to know the probability that $X$ falls in some range, you find the area under the curve over that range.

---

## The empirical rule: 68, 95, 99.7

One of the most useful facts in statistics is so simple it sounds like a cheat.

For any normal distribution:

- About **68%** of the data fall within one standard deviation of the mean — between $\mu - \sigma$ and $\mu + \sigma$.
- About **95%** of the data fall within two standard deviations of the mean.
- About **99.7%** of the data fall within three standard deviations of the mean.

This is called the empirical rule, or sometimes the 68-95-99.7 rule.

These three numbers are worth memorizing because they let you estimate answers instantly. IQ scores are normally distributed with mean 100 and standard deviation 15. Without calculating anything: about 68% of people have IQs between 85 and 115. About 95% have IQs between 70 and 130. About 99.7% have IQs between 55 and 145. The rule tells you that an IQ above 145 is genuinely rare — it falls in the outer 0.3%, the tail beyond three standard deviations.

Notice the rule also gives you the tails. If 95% of data fall within two standard deviations, then 5% fall outside — split symmetrically, about 2.5% in each tail. If 99.7% fall within three, then 0.3% are more extreme, about 0.15% in each tail.

These tail probabilities matter enormously when you get to hypothesis testing. A result that is more than two standard deviations from what you expected is rare by chance. A result more than three standard deviations away is very rare. That's the intuition behind statistical significance, and it all flows from these three numbers.

![The 68-95-99.7 rule. Three numbers to memorize. The tails are already included.](images/06-the-normal-distribution-fig-04.png)
*Figure 6.4 — Standard normal bell curve with three nested shaded*

---

## Z-scores: the universal translator

Here is a problem. An applicant to your university scored 1200 on the SAT and another scored 28 on the ACT. These tests are on completely different scales. How do you compare them?

The answer is the z-score.

A z-score tells you how many standard deviations a value is away from its mean. The formula is:

$$z = \frac{x - \mu}{\sigma}$$

where $x$ is the raw value, $\mu$ is the mean of the distribution, and $\sigma$ is the standard deviation.

Suppose the SAT has mean 1050 and standard deviation 200. The applicant who scored 1200:

$$z = \frac{1200 - 1050}{200} = \frac{150}{200} = 0.75$$

This applicant is 0.75 standard deviations above the mean.

Suppose the ACT has mean 21 and standard deviation 5. The applicant who scored 28:

$$z = \frac{28 - 21}{5} = \frac{7}{5} = 1.40$$

This applicant is 1.4 standard deviations above the mean. The ACT score of 28 is more impressive than the SAT score of 1200, even though 1200 is a bigger number. The z-score strips away the arbitrary scale and tells you the only thing that matters: where you sit in the distribution.

The z-score has a concrete interpretation:
- $z = 0$ means you're at the mean.
- $z = 1$ means you're one standard deviation above the mean — about the 84th percentile.
- $z = -1$ means you're one standard deviation below — about the 16th percentile.
- $z = 2$ means you're two standard deviations above — about the 97.5th percentile.
- $z = -2$ means you're two below — about the 2.5th percentile.

These percentile numbers come directly from the empirical rule. If 68% of the population falls between $z = -1$ and $z = +1$, then 16% fall below $z = -1$ (by symmetry), and 84% fall below $z = +1$. The z-score and the percentile rank are the same information expressed differently.

### The standard normal distribution

When you convert a value to a z-score, you're translating it into a distribution with mean 0 and standard deviation 1. This is called the **standard normal distribution**, written $N(0, 1)$.

Every normal distribution, regardless of its original mean and standard deviation, becomes the same standard normal distribution once you convert to z-scores. This is why z-scores are so powerful: they provide a universal scale. All normal distributions are the same shape. Standardization reveals that universal shape.

![Z-scores put incomparable scales on a common ruler.](images/06-the-normal-distribution-fig-05.png)
*Figure 6.5 — Two-step transformation diagram*

---

## Reading the probability from the curve

The empirical rule gives you three checkpoints: 68%, 95%, 99.7%. But what if you need a more precise answer? What's the probability of a value between $z = 0$ and $z = 1.25$?

For this you use the standard normal table, also called the z-table.

The standard normal table lists, for each z-score, the area under the standard normal curve between 0 and that z-score. This area is a probability — the probability that a randomly selected value from a standard normal population falls between the mean and the z-score.

For example, the table entry for $z = 1.25$ is 0.3944. The probability of a value between the mean and 1.25 standard deviations above the mean is 0.3944.

A few reference points worth knowing:
- $z = 1.00$: area between 0 and 1 is 0.3413. Combined with the 0.5 below the mean, the area below $z = 1$ is 0.8413. This is the 84th percentile.
- $z = 1.96$: area below this z-score is very close to 0.975. This number — 1.96 — will come back constantly in Chapters 8 and 9.
- $z = 2.576$: area below this is 0.995. Used for 99% confidence intervals.

Because the curve is symmetric, the table for positive z-scores is all you need. The area below $z = -1.25$ equals the area above $z = 1.25$, which is $0.5 - 0.3944 = 0.1056$.

### Three types of probability questions

Every probability question about a normal distribution reduces to one of three types.

**Below a value:** Convert to a z-score. If positive, add 0.5 to the table value (since 0.5 of the area is to the left of the mean). If negative, subtract the table value from 0.5.

**Above a value:** Find the probability below it, then subtract from 1.

**Between two values:** Convert both to z-scores. Find the area between each and the mean. Add if they're on opposite sides of the mean; subtract if they're on the same side.

A worked example. Heights of adult men are normally distributed with mean 70 inches and standard deviation 2.5 inches. What's the probability a randomly selected man is taller than 72 inches?

Convert: $z = (72 - 70)/2.5 = 0.80$

Table: area between 0 and 0.80 is 0.2881.

Since we want *above* 72, and the area above the mean is 0.5: $0.5 - 0.2881 = 0.2119$.

About 21% of men are taller than 72 inches.

Another example: what fraction of men are between 67 and 73 inches?

$z_1 = (67 - 70)/2.5 = -1.20$; table value for 1.20 is 0.3849.
$z_2 = (73 - 70)/2.5 = 1.20$; table value is also 0.3849 (symmetric).

Since they're on opposite sides of the mean, add: $0.3849 + 0.3849 = 0.7698$.

About 77% of men fall in this range. Notice this is close to, but not exactly, the empirical rule's 68% for one standard deviation. The interval here is $\pm 1.2$ standard deviations, which is a bit wider than $\pm 1$.

![Three shapes, three moves. Every normal probability question is one of these.](images/06-the-normal-distribution-fig-06.png)
*Figure 6.6 — Three side-by-side standard normal curves illustrating the three*

---

## When the normal distribution doesn't fit

Not all data are normally distributed, and applying the normal distribution blindly to data that aren't is a real error with real consequences.

Heights in a population: yes, approximately normal.

Income: no. Incomes are right-skewed — a few extremely high earners pull the tail far to the right. The mean income is substantially higher than the median. Applying the normal distribution would badly underestimate the fraction of people with very high incomes.

Time between events, like customer arrivals or equipment failures: no. These follow exponential distributions, which are entirely one-sided — time can't be negative.

Exam scores in a class that's too easy: no. Everyone clusters near the top. The distribution is left-skewed, with a hard ceiling at 100.

The diagnostic is simple: plot the data. If the histogram looks roughly symmetric and bell-shaped, the normal distribution is a reasonable starting point. If it's skewed, or has two peaks, or has a hard floor or ceiling, look for a different model.

The most important violation to recognize is heavy tails. A normal distribution assigns very small probabilities to extreme values. If your data has more extreme values than the normal distribution predicts — which is common in financial returns, earthquake magnitudes, and network traffic — the normal distribution will underestimate the probability of rare, large events. This is not a minor problem. It's the kind of modeling error that produces financial crises.

![The tails look the same until they don't. Heavy-tailed data produces extreme events the normal distribution says are almost impossible.](images/06-the-normal-distribution-fig-07.png)
*Figure 6.7 — Overlapping density curves *

---

## The normal distribution and what comes next

This chapter is infrastructure. The normal distribution is the foundation on which the rest of the course is built.

In Chapter 7, you'll meet the central limit theorem. It says: take a large enough sample from any population — it doesn't matter what shape that population has — and compute the sample mean. Do this many times and plot those means. They form a normal distribution. This is astonishing. It means that even if you're sampling from a skewed population, or a bimodal population, or a population with heavy tails, the distribution of sample means is still approximately normal. The spread of that normal distribution shrinks as your sample size grows — larger samples give you a tighter estimate of the true mean.

This fact is what makes statistical inference possible. You don't need to know the shape of the population. You just need to know the distribution of sample means, and the central limit theorem tells you what that is.

The three-step pattern you've learned here — standardize, look up, interpret — is the template you'll use repeatedly:
1. Compute a z-score (or t-score, or chi-square value — these are all variants of the same idea).
2. Find the corresponding probability from a table or software.
3. Ask: is this probability small enough to be surprising?

That third step is hypothesis testing. But you need this chapter to get there.

The normal distribution is not a description of any particular dataset. It is a mathematical model — an idealization. Real data are never perfectly normal. The question is never "is this exactly normal?" It's "is this close enough to normal that the normal distribution gives me useful answers?" Most of the time, for well-measured continuous quantities in large populations, the answer is yes.

Galton didn't set out to discover the grammar of statistics. He was looking at dots on paper, trying to understand whether tall fathers had tall sons. He found something larger: that the shape of variation itself has a form, and that form is the bell. A hundred and fifty years later, every time you construct a confidence interval or run a t-test, you are using the shape that Galton drew on his graph paper.

---

## Exercises

### Warm-up

**6.1** *(Empirical rule — direct application.)* SAT scores are normally distributed with mean 1050 and standard deviation 200. Using only the 68-95-99.7 rule, answer without a calculator: (a) What percentage of test-takers score between 850 and 1250? (b) What percentage score above 1450? (c) Between what two scores do the middle 99.7% of test-takers fall?

**6.2** *(Computing and interpreting a z-score.)* A runner finishes a 10K race in 52 minutes. The distribution of finishing times for her age group is normal with mean 58 minutes and standard deviation 7 minutes. (a) Compute her z-score. (b) Interpret it in plain language. (c) Using the empirical rule, estimate her approximate percentile rank.

**6.3** *(Identifying when the normal distribution fits.)* For each of the following, state whether the normal distribution is likely a reasonable model. Briefly explain why or why not. (a) The annual salaries of all employees at a large corporation. (b) The weights of apples from a single orchard. (c) The number of customers who enter a store each hour.

### Application

**6.4** *(Standard normal table — below and above.)* Using the standard normal table: (a) Find $P(Z < 1.65)$. (b) Find $P(Z > 0.90)$. (c) Find $P(Z < -1.30)$.

**6.5** *(Between two values.)* A manufacturer fills juice bottles. Fill volumes are normally distributed with mean 16.2 oz and standard deviation 0.3 oz. Quality control accepts bottles between 15.8 and 16.8 oz. What fraction of bottles pass inspection?

**6.6** *(Comparing across populations.)* A student scores 74 on a chemistry exam (mean 68, SD 8) and 81 on a physics exam (mean 78, SD 4). On which exam did the student perform better relative to classmates? Show your z-score calculations and explain the answer.

### Synthesis

**6.7** *(Full three-step process.)* Adult women's resting heart rates are approximately normally distributed with mean 74 beats per minute (bpm) and standard deviation 8 bpm. (a) What fraction of women have resting heart rates below 60 bpm? (b) A physician flags patients whose resting heart rate is above 90 bpm for follow-up. What fraction of women will be flagged? (c) What fraction have heart rates between 66 and 82 bpm?

**6.8** *(Reverse: find the value from the probability.)* On a standardized exam with mean 500 and standard deviation 100, a scholarship requires scoring in the top 10% of test-takers. What is the minimum score required? (Hint: find the z-score that puts 90% of the area below it, then solve $x = \mu + z \cdot \sigma$.)

### Challenge

**6.9** *(Heavy tails and model failure.)* A risk analyst models daily stock returns as $N(0.05\%, 1.2\%)$. (a) According to this model, what is the probability of a single day's return falling below $-4\%$? (b) If the stock market has trading days about 252 days per year, how many such days would the model predict per century? (c) In reality, one-day drops of 4% or more happen roughly several times per decade for major indices. What does this tell you about the normal distribution as a model for daily returns?

**6.10** *(Design reasoning.)* A hospital sets a drug dosage at 200 mg, the mean required dose for patients in their population. The standard deviation of required doses is 30 mg. The drug is safe if the actual dose is within 50 mg of what a patient needs — that is, if the absolute difference between administered and required dose is at most 50 mg. (a) What fraction of patients receive a safe dose? (b) If the hospital could tighten dosing precision — reducing the standard deviation to 20 mg — what fraction would now be safe? (c) What does this calculation reveal about the relationship between precision and safety in medical dosing?

---

## LLM Exercise — Chapter 6: The Normal Distribution (Analyze One Dataset Project)

**Project:** Analyze One Real Dataset.
**What you're building this chapter:** normality assessment of key variables + z-score interpretation.
**Tool:** **Claude Code** + **Claude Project**.

---

**The Prompt:**

```
Chapter 6 of my Analyze-One-Dataset project. Chapter 6 covered the
Normal distribution N(μ, σ²); the standard Normal Z = (X − μ)/σ;
the empirical rule (68%/95%/99.7% within ±1, ±2, ±3 SDs); using
tables or software to find P(Z < z), P(Z > z), P(a < Z < b);
inverse normal: given a probability, find the corresponding x.

Write the brief's "Normality Analysis" section in 400-600 words.

1. **Pick a quantitative variable that you suspect is approximately
   Normal.** Often: heights, IQ scores, lab measurements, mean
   reaction times. Sometimes: test scores, daily returns on liquid
   markets (though see Ch 5 caveat about fat tails).

2. **Visualize.** Histogram + overlay of theoretical Normal with
   the sample mean and SD. Note where the empirical density
   matches and where it deviates.

3. **Q-Q plot.** Plot the variable's quantiles against the
   theoretical Normal quantiles. If approximately Normal, the
   points form a straight line. Deviations indicate:
   - S-shape: heavier or lighter tails than Normal.
   - Curved: skewness.
   - Steps: discretization (not continuous).

4. **Formal normality test.** Run Shapiro-Wilk (`scipy.stats.
   shapiro`). Note: with n > 5000, normality tests reject the
   null even for slight deviations (very high power). Use the
   visual + test together; the visual is more informative.

5. **Z-score computation.** Pick three values from your data:
   the maximum, the minimum, and one moderate point. Compute
   z for each. Look up the corresponding probabilities.
   - For the maximum: P(X > max in a Normal distribution with
     your sample mean and SD).
   - For the minimum: P(X < min similarly).
   The probabilities should be tiny (well below 1/n) if these
   are genuine extremes of a Normal distribution. If P(X > max)
   is, say, 0.10, your data has 10× more such extremes than a
   Normal predicts — your variable isn't Normal.

6. **Empirical rule check.** Compare empirical to theoretical:
   - Theoretical: 68%/95%/99.7% within ±1, ±2, ±3 SDs.
   - Empirical: count what fraction of data actually falls in
     each range.

End with the verdict: is your variable approximately Normal?
What kind of deviation (heavier tails? skew?) does it show?
This shapes which inference methods will be appropriate later.
```

---

**What this produces:** A 400-600 word section with Q-Q plot, empirical-vs-theoretical comparison, and a defensible normality assessment.

**How to adapt this prompt:**

- *For your own project:* If NO variable in your dataset is approximately Normal, you'll rely on CLT (Ch 7) for inference. Note that here.
- *For ChatGPT / Gemini:* Works as written.
- *For Claude Code:* `scipy.stats.shapiro`, `scipy.stats.probplot` for Q-Q.
- *For a Claude Project:* Append.

**Connection to previous chapters:** Ch 2's empirical-rule check, Ch 5's fitting work, and Ch 6's formal normality assessment build a layered evaluation.

**Preview of next chapter:** Chapter 7 covers the CLT. You'll simulate or bootstrap sampling distributions of the mean from your data and verify (or refute) the CLT's promise.

---

## AI Wayback Machine

**Carl Friedrich Gauss** was a mathematician whose 1809 work on least squares put the normal distribution at the center of statistical practice.

**Run this:**

```
Who is Carl Friedrich Gauss, and how does their work connect to the normal distribution we covered in this chapter? Keep it to three paragraphs. End with the single most surprising thing about their career or ideas.
```

→ Search **"Carl Friedrich Gauss"** on Wikipedia.

**Now make the prompt better.** Try one of these:

- Ask it to apply Carl Friedrich Gauss's framework to a small dataset you actually have.
- Add a constraint: "Answer including criticisms or limits of Carl Friedrich Gauss's framework."

What changes? What gets better? What gets worse?
