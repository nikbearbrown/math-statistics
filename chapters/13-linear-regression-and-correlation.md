# Chapter 13 — Linear Regression and Correlation
*On what it means for two things to move together, and what it doesn't mean.*

---

Francis Galton was an obsessive measurer. In the 1870s, he grew sweet pea plants, recorded the sizes of their seeds, then collected seeds from the tallest plants and from the shortest plants and grew a new generation. He measured the offspring. And something strange happened: the children of the tallest plants were, on average, shorter than their parents. The children of the shortest plants were, on average, taller.

The offspring did not match the parents. They moved back toward the middle.

Galton called this regression — regression to the mean. The word stuck, and we now use it for the entire machinery of fitting lines to data. But the underlying phenomenon is worth sitting with before we touch a formula. Sons of very tall fathers are tall, but not as tall as their fathers. A company that dominates its market one year does not dominate quite as thoroughly the next. A student who scores exceptionally high on one exam does not score quite as high on the next.

This is not a flaw. It is not noise. It is a real property of the world. The extreme value was partly skill, partly luck. Luck doesn't repeat reliably. Each time you measure an extreme, some of the extremeness evaporates.

Galton's deeper insight: if two variables are related — heights of parents and children, exam scores in two semesters, drug dose and response — you can draw a line through the scatter, and that line is not arbitrary. It is the line that best predicts one variable from the other. And because it is a prediction, it already incorporates regression to the mean. It moves you from the observed extreme toward the average by exactly the right amount.

That is what this chapter is about — how to find that line, what it tells you, and what it cannot tell you.

---

## Seeing co-movement

Before you can fit a line, you need to see whether there's something to fit. The tool is a scatter plot. Put one variable on the horizontal axis, the other on the vertical axis, and plot each observation as a point.

If the cloud of points tilts from lower left to upper right — high values on one axis tend to go with high values on the other — the variables are positively associated. If the cloud tilts from upper left to lower right, they're negatively associated. If the cloud is a shapeless blob, there's no linear relationship.

The scatter plot gives you the direction and a rough sense of strength. To quantify strength, you compute $r$, the correlation coefficient.

$$r = \frac{\sum (x_i - \bar{x})(y_i - \bar{y})}{\sqrt{\sum (x_i - \bar{x})^2 \sum (y_i - \bar{y})^2}}$$

The formula looks dense but the idea is clean. For each data point, you measure how far $x$ is from its mean and how far $y$ is from its mean, then multiply those two distances. If both are above their means, the product is positive. If one is above and one is below, the product is negative. If they consistently go the same direction, the products are consistently positive and sum to something large. If they consistently go opposite directions, the sum is large and negative. If there's no pattern, positive and negative products cancel and the sum is near zero.

The denominator is a scaling factor — it ensures $r$ always falls between $-1$ and $1$ regardless of the units of measurement.

$r = 1$: perfect positive linear relationship, every point on a line tilting upward.
$r = -1$: perfect negative linear relationship, every point on a line tilting downward.
$r = 0$: no linear relationship; the cloud is a blob.
$r = 0.8$: strong positive association, points cluster tightly around an upward slope.
$r = 0.3$: weak positive association, a slight upward tendency lost in wide scatter.

<!-- → [IMAGE: five scatter plots arranged in a row — from left to right: r=-1 (tight downward line), r=-0.7 (loose downward cloud), r=0 (shapeless blob), r=0.7 (loose upward cloud), r=1 (tight upward line); each labeled with its r value; same scale and point count in each; student sees at a glance how point-cloud tightness and tilt correspond to magnitude and sign of r] -->

One thing to watch: $r$ measures *linear* association. Two variables could be perfectly related in a curved way and still have $r$ near zero. A correlation of zero does not mean the variables are independent; it means there's no linear pattern. Always look at the scatter plot before trusting the number.

We often report $r^2$ instead of $r$. It has a cleaner interpretation: $r^2$ is the proportion of the variation in $y$ that is explained by $x$ through the linear relationship. If $r = 0.9$, then $r^2 = 0.81$, meaning 81% of the variation in $y$ is accounted for by the linear association with $x$. The remaining 19% is explained by something else — or by nothing measurable at all.

A critical point about $r$ and causation: the correlation coefficient is a number that measures co-movement. It is not a number that measures cause. If education and earnings are correlated with $r = 0.92$, that is not evidence that education causes high earnings. The two variables move together. Why they move together is a separate question, and $r$ contributes nothing to answering it.

