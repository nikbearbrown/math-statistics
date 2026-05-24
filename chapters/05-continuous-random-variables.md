# Chapter 5 — Continuous Random Variables
*Where probability becomes area, and a single point vanishes into nothing.*

---

Here is something worth sitting with before we start.

You pull up to a bus stop. The schedule says the bus comes every 12 minutes. You have no idea when the last one left. The time you wait — let's call it $X$ — is a random variable. But it's a different *kind* of random variable than anything you've met so far. You won't wait exactly 3 minutes with some probability. You won't wait exactly 7 minutes with some probability. You might wait 2.3 minutes, or 7.814 minutes, or 11.9992 minutes. Every real number in the interval $[0, 12]$ is a possible outcome. And that means something strange: the probability that you wait *exactly* 5 minutes is zero. Not small. Zero.

That's the thing we need to explain. Because it's genuinely strange, and if you just memorize the formula without understanding why it's true, you'll use it correctly and understand it not at all.

---

## The problem with measuring things

All through Chapter 4, we counted. How many heads in ten flips. How many customers in an hour. How many failures before the first success. The outcomes were integers, and integers have gaps between them. There is no outcome between 3 and 4. That's what made probability simple: the probability of each outcome was a definite number, and the probabilities summed to 1.

Now we're measuring. Not counting. Measuring time, distance, weight, temperature. And measured quantities don't have gaps. Between any two real numbers there are infinitely many others. Between 5 and 6 there's 5.5. Between 5 and 5.5 there's 5.25. Between 5 and 5.25 there's 5.125. This subdivision never stops.

Here's the consequence. Suppose you want to assign probabilities to every possible wait time in $[0, 12]$. In the discrete world, you had a list of outcomes — 0, 1, 2, 3, ... — and each got a probability, and they summed to 1. In the continuous world, you have an uncountable infinity of outcomes. If you tried to assign each one a positive probability — even a very small one — the sum would be infinite, not 1. The probabilities would overflow.

The only way out is to accept that the probability of any *single* point is zero. Not approximately zero. Exactly zero. The probability lives in intervals, not at points.

This is the foundational fact of continuous probability, and it resolves the apparent paradox immediately. $P(X = 5) = 0$. But $P(4.9 < X < 5.1) = \text{something real}$. The probability is spread continuously over the interval, so thinly that any single point gets none of it.

![Comparison ](images/05-continuous-random-variables-fig-01.png)
*Figure 5.1 — Comparison *

---

## Density instead of mass

In the discrete world, you had a **probability mass function**: a recipe that told you $P(X = k)$ for each integer $k$. The word "mass" is intentional — probability sits at points, like mass sitting on a number line.

In the continuous world, you have a **probability density function**, written $f(x)$. The word "density" is also intentional. Think of it like mass per unit length — the concentration of probability at each location. To get actual probability, you have to collect density over an interval, which means you compute the area under the curve.

$$P(c < X < d) = \text{area under } f(x) \text{ between } x = c \text{ and } x = d$$

This is not an analogy. It is the definition. The probability that $X$ falls between $c$ and $d$ is the area enclosed under the density curve from $c$ to $d$. If you know calculus, this is an integral. If you don't, it's still something you can work with — for the two distributions in this chapter, the geometry is simple enough to compute by hand.

Two consequences follow immediately. First, since the probability that $X$ lands *somewhere* in its domain is 1 (certainty), the total area under $f(x)$ must equal 1. Every valid density function integrates to 1. Second, since adding or removing a single point from an interval doesn't change the area, the distinction between $P(c < X < d)$ and $P(c \leq X \leq d)$ vanishes. Both give the same answer. The endpoints have zero probability individually, so including or excluding them changes nothing.

One thing to watch for: the height of the density function at a point is *not* a probability. It can be greater than 1, and often is. What matters is area. A density of 2 at a point just means probability is highly concentrated there — spread it over a thin enough interval and the area stays small.

![A smooth density curve f(x) with a shaded](images/05-continuous-random-variables-fig-02.png)
*Figure 5.2 — A smooth density curve f(x) with a shaded*

---

## The uniform distribution: what ignorance looks like

The bus problem has a natural first model. You arrive with no idea where you are in the 12-minute cycle. One part of the interval seems exactly as likely as any other. The density function should be flat — a horizontal line.

