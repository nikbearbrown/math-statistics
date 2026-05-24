# Chapter 2 — Descriptive Statistics
*What the numbers are actually telling you.*

---

Here is a fact that should bother you: the same dataset can produce two completely honest, completely contradictory summaries.

A recruiter tells you the engineers at her company earn an average of $95,000 a year. She's not lying. The mean really is $95,000. But when you look at the spreadsheet, you find one principal engineer earning $780,000 and forty-two junior engineers earning between $48,000 and $62,000. The median — the value right in the middle of the ordered list — is $51,500. Both numbers are correct. They describe the same forty-three people. And yet one of them is honest and one of them is, in the relevant sense, a lie.

That's what descriptive statistics is about. Not computation — computing the mean of a list is arithmetic, and arithmetic is not statistics. Statistics is knowing *which* number to compute, *why* that number answers your question, and *what happens* to your answer when the data aren't well-behaved. Everything in this chapter is a consequence of that one problem.

---

## First, look at the thing

Before you calculate anything, you should look at the data. I mean actually look — draw a picture of it. This seems obvious, but it is the step that gets skipped most often, and skipping it is the source of a remarkable number of embarrassments.

The tool for looking is the **histogram**. Take your data — two thousand salaries, or fifty test scores, or thirteen house prices — and divide the range of values into adjacent intervals of equal width. Count how many observations fall in each interval. Draw a bar for each interval, height equal to the count. The bars should touch, because the underlying variable is continuous.

Now you can see things you couldn't see from the numbers alone. Are the data piled up in one place, or spread across the whole range? Is there a long tail on one side? Are there two separate humps — suggesting your "population" is actually two different groups? Is there one bar sitting far away from all the others?

![Histogram showing four archetypal shapes side by side](images/02-descriptive-statistics-fig-01.png)
*Figure 2.1 — Histogram showing four archetypal shapes side by side*

That last thing — one bar sitting far away — is an outlier, and it is the source of all your trouble with the mean. More on that shortly.

For a smaller dataset, say twenty or thirty values, there is a more informative version of the histogram called the **stem-and-leaf plot**. Each number is split into a "stem" (all digits except the last) and a "leaf" (the last digit). Line up the stems vertically, and write each leaf next to its stem. The result is a histogram that preserves the actual values. Twenty exam scores — 49, 53, 55, 55, 61, 63, 67, 68, 68, 69, 69, 72, 73, 74, 78, 80, 83, 88, 88, 88 — arranged this way, you see immediately that the scores cluster in the 60s and 70s, with three students stuck in 88 and one unlucky person at 49.

![Stem-and-leaf plot of the twenty exam scores above,](images/02-descriptive-statistics-fig-02.png)
*Figure 2.2 — Stem-and-leaf plot of the twenty exam scores above,*

The third picture is the **box plot**, which is really a picture of five numbers: the minimum, the 25th percentile (called Q1), the median, the 75th percentile (Q3), and the maximum. Draw a box from Q1 to Q3. Put a line inside the box at the median. Extend lines — "whiskers" — from the box out to the min and max. Any values that sit suspiciously far from the box get plotted as individual points.

That "suspiciously far" has a precise meaning: a value is flagged as a suspected outlier if it lies more than $1.5 \times (Q3 - Q1)$ below Q1 or above Q3. The quantity $Q3 - Q1$ is called the **interquartile range**, or IQR, and it measures the spread of the middle 50% of your data. The IQR is a crucial idea — we'll come back to it when we measure spread properly.

The box plot is what you reach for when you want to compare two groups at a glance, or when you have too many observations for a stem-and-leaf plot to stay readable.

![Labeled box plot diagram showing a single distribution](images/02-descriptive-statistics-fig-03.png)
*Figure 2.3 — Labeled box plot diagram showing a single distribution*

---

## Three ways to find the center

Now we can talk about measuring things. The first thing anyone wants to know about a dataset is: what's typical? What's the middle? This turns out to have three different answers, and choosing the wrong one is a very common mistake.

### The mean

The **mean** (also called the arithmetic mean, or just "the average") is the sum of all values divided by the count of values. In symbols:

$$\overline{x} = \frac{\sum_{i=1}^{n} x_i}{n}$$

The $\overline{x}$ (read "x-bar") is the sample mean. If you're describing a whole population rather than a sample from it, the symbol changes to $\mu$ (the Greek letter mu), and $n$ becomes $N$. The formula is otherwise identical.