---

## The line that fits best

Now suppose the scatter plot shows a reasonable linear pattern. You want to draw a line — not just any line, but the one that best summarizes the data. What does "best" mean?

The answer: best means it minimizes the squared prediction errors.

For every data point, you can measure the residual — the vertical distance between the actual $y$ value and what the line predicts at that $x$ value:

$$e_i = y_i - \hat{y}_i$$

The residual is what the line misses. If the point is above the line, the residual is positive. Below the line, negative. If you just added up all residuals, positive and negative would cancel out — for any line that passes through the mean of the data, the residuals sum to zero exactly. That's useless for measuring total error.

So instead you square each residual and add those squares. The sum of squared errors, SSE, is a measure of total miss. The least-squares regression line is the unique line that minimizes SSE.

Why square rather than take absolute values? Because the calculus is clean. The minimum of a sum of squared terms has a closed-form solution. You don't have to search for it numerically — you can derive it directly.

The equation of the regression line is:

$$\hat{y} = b_0 + b_1 x$$

where $b_1$ is the slope and $b_0$ is the intercept. These come from minimizing SSE, and they work out to:

$$b_1 = r \cdot \frac{s_y}{s_x}$$

$$b_0 = \bar{y} - b_1 \bar{x}$$

Look at the slope formula. It says the slope is the correlation scaled by the ratio of standard deviations. This connection between $r$ and the regression line is not a coincidence — it is the machinery that links the two ideas.

If $r = 0$, the slope is zero — the line is horizontal, and $x$ predicts nothing. If $r = 1$, the slope is the ratio of standard deviations — maximum responsiveness, every unit change in $x$ predicts a unit change measured in the units of $y$. The standard deviations convert $r$ from a dimensionless number into a concrete slope with the right units.

The intercept formula says the line passes through the point $(\bar{x}, \bar{y})$ — the means of both variables. This is always true for the least-squares line. If you know the slope and you know the means, the line is determined.

<!-- → [IMAGE: scatter plot of ~20 points with the least-squares line drawn through them; a bold dot marks the point (x̄, ȳ) labeled "the line always passes through the means"; a vertical dashed line from one data point to the regression line labeled "residual eᵢ = yᵢ − ŷᵢ"; arrows show two residuals, one positive (point above line) and one negative (point below line); helps students see how the line and residuals relate geometrically before computing] -->

Let me work through this fully. A statistics class has 25 students.

- Mean midterm: $\bar{x} = 74$
- Mean final: $\bar{y} = 76$
- Standard deviation of midterm: $s_x = 10$
- Standard deviation of final: $s_y = 11$
- Correlation: $r = 0.85$

Slope:

$$b_1 = 0.85 \times \frac{11}{10} = 0.85 \times 1.1 = 0.935$$

Intercept:

$$b_0 = 76 - 0.935 \times 74 = 76 - 69.19 = 6.81$$

Regression line: $\hat{y} = 6.81 + 0.935x$.

Prediction for a student who scored 85 on the midterm:

$$\hat{y} = 6.81 + 0.935 \times 85 = 6.81 + 79.48 = 86.29$$

The predicted final exam score is about 86.

Now here is Galton's regression in the numbers. The student scored 85 on the midterm — 11 points above the mean of 74. But the predicted final score of 86.29 is only 10.29 points above the mean final of 76. The prediction regresses toward the mean. You do not predict the same distance above average on the final as they showed on the midterm. You predict somewhat less, because part of the midterm advantage was likely luck, and luck is not reliably repeated.

The regression line encodes exactly this. The slope is 0.935, not 1.0. Every unit deviation from the mean on the midterm corresponds to only 0.935 units of predicted deviation on the final. That fraction is $r \cdot (s_y / s_x) = 0.85 \times 1.1 = 0.935$. The correlation determines how much regression toward the mean the line predicts.

<!-- → [IMAGE: scatter plot of exam data with regression line ŷ = 6.81 + 0.935x; horizontal dashed line at ȳ=76 and vertical dashed line at x̄=74; a single point at (85, actual final) with a vertical bracket showing its distance above the mean final; a second bracket on the x-axis showing the 11-point deviation from the mean midterm; caption: "The deviation shrinks from 11 points above midterm mean to only 10.29 points above final mean — regression to the mean in action"] -->

---