This is the **uniform distribution**. If $X \sim U(a, b)$, then $X$ is equally likely to fall anywhere in $[a, b]$, and the density function is:

$$f(x) = \frac{1}{b - a} \quad \text{for } a \leq x \leq b$$

The height is constant. The shape is a rectangle with base $(b - a)$ and height $\frac{1}{b-a}$. Its area is exactly 1, as required.

![Uniform distribution rectangle for U(0,12) ](images/05-continuous-random-variables-fig-03.png)
*Figure 5.3 — Uniform distribution rectangle for U(0,12) *

To find any probability, you compute the area of the relevant slice — still a rectangle, just narrower:

$$P(c < X < d) = (d - c) \cdot \frac{1}{b - a} = \frac{d - c}{b - a}$$

This is base times height, which is just arithmetic. The probability is the fraction of the total interval that your sub-interval occupies. If the total interval is 12 minutes and you're asking about a 3-minute window, the probability is $\frac{3}{12} = 0.25$.

The mean of a uniform distribution is the midpoint:

$$\mu = \frac{a + b}{2}$$

This makes intuitive sense — if all outcomes are equally likely, the average is right in the middle. The standard deviation is less obvious:

$$\sigma = \sqrt{\frac{(b - a)^2}{12}}$$

The derivation belongs in a more advanced course. The intuition is: wider intervals mean more spread, so $\sigma$ grows with $(b - a)$.

Let me work through the bus problem fully.

$X \sim U(0, 12)$. You arrive at the stop and want to know several things.

*What is the probability you wait fewer than 3 minutes?*

$$P(X < 3) = \frac{3 - 0}{12 - 0} = \frac{3}{12} = 0.25$$

One quarter of the time, you're lucky.

*What is the 75th percentile of wait times?*

You want the value $k$ such that $P(X < k) = 0.75$.

$$0.75 = \frac{k}{12} \implies k = 9$$

Three-quarters of the time you wait less than 9 minutes. One quarter of the time you're waiting at least that long.

*What is the mean wait?*

$$\mu = \frac{0 + 12}{2} = 6 \text{ minutes}$$

On average you wait half the cycle — which is exactly what you'd expect if all positions in the cycle are equally probable.