The physical intuition for the mean is a balance point. Imagine placing weights on a number line, one weight per observation. The mean is the fulcrum location that keeps the whole thing level. This intuition immediately tells you something important: a single heavy weight placed far from the others will drag the fulcrum toward it. An outlier pulls the mean.

![Number line with five evenly spaced weights (2,](images/02-descriptive-statistics-fig-04.png)
*Figure 2.4 — Number line with five evenly spaced weights (2,*

You can also compute the mean when some values repeat, by multiplying each distinct value by how many times it appears:

$$\overline{x} = \frac{\sum f_i \cdot x_i}{n}$$

where $f_i$ is the frequency of value $x_i$.

### The median

The **median** is the middle value of an ordered list. Arrange your observations from smallest to largest. If $n$ is odd, the median is the value at position $\frac{n+1}{2}$. If $n$ is even, there is no single middle value — the median is the average of the two values at positions $\frac{n}{2}$ and $\frac{n}{2} + 1$.

The median is not pulled by outliers. It is determined by position, not by magnitude. If you add a billionaire to a room of middle-class people, the median income of the room barely moves; the mean income rockets. This is the key difference.

Here is the question that tells you which one to use: *Is there an outlier, or is the distribution strongly skewed?* If yes — use the median. If the distribution is roughly symmetric with no extreme values — mean and median will be close to each other, and either works.

A small town with 50 residents: 49 earn $30,000 a year; one earns $5,000,000. The mean is $129,400. The median is $30,000. One of these answers tells you what life is like for most people in that town. The other answers a question nobody was asking.

### The mode

The **mode** is the most frequently occurring value. A dataset can have one mode (unimodal), two (bimodal), or more. The mode is the only measure of center that applies to categorical data — if you sell T-shirts in six colors, you can't average the colors, but you can find the most popular one.

For numerical data, the mode is most useful when there are clear, discrete peaks. If an exam produces a bimodal distribution — many students clustered near 55, many others clustered near 85 — the mode reveals that your class contains two distinct groups (perhaps students who did and didn't attend review sessions). The mean and median would give you something around 70, which doesn't describe either group.

### How shape distorts center

When a distribution is **symmetric**, the mean, median, and mode are all approximately equal. There's no distortion to worry about.

When a distribution is **right-skewed** — it has a long tail toward high values — the mean gets pulled right, past the median, which is past the mode. The order is: mode < median < mean.

When a distribution is **left-skewed** — long tail toward low values — the pulling goes the other direction: mean < median < mode.

This is why the shape of the histogram matters before you compute anything. If the histogram shows a right-skewed distribution, you already know the mean is going to be inflated relative to the median, and you should be suspicious of any claim that reports only the mean.

![Three side-by-side distribution curves ](images/02-descriptive-statistics-fig-05.png)
*Figure 2.5 — Three side-by-side distribution curves *

---

## Measuring spread

Two datasets can have identical means and still be completely different. Consider two supermarkets, both with a mean customer wait time of five minutes. At Supermarket A, the actual wait times range from four minutes to six minutes. At Supermarket B, they range from one minute to eleven minutes. The average is the same; the experience is not.

Spread is the second dimension of any distribution. You need four tools to measure it properly.

### The range

The **range** is the maximum minus the minimum. It is easy to compute and instantly interpretable — "salaries at this company range from $35,000 to $180,000" is useful information. But it is determined entirely by the two most extreme values, which means one outlier can make it useless. Use the range as a first orientation, then follow up with something better.

### The interquartile range

The **IQR** is $Q3 - Q1$: the range of the middle 50% of the data. Because it ignores the bottom 25% and the top 25%, outliers don't affect it. The IQR is the spread measure to use when outliers are present, which is most of the time in real data.

And as noted earlier, the IQR is also used to define what counts as an outlier: a value is suspicious if it lies below $Q1 - 1.5 \times \text{IQR}$ or above $Q3 + 1.5 \times \text{IQR}$. This is not a law of nature — it's a convention that works well in practice. Values flagged by this rule should be *investigated*, not automatically discarded.

### Variance

The **variance** is the average squared distance from the mean. For a sample of $n$ observations:

$$s^2 = \frac{\sum_{i=1}^{n} (x_i - \overline{x})^2}{n - 1}$$

For a population of $N$:

$$\sigma^2 = \frac{\sum_{i=1}^{N} (x_i - \mu)^2}{N}$$

Why square the deviations? Because raw deviations $(x_i - \overline{x})$ always sum to zero — the positive and negative deviations cancel exactly, because $\overline{x}$ is the balance point. Squaring makes every deviation positive, so they accumulate rather than cancel.

Why divide by $n - 1$ for samples rather than $n$? This is Bessel's correction. When you compute a sample mean, you've already used the data to estimate $\mu$, which introduces a slight downward bias into the sum of squared deviations. Dividing by $n-1$ instead of $n$ corrects for this bias. (The full derivation belongs in a more advanced course; for now, the rule is: sample → divide by $n-1$, population → divide by $N$.)

The squaring also amplifies large deviations more than small ones. A value 10 units from the mean contributes 100 to the sum; a value 100 units away contributes 10,000. This means variance (and standard deviation) is sensitive to outliers — which is a reason to prefer IQR when outliers are present.

### Standard deviation

The **standard deviation** is simply the square root of variance:

$$s = \sqrt{s^2} \qquad \sigma = \sqrt{\sigma^2}$$

Why take the square root? Because variance is in *squared* units — if you're measuring heights in inches, variance is in square inches, which means nothing. Standard deviation brings you back to the original units. If incomes are measured in dollars, the standard deviation is also in dollars. That makes it interpretable: on average, values stray roughly $s$ units from the mean.

Let me show you the calculation for a small example, because seeing all the steps once is worth more than describing them abstractly.

Five observations: 2, 4, 6, 8, 10.

**Step 1.** Calculate the mean.

$$\overline{x} = \frac{2 + 4 + 6 + 8 + 10}{5} = \frac{30}{5} = 6$$

**Step 2.** Calculate each deviation and its square.

| $x_i$ | $x_i - \overline{x}$ | $(x_i - \overline{x})^2$ |
|---|---|---|
| 2 | −4 | 16 |
| 4 | −2 | 4 |
| 6 | 0 | 0 |
| 8 | 2 | 4 |
| 10 | 4 | 16 |

**Step 3.** Sum the squared deviations.

$$\sum (x_i - \overline{x})^2 = 16 + 4 + 0 + 4 + 16 = 40$$

**Step 4.** Divide by $n - 1$ for sample variance.

$$s^2 = \frac{40}{4} = 10$$

**Step 5.** Take the square root for standard deviation.

$$s = \sqrt{10} \approx 3.16$$

Interpretation: the typical observation in this dataset sits about 3.16 units from the mean of 6. The dataset is 2, 4, 6, 8, 10 — evenly spaced, perfectly symmetric — and a standard deviation of about 3 feels right.

Now compare two datasets with the same mean. Supermarket A: mean wait = 5 min, $s$ = 0.5 min. Supermarket B: mean wait = 5 min, $s$ = 2 min. At A, the wait is almost always between 4 and 6 minutes. At B, the wait can be anywhere from 1 to 9 minutes. Same average experience on paper. Very different actual experience.

![Two overlapping bell-curve-shaped distributions on the same axis](images/02-descriptive-statistics-fig-06.png)
*Figure 2.6 — Two overlapping bell-curve-shaped distributions on the same axis*

---

## Percentiles and quartiles

Every descriptive statistics course covers percentiles, and most students treat them as a bookkeeping task. They are actually a useful idea.

The **$p$-th percentile** is the value below which $p\%$ of the data falls. The 90th percentile of test scores is the score below which 90% of students scored. If you're at the 90th percentile, you did better than 90 out of every 100 test-takers.

To calculate the $k$-th percentile in an ordered dataset of $n$ values:

1. Compute the index $i = \frac{k}{100}(n + 1)$.
2. If $i$ is a whole number, the percentile is the value at position $i$.
3. If $i$ is not a whole number, the percentile is the average of the values at positions $\lfloor i \rfloor$ and $\lceil i \rceil$ (the two positions bracketing $i$).

Quartiles are the three percentiles that divide ordered data into four equal groups: Q1 (25th percentile), Q2 (50th percentile = median), Q3 (75th percentile). Together with the minimum and maximum, they form the **five-number summary**: (Min, Q1, Median, Q3, Max).

Here is an example. Twenty-nine test scores, ordered: 18, 21, 22, 25, 26, 27, 29, 30, 31, 33, 36, 37, 41, 42, 47, 52, 55, 57, 58, 62, 64, 67, 69, 71, 72, 73, 74, 76, 77.

Find the 70th percentile.

$$i = \frac{70}{100}(29 + 1) = 0.7 \times 30 = 21$$

Position 21 is a whole number, so the 70th percentile is the 21st value: **64**.

Find the 83rd percentile.

$$i = \frac{83}{100}(30) = 24.9$$

Not a whole number: average positions 24 and 25.

$$\text{83rd percentile} = \frac{71 + 72}{2} = 71.5$$

What this means: 83% of students scored 71.5 or below. If you scored 72, you were just above the 83rd percentile.

---

## Putting it together: a wealth management example

Abstract tools only become useful when you apply them to a concrete problem. Here is one.

A wealth management firm wants to describe three years of portfolio returns to prospective investors. They have data on 100 clients, with annual returns ranging from −15.3% to +28.4%.

The first step is the histogram. Group the returns into ten equal-width intervals (width ≈ 4.5 percentage points). The histogram shows a roughly bell-shaped distribution, slightly right-skewed — most clients earned between 2% and 11%, but there is a thin tail of exceptional performers on the high end, and a thin tail of bad years on the low end.

![Histogram of the 100 client portfolio returns ](images/02-descriptive-statistics-fig-07.png)
*Figure 2.7 — Histogram of the 100 client portfolio returns *

Now the summary statistics:

- **Mean return:** 6.2%
- **Median return:** 6.5%
- **Standard deviation:** 6.1%

The mean and median are close — 6.2% vs. 6.5% — confirming what the histogram suggested: the distribution is nearly symmetric. No extreme outlier is distorting the mean.

The five-number summary: Min = −15.3%, Q1 = 2.8%, Median = 6.5%, Q3 = 10.5%, Max = 28.4%.

IQR = $10.5 - 2.8 = 7.7$ percentage points.

Outlier check:
- Upper boundary: $10.5 + 1.5(7.7) = 22.05\%$
- The +28.4% return exceeds this boundary. It is a suspected outlier — likely an exceptional year for one particular client, not evidence that 28% returns are typical.

![Box plot of the 100 client returns ](images/02-descriptive-statistics-fig-08.png)
*Figure 2.8 — Box plot of the 100 client returns *

Now you can write an honest summary: "Our 100 clients achieved a mean annual return of 6.2% with a standard deviation of 6.1%. Half earned between 2.8% and 10.5%. Most returns clustered between 2% and 11%. One client achieved 28.4% — an exceptional result. Only six of 100 clients saw losses. The distribution is nearly symmetric, with no systematic bias in either direction."

This is what a complete descriptive analysis looks like. Not just one number, not just "the average was 6.2%." The mean, the spread, the shape, the outliers — together they give an honest picture.

---

## The shape tells you what to trust

I want to end with the connection between shape and the measures of center and spread, because it is the frame that holds everything else together.

When a distribution is symmetric: mean ≈ median. Standard deviation is the appropriate measure of spread. Report the mean and SD.

When a distribution is right-skewed (long tail toward high values): mean > median. The mean is pulled up by the tail; it overstates what is typical. Report the median and IQR.

When a distribution is left-skewed (long tail toward low values): mean < median. The mean is pulled down by the tail. Report the median and IQR.

When outliers are present: same principle. The mean and standard deviation are both sensitive to outliers. Prefer median and IQR.

The measures are not interchangeable. They answer different questions. Your job as a data analyst — not a data computer — is to look at the shape first, then choose the measure that answers the question honestly.

The salary recruiter gave you the mean because it made her company look better. You know enough now to ask for the median.

---

## What would change my mind

The $1.5 \times \text{IQR}$ rule for outliers is a useful convention, not a physical law. For certain types of data — particularly in fields like finance, where extreme events follow heavy-tailed distributions — this rule flags far too many "outliers," most of which are real. If I found a better rule that more accurately predicted which values were measurement errors versus genuine extremes in those contexts, I would revise my reliance on the 1.5× convention.

I also don't have a fully satisfying first-principles derivation for why dividing sample variance by $n-1$ rather than $n$ corrects the bias. The algebraic argument works out cleanly — but I want to understand it the way you understand *why* a gyroscope precesses rather than just knowing that it does.

---

## Connections forward

Chapter 3 introduces probability, and the first thing we'll do there is calculate how likely it is that a randomly selected value falls in some range. That calculation is only possible if we know the distribution — its shape, its center, its spread. The histogram you draw in Chapter 2 is the foundation.

In Chapter 6, the normal distribution gives precise meaning to the standard deviation: in a bell-shaped distribution, about 68% of values fall within one standard deviation of the mean, about 95% within two, and about 99.7% within three. The standard deviation transforms from a summary statistic into a forecasting tool.

In Chapter 9, hypothesis testing will ask: "If the null hypothesis were true, how unlikely is our observed sample mean?" The answer is computed using the standard deviation. Without it, you cannot do inference. Everything from Chapter 2 forward.

---

## Exercises

### Warm-up

**2.1** *(Reading a histogram.)* A histogram of 200 students' weekly study hours shows the tallest bar spanning 10–14 hours with a frequency of 52, and the next tallest spanning 14–18 hours with a frequency of 47. What percentage of students studied between 10 and 18 hours per week? What does a right-skewed shape in this histogram tell you about which center measure to report?

**2.2** *(Mean vs. median.)* A used-car lot has nine vehicles priced at $8,000, $8,500, $9,000, $9,200, $9,500, $10,000, $11,000, $12,000, and $35,000. Calculate the mean and median. Which better represents a typical vehicle at this lot, and why? What does the $35,000 vehicle do to the mean?

**2.3** *(Standard deviation intuition.)* Two coffee shops have the same mean daily customer count: 120. Shop A has a standard deviation of 8 customers; Shop B has a standard deviation of 40 customers. Without calculating anything, describe what the staffing implications are for each shop's manager.

### Application

**2.4** *(Five-number summary and outlier detection.)* Commute times in minutes for 15 employees: 12, 14, 18, 20, 22, 25, 27, 28, 30, 32, 35, 38, 40, 42, 55. Calculate Q1, the median, Q3, and IQR. Apply the $1.5 \times \text{IQR}$ rule. Is the 55-minute commute a suspected outlier? If you were a manager reviewing this dataset, what would you want to investigate about that value?

**2.5** *(Percentile calculation.)* A standardized exam has 40 scores, ordered from lowest to highest. You want to find the 65th percentile. Using the index formula $i = \frac{k}{100}(n+1)$, determine which position (or positions) to use. If the values at those positions are 71 and 73, what is the 65th percentile? Interpret the result in plain language.

**2.6** *(Standard deviation step-by-step.)* Six observations: 3, 7, 7, 9, 11, 15. Calculate the mean, then compute the sample standard deviation by building the deviation table. Show each step. After computing $s$, verify the sign makes sense by checking that the dataset spans roughly mean ± $s$ to mean ± $2s$.

### Synthesis

**2.7** *(Shape, center, and what to report.)* A hospital tracks emergency room wait times (in minutes) for 500 patients. The histogram is strongly right-skewed: most patients wait 15–40 minutes, but a few wait over 4 hours. The mean is 52 minutes; the median is 31 minutes. Explain why the mean overstates what a typical patient experiences. Which measure and which spread statistic would you report to a patient asking "how long will I wait?"

**2.8** *(Choosing the right tools across two groups.)* Employee satisfaction scores (1–10) at two firms: Firm A has mean = 7.2, SD = 0.7; Firm B has mean = 7.1, SD = 2.4. A journalist reports "both firms average around 7." What is missing from that summary? What does the difference in standard deviations actually mean for employees at each firm? Which firm would you rather work at if you are risk-averse about job satisfaction?

**2.9** *(Full descriptive analysis from grouped data.)* Annual salaries (in thousands of dollars) for 50 employees, given as a frequency table:

| Salary interval (K$) | Frequency |
|---|---|
| 30–40 | 6 |
| 40–50 | 14 |
| 50–60 | 18 |
| 60–70 | 9 |
| 70–80 | 3 |

Estimate the mean using midpoints. Describe the distribution's shape from the table alone. Would you expect the mean or median to be higher? Why?

### Challenge

**2.10** *(When the IQR rule misleads.)* A financial analyst applies the $1.5 \times \text{IQR}$ outlier rule to daily stock returns for a volatile technology company over five years and flags 87 values as "outliers." A colleague points out that heavy-tailed financial returns routinely produce large values that are not errors. Explain the tension between the two positions. What would you need to know about the return distribution to decide whether those 87 values should be investigated or accepted as real?

**2.11** *(Full analysis: books read per year.)* Thirty high-school students reported the number of books read in one year: 0, 2, 3, 5, 5, 6, 7, 8, 8, 9, 10, 11, 12, 12, 13, 14, 15, 16, 17, 18, 19, 20, 22, 24, 25, 28, 30, 35, 40, 50. (a) Calculate the mean and median. (b) Calculate Q1, Q3, and IQR, and identify any suspected outliers. (c) Is this distribution symmetric, right-skewed, or left-skewed? How do the mean and median support your answer? (d) Write two sentences a school administrator could use in a report — one that honestly represents typical reading, and one that a fundraiser might prefer to use. Identify which measure underlies each sentence.

---

## LLM Exercise — Chapter 2: Descriptive Statistics (Analyze One Dataset Project)

**Project:** Analyze One Real Dataset.  
**What you're building this chapter:** the descriptive-statistics section of your analysis — visualizations, summary statistics, outlier identification.  
**Tool:** **Claude Code** is genuinely useful here. Python with pandas + matplotlib + seaborn produces real charts.

---

**The Prompt:**

```
Chapter 2 of my Analyze-One-Dataset project. Foundation Document
is in this Claude Project. Chapter 2 covered: visualization
(histograms for distribution shape, box plots for spread + outliers,
scatter plots for relationships); measures of center (mean, median,
mode — the mean is sensitive to outliers, median is robust); measures
of spread (range, IQR, standard deviation, variance); percentiles
and z-scores; the empirical rule (68-95-99.7 for bell-shaped
distributions); the 1.5×IQR rule for outlier identification.

Write the brief's "Descriptive Analysis" section in 600-1000 words.

1. **Load and inspect.** Show the head() and info() output. Note
   any missing data, the data types pandas inferred, and the
   sample size (n).

2. **Univariate summaries.** For each quantitative variable:
   - Mean and standard deviation.
   - Median, IQR, min, max.
   - Skewness (positive = right-skewed = long right tail;
     negative = left-skewed; ~0 = roughly symmetric).
   - A histogram (produce with matplotlib).
   - A box plot.
   For each categorical variable:
   - Counts and proportions.
   - A bar chart.

3. **Outlier identification.** Use the 1.5×IQR rule:
   - Lower bound: Q1 - 1.5×IQR.
   - Upper bound: Q3 + 1.5×IQR.
   - Anything outside these bounds is flagged.
   For each outlier, note whether it's a data-entry error (clearly
   wrong values), a genuine extreme observation, or context-
   dependent (some sports stats produce many "outliers" that are
   real records).

4. **The empirical rule check.** Pick a quantitative variable that
   looks roughly bell-shaped on its histogram. Check whether ~68%
   of data falls within mean ± 1 SD, ~95% within mean ± 2 SD,
   ~99.7% within mean ± 3 SD. If it doesn't, the variable isn't
   well-approximated by a Normal distribution — note that for
   Chapter 6's revisit.

5. **Z-scores of two extremes.** Pick the maximum value and the
   minimum value of one variable. Compute z = (x − mean) / SD.
   In a Normal distribution, |z| > 3 happens about 0.3% of the
   time. If your extreme has |z| > 3, the variable likely isn't
   Normal (or these are real outliers worth investigating).

End with a "Key descriptive findings" paragraph — the 2-3 most
notable patterns from the descriptive analysis. These often shape
what hypothesis tests will be most interesting in Chapters 9-13.
```

---

**What this produces:** A 600-1000 word section with summary tables, histograms, box plots, and outlier analysis. The descriptive section is the foundation of any analysis report — clean output here pays for itself across all subsequent chapters.

**How to adapt this prompt:**

- *For your own project:* If your dataset has 100+ variables (some have), focus on the 5-10 most relevant to your big question.
- *For ChatGPT / Gemini:* Works as written.
- *For Claude Code:* The right tool here. A Jupyter notebook with cell-by-cell visualization is the cleanest workflow.
- *For a Claude Project:* Save the notebook output (HTML export) alongside the prose.

**Connection to previous chapters:** Chapter 1's variable inventory tells you what to compute summaries for.

**Preview of next chapter:** Chapter 3 covers probability. You'll compute conditional probabilities and contingency tables for categorical variables in your dataset.

---

## AI Wayback Machine

**John Tukey** invented exploratory data analysis — the framework underlying every modern look at a dataset.

**Run this:**

```
Who is John Tukey, and how does their work connect to descriptive statistics we covered in this chapter? Keep it to three paragraphs. End with the single most surprising thing about their career or ideas.
```

→ Search **"John Tukey"** on Wikipedia.

**Now make the prompt better.** Try one of these:

- Ask it to apply John Tukey's framework to a small dataset you actually have.
- Add a constraint: "Answer including criticisms or limits of John Tukey's framework."

What changes? What gets better? What gets worse?