## What the line misses: Residuals and $r^2$

The regression line summarizes the relationship. The residuals tell you what the summary misses.

Plot the residuals. Put the fitted values $\hat{y}$ on the horizontal axis and the residuals $e_i$ on the vertical axis. If the line is a good model, the residual plot should look like a random scatter around zero — no pattern, no trend, roughly equal spread above and below across the whole range.

If the residual plot shows a curve — points below the line at the extremes and above it in the middle, say — the relationship between $x$ and $y$ is not linear. You've fit a straight line to a bent relationship. The line is the wrong model.

If the residual plot fans out — small residuals for small $\hat{y}$, large residuals for large $\hat{y}$ — the spread of the data is not constant across the range of $x$. Your predictions are more reliable in some ranges than others.

If there are a few points with very large residuals, those are worth investigating. They are observations where the model fails substantially. Sometimes they're errors. Sometimes they're the most interesting cases in the dataset — the student who studied brilliantly after a poor midterm, the year a company had an anomalous result.

<!-- → [IMAGE: three residual plots in a row, each showing fitted values on the horizontal axis and residuals on the vertical axis — (1) random scatter around zero labeled "Good fit: assumptions met"; (2) curved pattern (U-shape) labeled "Nonlinearity: wrong model"; (3) fan shape widening to the right labeled "Heteroscedasticity: unequal variance"; each plot annotated with what the student should do in response] -->

The quantitative measure of overall fit is $r^2$, the coefficient of determination:

$$R^2 = 1 - \frac{\text{SS}_\text{error}}{\text{SS}_\text{total}}$$

$\text{SS}_\text{total} = \sum(y_i - \bar{y})^2$ is the total variation in $y$. $\text{SS}_\text{error} = \sum(y_i - \hat{y}_i)^2$ is the variation left unexplained after fitting the line. Their ratio is the fraction of total variation that remains unexplained. One minus that fraction is $R^2$ — the fraction explained.

For the exam data, $r = 0.85$, so $R^2 = 0.7225$. The regression line explains 72.25% of the variation in final exam scores. The remaining 27.75% comes from everything the midterm score didn't capture: how much students studied between exams, illness, anxiety, luck on particular questions.

27.75% is substantial. If you're using this line to advise students, you need to know that the model misses more than a quarter of the story. The line is useful. It is not complete.

---

## What regression can't do

Here is the most important section of this chapter, and the one most often ignored.

The regression line quantifies association. It finds the line that best predicts $y$ from $x$ in the data you have. It cannot tell you whether $x$ causes $y$.

Two variables can be correlated for three distinct reasons.

First, $x$ causes $y$. More education leads to higher earnings because employers pay premiums for educated workers. Higher doses cause more pain relief because of the drug's biochemistry.

Second, $y$ causes $x$. Higher earnings enable investment in more education. A strong patient response to a drug causes the prescriber to increase the dose.

Third, a third variable causes both. Cognitive ability drives both educational attainment and earning potential, independently of each other. A confounding variable makes two otherwise unrelated things appear related.

All three of these situations produce a scatter with an upward tilt. All three produce a positive $r$. All three produce the same regression line. The line does not distinguish between them. It is agnostic about causation.

<!-- → [INFOGRAPHIC: three-panel diagram, each showing the same upward scatter and identical regression line, but with different causal arrows underneath — Panel 1: X → Y (x causes y); Panel 2: Y → X (reverse causation); Panel 3: Z → X and Z → Y with no direct arrow between X and Y (confounding); all three labeled with a real example (education/earnings); caption: "The same regression line can arise from three completely different causal structures — the line cannot tell you which"] -->

To establish that $x$ causes $y$, you need more than a scatter plot. You need an experiment: randomize who gets the treatment and who doesn't, then compare outcomes. You need temporal precedence: $x$ must precede $y$ in time. You need to rule out confounders: show that no third variable plausibly explains the association.

Observational data — the kind most regression analyses use — can suggest causal hypotheses. It cannot confirm them. When a consulting firm reports "companies with more women on the board earn higher returns," that could mean board diversity improves performance, or it could mean high-performing companies are more likely to recruit women, or it could mean both are driven by better governance culture. Regression on observational data cannot tell you which.

The rule: **Do not claim causation from regression without experimental evidence or a theory that rules out confounding.**