The uniform distribution is the "I know nothing except the bounds" model. It is the maximum-entropy assumption when your only information is that the outcome lies in some finite range. It doesn't say the situation is random — it says you're ignorant of the pattern, if there is one. When buses bunch in real cities (two come together and then there's a long gap), the uniform model is wrong. But for first approximations, and for the many situations where you genuinely don't know where you are in the cycle, it is honest.

---

## The exponential distribution: why short times dominate

The uniform distribution says all outcomes are equally likely. Most real waiting times aren't like that. When you're waiting for a bus that doesn't run on a fixed cycle, or waiting for a customer service call to arrive, or waiting for a rare equipment failure — short times are much more likely than long times. The distribution is not flat. It slopes steeply downward.

The **exponential distribution** describes the time between events that arrive at random at a constant average rate. It's the continuous counterpart of the Poisson distribution from Chapter 4. If events arrive at an average rate of one every $\mu$ minutes, then the time $X$ until the next event follows $X \sim \text{Exp}(\mu)$, with density:

$$f(x) = \frac{1}{\mu} e^{-x/\mu}, \quad x \geq 0$$

Here $e \approx 2.718$ is Euler's number, the base of natural logarithms. The density is highest at $x = 0$ and decays as $x$ grows. Short waits are the most probable; long waits are possible but increasingly rare.

![Exponential density curve for Exp(4) ](images/05-continuous-random-variables-fig-04.png)
*Figure 5.4 — Exponential density curve for Exp(4) *

Both the mean and the standard deviation equal $\mu$:

$$\mu = \text{mean}, \quad \sigma = \mu$$

The fact that the standard deviation equals the mean is a signature of exponential data. It means the distribution is highly variable — spread over a range roughly as wide as the mean itself.

Computing probabilities requires the cumulative distribution function. For a continuous random variable, the CDF at value $x$ gives the area to the left of $x$ — the probability that $X$ is at most $x$:

$$P(X \leq x) = 1 - e^{-x/\mu}$$

From this, everything else follows:

$$P(X > x) = 1 - P(X \leq x) = e^{-x/\mu}$$

$$P(c < X < d) = P(X < d) - P(X < c) = e^{-c/\mu} - e^{-d/\mu}$$

Let me work through a concrete example. Calls arrive at a help desk at an average rate of one every 4 minutes. The time until the next call is $X \sim \text{Exp}(4)$.

*What is the probability the next call arrives within the first minute?*

$$P(X < 1) = 1 - e^{-1/4} = 1 - e^{-0.25} \approx 1 - 0.7788 = 0.2212$$

About 22% chance. Not huge — but remember, the average is 4 minutes, so arriving in under 1 minute is genuinely uncommon.

*What is the probability of waiting longer than 5 minutes?*

$$P(X > 5) = e^{-5/4} = e^{-1.25} \approx 0.2865$$

About 29% chance. This seems high — you might expect that with a mean of 4 minutes, waiting 5+ minutes would be rare. But the exponential has a heavy right tail. Long waits happen more than you'd intuit from the mean alone.

*What is the median wait time?*

The median satisfies $P(X \leq m) = 0.5$:

$$1 - e^{-m/4} = 0.5 \implies e^{-m/4} = 0.5 \implies -\frac{m}{4} = \ln(0.5) \approx -0.693$$

$$m \approx 2.77 \text{ minutes}$$

The median is about 2.8 minutes, well below the mean of 4. This is always true for the exponential: the distribution is right-skewed, the mean is pulled upward by the possibility of very long waits, and the median — the typical experience — is lower.

---

## The memoryless property

Here is the thing about the exponential distribution that is genuinely surprising.

Suppose 6 minutes have passed since the last call. No new call has arrived. You might think: "The mean is 4 minutes. I've already waited 6. The next call must be coming soon — it's overdue."

You would be wrong.

The **memoryless property** of the exponential distribution says: given that you've already waited $r$ minutes, the probability of waiting *another* $t$ minutes is the same as the probability of waiting $t$ minutes from the start. Your past waiting time carries no information about your future waiting time.

In symbols:

$$P(X > r + t \mid X > r) = P(X > t) = e^{-t/\mu}$$

For our example, with $\mu = 4$ and having already waited $r = 6$ minutes:

$$P(X > 6 + 1 \mid X > 6) = P(X > 1) = e^{-1/4} \approx 0.779$$

The probability of waiting another minute is the same as it was at the very start. The clock resets. There is no "due" call.

![The distribution of remaining wait time is identical regardless of how long you have already waited](images/05-continuous-random-variables-fig-05.png)
*Figure 5.5 — Timeline diagram illustrating the memoryless property *

Why does this happen? Because the exponential distribution models truly random arrivals — events that happen at a constant rate, independent of when the last one occurred. The randomness doesn't have memory. The probability that a call arrives in the next second is the same whether the last call was 1 second ago or 10 minutes ago. The past left no trace.

This is strange, but it follows directly from the mathematics of exponential decay. The conditional probability calculation works out to give the same density function you started with. The distribution is the only continuous distribution with this property.

The memoryless property has consequences that can seem paradoxical. For equipment lifetimes: if a component's failure time is exponentially distributed, a 10-year-old component has exactly the same probability of failing in the next year as a brand-new one. Age tells you nothing about remaining life. This is often unrealistic — real components wear out, and older parts fail more often than newer ones. For physical wear, the exponential is the wrong model. But for truly random events — cosmic ray strikes, radioactive decay, random electrical failures — the memoryless property holds, because the underlying randomness has no mechanism for remembering the past.

---

## How the two distributions relate to each other

The uniform and exponential distributions are in some sense philosophical opposites.

The uniform distribution says: I know the range, and within it, I know nothing. All outcomes are equally likely. The density is flat. Use it when ignorance is the honest position.

The exponential distribution says: I know the average rate, and I know that events are random. Short times are more probable than long ones. The density decays. Use it when you're modeling time until the next occurrence of a random event.

Both have clean formulas. Both capture real phenomena. And both will appear again.

The uniform distribution is rarely the final answer — real data is almost never flat. But it is the right model when you have no information beyond the bounds, and it is the foundation for thinking about what it means to choose a distribution. When you later build more complex models, uniform distributions often appear inside them as building blocks.

The exponential distribution is the continuous partner of the Poisson. If events arrive at an average rate of $\lambda$ per hour (Poisson count), then the time between events is exponential with mean $\mu = 1/\lambda$. You can go back and forth between these two descriptions of the same process: count the arrivals (discrete, Poisson) or measure the gaps (continuous, exponential). They're two ways of describing the same underlying randomness.

![A timeline of random events (vertical tick marks](images/05-continuous-random-variables-fig-06.png)
*Figure 5.6 — A timeline of random events (vertical tick marks*

---

## Why this chapter matters for everything that follows

All the inference you'll do in later chapters — confidence intervals, hypothesis tests, $p$-values — depends on continuous distributions. When you compute a $p$-value, you're computing the area under a curve: the probability, under the null hypothesis, of observing something at least as extreme as your data. That's exactly what you've been doing in this chapter. The curve changes (normal, $t$, $\chi^2$, $F$), but the question is always the same: what area does this interval have?

The shift from discrete to continuous is also the shift from summing to integrating — from adding up probabilities at discrete points to sweeping across a continuous range. The conceptual move is the same as the shift from counting to measuring. Once you're comfortable with the idea that probability is area, the rest of statistics becomes a matter of knowing which curve to use.

---

## What would change my mind

The claim that uniform is the "maximum entropy" assumption given only a finite range is a defensible position in information theory, but real waiting-time data — buses, queues, service times — almost never comes out flat. If I were shown systematic evidence that real bus arrival times within stated intervals are consistently non-uniform in predictable ways, I would revise how strongly I recommend uniform as a first model.

The memoryless property claim — that the exponential is the *only* continuous distribution with this property — has a clean proof. I don't expect to revise it. But the intuition for *why* constant-rate arrivals must produce this particular decay shape still feels incomplete to me. The algebra works out; the necessity hasn't fully clicked in the way that, say, the necessity of the balance-point interpretation of the mean clicks.

---

## Connections forward

Chapter 6 introduces the normal distribution — the most important continuous distribution in statistics, and the one that describes what happens to sample means. Chapter 7 proves that as sample sizes grow, sample means from almost any distribution converge to a normal shape, which is why the normal distribution shows up everywhere in inference. Every $p$-value you compute in Chapters 8 through 13 is an area under a continuous curve. The tools you built here — reading a density, computing area as probability, recognizing the shape of a distribution — are the vocabulary for everything that follows.

---

## Exercises

### Warm-up

**5.1** *(Discrete vs. continuous.)* For each of the following, state whether the outcome is best modeled as discrete or continuous, and give one sentence of justification. (a) The number of phone calls a call center receives in an hour. (b) The duration in seconds of a single phone call. (c) The shoe size (in whole numbers) of a customer. (d) The weight in grams of a package, measured to the nearest tenth.

**5.2** *(Reading the density function.)* A continuous random variable $X$ has density $f(x) = 0.2$ for $0 \leq x \leq 5$. (a) Verify this is a valid density. (b) Compute $P(1 < X < 4)$. (c) Compute $P(X = 3)$. (d) Compute $P(X \leq 2)$. What distribution is this, and what are its parameters?

**5.3** *(Uniform probability.)* A commuter train arrives uniformly between 7:00 and 7:20 a.m. You arrive at the platform at 7:00. What is the probability the train arrives within the first 6 minutes? What is your mean wait time?

### Application

**5.4** *(Uniform: mean, SD, percentile.)* The lifetime of a type of fluorescent bulb is uniformly distributed between 800 and 1,400 hours. (a) Find the mean and standard deviation of the lifetime. (b) What is the probability a bulb lasts more than 1,200 hours? (c) Find the 40th percentile of bulb lifetime. Interpret it in plain language.

**5.5** *(Exponential probabilities.)* Patients arrive at an urgent care clinic at an average rate of one every 6 minutes. The time between arrivals is exponentially distributed. (a) What is the probability the next patient arrives within 3 minutes? (b) What is the probability the gap between arrivals exceeds 10 minutes? (c) What is the median time between arrivals? Is it greater or less than the mean? Why?

**5.6** *(Memoryless property in practice.)* A machine part has an exponentially distributed failure time with a mean of 500 hours. The part has already been running for 200 hours without failure. What is the probability it runs for at least another 300 hours? How does your answer compare to the probability that a brand-new part runs 300 hours without failure? Explain why these are equal using the memoryless property.

### Synthesis

**5.7** *(Connecting Poisson and exponential.)* Emergency calls to a dispatch center arrive at an average rate of 3 per hour. (a) What distribution models the number of calls in a fixed one-hour window? What is its mean? (b) What distribution models the time between calls? What is its mean in minutes? (c) Using the exponential model, what is the probability the next call arrives within 10 minutes? (d) A dispatcher says: "We've had no calls for 25 minutes — something must be wrong." Is this reasoning sound? Explain using the memoryless property.

**5.8** *(Choosing the right model.)* You are analyzing two types of data from a manufacturing process: (A) the number of defective items per batch of 100, and (B) the time in hours between quality-control alerts. For each, name the distribution family you would consider first, state whether it is discrete or continuous, and give one practical reason the other family would be wrong for that variable.

**5.9** *(Density height vs. probability.)* A student says: "The density of $f(x) = 4$ at $x = 0.1$ for the distribution $U(0, 0.25)$ must be wrong — probabilities can't be 4." Correct the student's reasoning in two sentences. Then verify that the stated density gives a valid probability distribution by computing the total area.

### Challenge

**5.10** *(Limits of the exponential model.)* A manufacturer claims the lifetime of its solid-state drives is exponentially distributed with a mean of 5 years. A reliability engineer argues this is almost certainly wrong. (a) What does exponential lifetime imply about the failure rate of a drive that has been running for 4 years versus a brand-new drive? (b) Why is this physically implausible for mechanical or electronic components subject to wear? (c) What qualitative shape of lifetime distribution would you expect for a component that truly wears out over time? You do not need to name a specific distribution — describe the shape and where the density should be concentrated.

---

## LLM Exercise — Chapter 5: Continuous Random Variables (Analyze One Dataset Project)

**Project:** Analyze One Real Dataset.  
**What you're building this chapter:** continuous-distribution analysis — uniform/exponential fitting where applicable.  
**Tool:** **Claude Code** + **Claude Project**.

---

**The Prompt:**

```
Chapter 5 of my Analyze-One-Dataset project. Chapter 5 covered:
continuous random variables; pdf (probability density function);
the fundamental shift — probability is area under the pdf (between
two points), not at a point (which is always 0); uniform
distribution (constant density between a and b); exponential
distribution (rate parameter λ; memoryless property — what
happened doesn't affect future).

Write the brief's "Continuous Variable Analysis" section in 400-600
words.

1. **Pick a continuous variable.** From your variable inventory,
   identify a quantitative variable that's genuinely continuous
   (time durations, prices, measurements, distances, etc.).

2. **Plot the empirical density.** Use kernel density estimate
   (KDE) or a smoothed histogram. Note the shape — symmetric?
   skewed? bimodal? bounded?

3. **Try fitting a continuous distribution.** Three plausible
   candidates:
   - **Uniform(a, b)**: if the variable is bounded with roughly
     constant density between a and b.
   - **Exponential(λ)**: if the variable is a "wait time" or
     "interval between events" — has a long right tail with most
     mass near 0.
   - **Normal(μ, σ)**: if approximately symmetric and bell-shaped
     (Chapter 6 will go deeper).

   For each candidate, estimate the parameters from the data,
   then overlay the theoretical pdf on the empirical histogram.

4. **Compute a probability as area.** Pick a meaningful interval
   in your variable's range. Compute P(a < X < b):
   - From the empirical CDF: count the fraction of data in the
     interval.
   - From the fitted distribution: integrate the theoretical pdf
     between a and b (or use scipy.stats CDF: F(b) - F(a)).
   - Compare. Are they close?

5. **The honest fit verdict.** Continuous distributions rarely
   fit real data perfectly. Note where the fit fails — usually in
   the tails (real data has heavier or lighter tails than the
   theoretical distribution). The Normal is famously poor for
   financial data (real returns have fat tails).

End with: which interval of your variable's range has the most
practical meaning? Compute the probability and interpret.
```

---

**What this produces:** A 400-600 word section with fitted continuous distribution + computed probabilities. The "where the fit fails" diagnosis is valuable.

**How to adapt this prompt:**

- *For your own project:* If your dataset has no obvious continuous variable, you may be working with mostly categorical/discrete data. Note that and move on; later chapters won't depend on this section.
- *For ChatGPT / Gemini:* Works as written.
- *For Claude Code:* `scipy.stats.expon`, `scipy.stats.uniform`, `scipy.stats.norm` for theoretical pdfs and CDFs.
- *For a Claude Project:* Append.

**Connection to previous chapters:** Ch 4 (discrete) and Ch 5 (continuous) are paired. Together they cover all numerical variables.

**Preview of next chapter:** Chapter 6 zooms in on the Normal distribution — the most important continuous distribution in statistics. You'll standardize your data to z-scores and test bell-shape claims.

---

## AI Wayback Machine

**Abraham de Moivre** derived the normal approximation to the binomial in 1733 — the first appearance of the normal distribution in print.

**Run this:**

```
Who is Abraham de Moivre, and how does their work connect to continuous random variables we covered in this chapter? Keep it to three paragraphs. End with the single most surprising thing about their career or ideas.
```

→ Search **"Abraham de Moivre"** on Wikipedia.

**Now make the prompt better.** Try one of these:

- Ask it to apply Abraham de Moivre's framework to a small dataset you actually have.
- Add a constraint: "Answer including criticisms or limits of Abraham de Moivre's framework."

What changes? What gets better? What gets worse?

## Prompts

Use these prompts with Claude to generate interactive D3 v7 versions of the
figures in this chapter. Each produces a standalone HTML file you can open
in a browser and modify freely.

**Prerequisites:** Load `brutalist/CLAUDE.md` and `brutalist/DESIGN.md` into
your Claude project context before using these prompts. They define the stack,
naming conventions, color system, and typography the figures use.

---

### Figure 5.1 — Comparison 

Create a standalone D3 v7 HTML file for Figure Comparison . Use the CDN https://cdnjs.cloudflare.com/ajax/libs/d3/7.9.0/d3.min.js, inline CSS, ResizeObserver redraw, SVG role="img", aria-labelledby, title, and desc. Build the figure from this structural brief: two-panel comparison — left panel shows a discrete number line with probability "masses" (filled circles) sitting at integers 0 through 5, each labeled with a probability; right panel shows a continuous interval [0,5] with a smooth curve above it and a shaded region between two values, captioned "probability = area"; the visual contrast explains why individual points on the right have zero probability while intervals do not. Use the described data shape and labels; when exact values are not supplied, use plausible illustrative values that preserve the relationships in the brief. Use a zero baseline for bars or areas, direct labels where possible, and annotations named in the brief. Use only DESIGN.md color variables and the required serif/mono font split.

> Reference implementation: `d3/05-continuous-random-variables-fig-01.html`

---

### Figure 5.2 — A smooth density curve f(x) with a shaded

Create a standalone D3 v7 HTML file for Figure A smooth density curve f(x) with a shaded. Use the CDN https://cdnjs.cloudflare.com/ajax/libs/d3/7.9.0/d3.min.js, inline CSS, ResizeObserver redraw, SVG role="img", aria-labelledby, title, and desc. Build the figure from this structural brief: a smooth density curve f(x) with a shaded region between x=c and x=d, labeled "P(c < X < d) = this area"; a separate callout arrow points to the curve height at a single point x=c labeled "f(c) = density, NOT probability — can exceed 1"; helps students immediately internalize that height ≠ probability before they hit the uniform and exponential sections. Use the described data shape and labels; when exact values are not supplied, use plausible illustrative values that preserve the relationships in the brief. Use a zero baseline for bars or areas, direct labels where possible, and annotations named in the brief. Use only DESIGN.md color variables and the required serif/mono font split.

> Reference implementation: `d3/05-continuous-random-variables-fig-02.html`

---

### Figure 5.3 — Uniform distribution rectangle for U(0,12) 

Create a standalone D3 v7 HTML file for Figure Uniform distribution rectangle for U(0,12) . Use the CDN https://cdnjs.cloudflare.com/ajax/libs/d3/7.9.0/d3.min.js, inline CSS, ResizeObserver redraw, SVG role="img", aria-labelledby, title, and desc. Build the figure from this structural brief: uniform distribution rectangle for U(0,12) — horizontal axis labeled "Wait time (minutes)" from 0 to 12, flat horizontal density line at height 1/12 ≈ 0.083, a narrower shaded rectangle from 0 to 3 labeled "P(X < 3) = 3/12 = 0.25"; shows how probability-as-fraction-of-total-base works geometrically. Use the described data shape and labels; when exact values are not supplied, use plausible illustrative values that preserve the relationships in the brief. Use a zero baseline for bars or areas, direct labels where possible, and annotations named in the brief. Use only DESIGN.md color variables and the required serif/mono font split.

> Reference implementation: `d3/05-continuous-random-variables-fig-03.html`

---

### Figure 5.4 — Exponential density curve for Exp(4) 

Create a standalone D3 v7 HTML file for Figure Exponential density curve for Exp(4) . Use the CDN https://cdnjs.cloudflare.com/ajax/libs/d3/7.9.0/d3.min.js, inline CSS, ResizeObserver redraw, SVG role="img", aria-labelledby, title, and desc. Build the figure from this structural brief: exponential density curve for Exp(4) — horizontal axis "Time (minutes)" from 0 to 16, vertical axis "Density f(x)"; curve starts at height 0.25 at x=0 and decays toward zero; shaded region under the curve from 0 to 1 labeled "P(X < 1) ≈ 0.22"; vertical dashed lines at mean=4 and median≈2.77 labeled to show median < mean; student sees the right-skew and understands why long waits inflate the mean. Use the described data shape and labels; when exact values are not supplied, use plausible illustrative values that preserve the relationships in the brief. Use a zero baseline for bars or areas, direct labels where possible, and annotations named in the brief. Use only DESIGN.md color variables and the required serif/mono font split.

> Reference implementation: `d3/05-continuous-random-variables-fig-04.html`

---

### Figure 5.5 — Timeline diagram illustrating the memoryless property 

Create a standalone D3 v7 HTML file for Figure Timeline diagram illustrating the memoryless property . Use the CDN https://cdnjs.cloudflare.com/ajax/libs/d3/7.9.0/d3.min.js, inline CSS, ResizeObserver redraw, SVG role="img", aria-labelledby, title, and desc. Build the figure from this structural brief: timeline diagram illustrating the memoryless property — a horizontal line representing time, with "Last event" marked at left, a bracket labeled "Already waited r=6 min" ending at "Now", and a second bracket extending rightward labeled "Remaining wait t" with a question mark; below it, a second identical timeline starting fresh at "Now", with the same bracket labeled "Fresh wait t from scratch"; caption: "The distribution of remaining wait time is identical regardless of how long you have already waited". Use the described data shape and labels; when exact values are not supplied, use plausible illustrative values that preserve the relationships in the brief. Use a zero baseline for bars or areas, direct labels where possible, and annotations named in the brief. Use only DESIGN.md color variables and the required serif/mono font split.

> Reference implementation: `d3/05-continuous-random-variables-fig-05.html`

---

### Figure 5.6 — A timeline of random events (vertical tick marks

Create a standalone D3 v7 HTML file for Figure A timeline of random events (vertical tick marks. Use the CDN https://cdnjs.cloudflare.com/ajax/libs/d3/7.9.0/d3.min.js, inline CSS, ResizeObserver redraw, SVG role="img", aria-labelledby, title, and desc. Build the figure from this structural brief: a timeline of random events (vertical tick marks at irregular intervals) above two parallel descriptions — top row: "Count events in fixed window → Poisson(λ)" with a bracket spanning several ticks and labeled "n=3 arrivals"; bottom row: "Measure gap between events → Exponential(1/λ)" with a bracket spanning one inter-arrival gap labeled "gap = X"; shows the same process viewed two ways, connecting the discrete chapter to this one. Use the described data shape and labels; when exact values are not supplied, use plausible illustrative values that preserve the relationships in the brief. Use a zero baseline for bars or areas, direct labels where possible, and annotations named in the brief. Use only DESIGN.md color variables and the required serif/mono font split.

> Reference implementation: `d3/05-continuous-random-variables-fig-06.html`