There is also the assumption of linearity to name honestly. Regression assumes $x$ and $y$ are related by a straight line. Many real relationships are not. A drug might have no effect below a threshold dose, a strong effect in a middle range, and a plateau at high doses. A linear regression through that data would give you a misleading average slope and unreliable predictions across the full range.

Always plot the data first. If the scatter looks curved, do not force a straight line through it. Try transforming the variables — logarithms often linearize relationships that are multiplicative in their raw form — or fit a nonlinear model.

And finally, extrapolation. The regression line was fit in the range of $x$ you observed. If your data cover doses from 50 mg to 150 mg, the line knows about that range. Predicting the effect at 200 mg is extrapolation — you are guessing what the relationship does outside the region where you have evidence. The line might bend. It might break down entirely. Extrapolation is unreliable.

<!-- → [IMAGE: scatter plot with regression line extending beyond the data range — data points clustered between x=50 and x=150 with the line fitting them well; to the right, the line continues as a dashed extension past x=150 labeled "extrapolation: no data here"; a curved dotted line suggests the true relationship might plateau or reverse beyond the observed range; caption: "The line was fit only where you have data — predictions beyond that range assume the pattern continues, which may be false"] -->

The full and honest statement of what a regression line gives you: *Given the data I observed, assuming the relationship is linear in this range, assuming my next observation comes from the same population as my past observations, and assuming no unmeasured confounders are at work — this is the best linear prediction of $y$ given $x$.*

That is genuinely powerful. But every word in that sentence matters.

---

## A closing thought on what regression reveals

Galton noticed that children of exceptional parents tend toward the middle. He was puzzled at first, then understood: the exceptional parent is exceptional partly for reasons that transmit to children — genetics, environment, habit — and partly for reasons that don't — luck, timing, circumstance. The regression line is the mathematical statement of how much transmits and how much doesn't.

The slope tells you the transmission rate. A slope of 1.0 would mean perfect transmission — every deviation in the parent predicts an equal deviation in the child. A slope less than 1 (which is what you get whenever $r < 1$) means partial transmission — the child inherits some of the deviation but not all. The remaining fraction regresses to the mean.

This is why regression and correlation are not just computational tools. They are a way of seeing how much of any pattern in the world is stable and how much is transient. The physicist finds $R^2 = 0.999$ and concludes the relationship is a physical law — almost everything is transmitted, almost nothing is noise. The social scientist finds $R^2 = 0.4$ and concludes that factors are interacting in complex, poorly understood ways — much is transmitted, but much more is left unexplained.

The line is not the world. It is a sketch of the part of the world that follows a pattern.

---

## What would change my mind

The most important claim in this chapter is that correlation cannot establish causation. This is correct for observational data, but it deserves a nuance I have elided: certain designs applied to observational data — instrumental variables, regression discontinuity, difference-in-differences — can provide credible causal estimates under specific assumptions. They are not magic; they still require assumptions that may fail. But if you showed me compelling evidence from such a design, I would accept the causal claim even from observational data. The line between "observational" and "experimental" is not as sharp as introductory statistics courses suggest.

The expected-count floor for chi-square (from Chapter 11) has an analogue here: the normal-residuals assumption for inference. I teach it as a requirement, but for large samples, the central limit theorem makes inference relatively robust to non-normality. In practice, the assumption matters most for small samples and for the tails of prediction intervals.

---

## Connections forward

This chapter closes the statistical core of this book. But regression is not the end of the story — it is the beginning of a much larger one.

Multiple regression extends everything here to more than one predictor. Instead of $\hat{y} = b_0 + b_1 x$, you have $\hat{y} = b_0 + b_1 x_1 + b_2 x_2 + \cdots + b_k x_k$. The principle is identical — minimize SSE — but now you're working in higher dimensions, and the interpretation of each slope becomes "the effect of this predictor while holding all others constant." That last phrase is where a great deal of applied statistics lives.

Logistic regression handles the case where $y$ is categorical rather than continuous — whether a patient survives or doesn't, whether a customer buys or doesn't. The line becomes a curve, and the prediction becomes a probability rather than a measurement.

Time series analysis handles the case where your observations are not independent — each data point in a sequence depends on the ones before it. The regression framework breaks down when the independence assumption fails, and you need different tools.

And throughout all of this, the causal inference question remains open. Randomized controlled experiments remain the gold standard for establishing causation. The methods of causal inference from observational data — instrumental variables, natural experiments, matching — are the frontier where statistics and science meet. That frontier is where the most important questions live.

---

## Exercises

### Warm-up

**13.1** *(Interpreting r.)* A researcher reports that the correlation between daily hours of exercise and resting heart rate is $r = -0.68$. (a) What does the sign tell you? (b) What does the magnitude tell you? (c) Does this mean that exercising more *causes* a lower resting heart rate? What additional evidence would you need to make that claim?

**13.2** *(r vs. r².)* A regression model predicting house price from square footage has $r = 0.75$. (a) Calculate $r^2$. (b) A real estate agent says, "Square footage explains 75% of price variation." Is that correct? (c) What fraction of price variation is left unexplained, and what kinds of variables might account for it?

**13.3** *(Reading a regression line.)* The regression line for predicting a student's final exam score from their midterm score is $\hat{y} = 12 + 0.8x$. (a) Interpret the slope in context. (b) Interpret the intercept — does it have a meaningful real-world interpretation here? (c) Predict the final score for a student who scored 70 on the midterm.

### Application

**13.4** *(Computing the regression line.)* A company tracks advertising spend (thousands of dollars) and monthly sales (thousands of units). Summary statistics: $\bar{x} = 8$, $\bar{y} = 50$, $s_x = 3$, $s_y = 12$, $r = 0.78$. (a) Calculate the slope. (b) Calculate the intercept. (c) Write the equation. (d) Predict monthly sales when advertising spend is \$11,000. (e) Is this a prediction or an extrapolation? How do you know?

**13.5** *(Regression to the mean.)* Using the data from Exercise 13.4, suppose a particular month had advertising spend \$5,000 below the mean. (a) How many units below the mean sales does the regression line predict for that month? (b) Is the predicted deviation smaller or larger than the $x$ deviation? (c) Explain this in terms of regression to the mean and what it tells you about the role of luck in month-to-month performance.

**13.6** *(Residuals and r².)* For five data points, the actual $y$ values and predicted $\hat{y}$ values are:

| $y$ | $\hat{y}$ | Residual | Residual² |
|---|---|---|---|
| 42 | 38 | | |
| 55 | 59 | | |
| 63 | 61 | | |
| 47 | 50 | | |
| 70 | 68 | | |

Complete the table. The mean of $y$ is 55.4; $\text{SS}_\text{total} = 492.8$. Calculate $R^2$ and interpret it.

### Synthesis

**13.7** *(Causation claims.)* A data scientist analyzes a large company's HR records and finds that employees who attend more optional training sessions have higher salaries ($r = 0.54$, $p < 0.001$). The HR team proposes: "We should require more optional training because it raises salaries." Identify the three causal explanations that could produce this correlation. Which do you think is most plausible, and why? What would you need to test your preferred explanation?

**13.8** *(Nonlinearity and model choice.)* A researcher fits a linear regression to data on drug dose (0–200 mg) vs. symptom relief. The scatter plot shows the relationship is clearly curved — relief rises steeply from 0 to 100 mg, then flattens from 100 to 200 mg. The linear regression has $R^2 = 0.71$. (a) What does the residual plot likely look like? (b) Why is $R^2 = 0.71$ misleading here? (c) What would you do differently?

### Challenge

**13.9** *(What the slope really means.)* Two researchers study the same population and fit regression lines predicting son's height from father's height. Researcher A finds $b_1 = 0.45$ with $r = 0.45$ (since $s_x = s_y$ in their sample). Researcher B, using a different sample with much higher variance in fathers' heights, finds $b_1 = 0.45$ with $r = 0.62$. Explain how the same slope can correspond to different correlations. What does each researcher's $r$ tell you that the slope alone does not?

**13.10** *(The honest limit.)* A graduate student fits a regression model predicting college GPA from high school GPA across 800 students. She gets $R^2 = 0.38$ and a statistically significant slope of 0.6. She presents the finding as: "High school GPA is a strong predictor of college GPA." Critique this claim on three grounds: the meaning of $R^2 = 0.38$, the risk of confounding, and what "predictor" means versus "cause." Suggest one piece of additional data that would strengthen her analysis.

---

## LLM Exercise — Chapter 13: Linear Regression and Correlation (Analyze One Dataset Project) — FINAL CLOSER

**Project:** Analyze One Real Dataset — final integration.  
**What you're building this chapter:** linear regression on your dataset + the compiled analysis report.  
**Tool:** **Claude Code** + **Cowork** for final assembly.

---

**The Prompt:**

```
Chapter 13 — the closing chapter — of my Analyze-One-Dataset
project. Chapter 13 covered: scatter plots for visualizing
relationships; correlation coefficient r (between -1 and +1;
measures linear association strength); r² = coefficient of
determination (fraction of y's variance explained by x); least-
squares regression line ŷ = b₀ + b₁x; residuals (e = y − ŷ);
residual plot for assumption checking; assumptions (linearity,
independence, equal variance, normality of residuals); prediction
within vs. extrapolation outside data range; correlation ≠
causation.

Two parts.

PART A — Regression analysis.

1. **Pick two quantitative variables that could plausibly be
   related.** Build a scatter plot. Observe: is the relationship
   linear? Strong/moderate/weak? Positive/negative? Any outliers?

2. **Compute r and r².**
   - r = correlation coefficient.
   - r² = fraction of y's variance explained by x.
   - Interpret: a moderate r² (say, 0.40) means x explains 40%
     of y's variance.

3. **Fit the regression line.**
   - Slope b₁ = r · (s_y / s_x).
   - Intercept b₀ = ȳ − b₁ x̄.
   - Or use scipy.stats.linregress or statsmodels.api.OLS.
   - Interpret slope in context: "for each one-unit increase in
     [x], [y] increases on average by [slope] units."

4. **Check assumptions via residual plot.** Plot residuals vs.
   fitted values:
   - Random scatter → assumptions met.
   - Pattern → linearity violated.
   - Funnel shape → heteroscedasticity (unequal variances).
   - Outliers → influential points.

5. **The honest reading.** Even if r² is high, correlation ≠
   causation. Address:
   - Could the relationship be confounded by another variable?
   - Could it be reversed (y → x instead of x → y)?
   - Could it be coincidence (with enough variables, some
     correlate by chance)?

PART B — Compile the final report.

Have Claude assemble the full analysis. Each chapter's section is
already in the project. Need to:

1. Write the **Executive Summary** (300-500 words) at the top of
   the report:
   - The dataset and its source.
   - The big question.
   - The three or four most important findings.
   - The conclusion (with calibrated confidence).

2. Order the sections logically (the chapter order works, but
   tighten transitions).

3. Write a **Conclusions** section at the end:
   - Direct answer to the big question.
   - Limitations: sampling issues from Ch 1 + assumption violations
     from later chapters.
   - "What I'd refine if I had more data or more methods" — name
     specifically what's missing.

4. Format the document professionally — section headers, tables,
   figures with captions, references.

5. Output the **compiled report** as a single markdown document
   (5,000–8,000 words). This is the project's deliverable.

End with a 200-word **Coda** — what 13 chapters of analyzing one
dataset taught you about statistics that the textbook alone
wouldn't have. The coda is the project's intellectual-honesty
close.
```

---

**What this produces:** A regression analysis + the compiled 5,000–8,000 word statistical analysis report. This is the project's portfolio piece — a real piece of statistical analysis the student can show.

**How to adapt this prompt:**

- *For your own project:* The Executive Summary is the section a hiring manager or grad-school admissions reader will actually read. Spend disproportionate effort on it.
- *For ChatGPT / Gemini:* Works as written.
- *For Claude Code:* `scipy.stats.linregress` or `statsmodels.api.OLS` for regression.
- *For Cowork:* The right tool for the final assembly — Cowork can pull all 13 sections together into one document.

**Connection to previous chapters:** Every prior chapter feeds this. The "what 13 chapters taught me" coda is the project's reflective close.

**Preview of next chapter:** There is no next chapter — this is the close. What's next is the analysis report itself: a portfolio-quality piece of statistical analysis that any data-curious reader could follow.

---

## AI Wayback Machine

**Gertrude Cox** was the first woman to chair a statistics department in the US — at North Carolina State — and a pioneer of experimental design and regression methods.

**Run this:**

```
Who is Gertrude Cox, and how does their work connect to regression and correlation we covered in this chapter? Keep it to three paragraphs. End with the single most surprising thing about their career or ideas.
```

→ Search **"Gertrude Cox"** on Wikipedia.

**Now make the prompt better.** Try one of these:

- Ask it to apply Gertrude Cox's framework to a small dataset you actually have.
- Add a constraint: "Answer including criticisms or limits of Gertrude Cox's framework."

What changes? What gets better? What gets worse?
