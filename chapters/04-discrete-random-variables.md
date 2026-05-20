# Chapter 4 — Discrete Random Variables

Here is a question that sounds simple until you try to answer it precisely: before you flip a coin, what is the "value" of the number of heads you are about to get?

It is not zero. It is not one. It is not one-half, either — you will not get half a head. The experiment has not happened yet. The outcome is genuinely uncertain. And yet we want to *talk* about it, *reason* about it, *calculate* things about it before the coin leaves your hand.

The machinery invented to do this is the random variable. It is not a variable in the algebraic sense — not an unknown you solve for. It is a rule: a function that takes every possible outcome of an uncertain experiment and assigns it a number. Once you have that rule, you can ask mathematical questions about the uncertain future. You can say: what is the probability this number equals three? What is its average value over many repetitions? How spread out around that average should I expect results to be?

This chapter is about the simplest version of that idea: discrete random variables, which land on whole-number values you can list. And from that idea, two extraordinarily useful distributions emerge — the binomial, which governs independent repeated trials, and the Poisson, which governs rare events arriving at random in time. Between them, they describe a remarkable fraction of the counting problems that come up in the real world.

---

## Naming the uncertainty

Let me make the definition concrete. You flip three fair coins. Before any flip happens, define a rule: $X$ equals the number of heads. That rule is the random variable. The sample space — all possible sequences of three flips — has eight elements: TTT, TTH, THT, THH, HTT, HTH, HHT, HHH. The rule $X$ maps each element to a number:

TTT → 0, TTH → 1, THT → 1, THH → 2, HTT → 1, HTH → 2, HHT → 2, HHH → 3.

So $X$ can take the values 0, 1, 2, or 3. Those are the only possibilities. They are whole numbers. You can list them. That is what makes this a *discrete* random variable — discrete in the sense of separated, countable, jumping from one value to the next with nothing in between.

Now you can ask a meaningful question: what is the probability that $X = 2$? Three of the eight equally-likely outcomes produce two heads, so the probability is 3/8.

The collection of all probabilities — one for each possible value — is called the **probability mass function** (PMF). It gives you the complete picture of how the random variable behaves. A valid PMF has to satisfy two requirements: every probability must be between zero and one, and all the probabilities must sum to exactly one, because some outcome must occur.

This sounds almost tautological, but it is worth stating because it gives you a check. If someone hands you a PMF and the numbers do not sum to one, something is wrong. And "wrong" in statistics means your conclusions will be wrong too.

A bakery tracks daily croissant sales and finds: probability 0.30 of selling one batch, 0.44 of selling two, 0.20 of selling three, 0.06 of selling four. Check: $0.30 + 0.44 + 0.20 + 0.06 = 1.00$. The PMF is valid. The baker can reason about tomorrow's sales using these probabilities.

---

## What you expect, and how much you should worry about variation

Once you have a PMF, two numbers summarize almost everything you need to know about a discrete random variable.

The first is the **expected value**, written $E(X)$ or $\mu$. It is the long-run average: if you repeat the experiment very many times and average all the results, the average converges to $E(X)$. The formula is:

$$E(X) = \sum x \cdot P(x)$$

You take each possible value, multiply it by the probability of that value, and add everything up. For the bakery: $E(X) = 1 \cdot 0.30 + 2 \cdot 0.44 + 3 \cdot 0.20 + 4 \cdot 0.06 = 0.30 + 0.88 + 0.60 + 0.24 = 2.02$. The baker expects to sell about two batches per day on average.

Notice that 2.02 is not one of the values the random variable actually takes. The baker will never sell 2.02 batches. Expected value is the balance point of the distribution, not a guaranteed outcome.

The second summary number is the **variance**, written $\text{Var}(X)$ or $\sigma^2$:

$$\text{Var}(X) = \sum (x - \mu)^2 \cdot P(x)$$

And its square root, the **standard deviation** $\sigma$, brings the units back to the original scale — batches of croissants, not batches-squared. Variance measures how spread out the outcomes are around the mean. Large variance means the random variable bounces around a lot; small variance means it stays close to $\mu$.

An emergency room sees patients arriving at a rate that gives the following distribution:

| Arrivals | 10 | 15 | 20 | 25 |
|---|---|---|---|---|
| Probability | 0.20 | 0.35 | 0.30 | 0.15 |

$E(X) = 10(0.20) + 15(0.35) + 20(0.30) + 25(0.15) = 2 + 5.25 + 6 + 3.75 = 17$

$\text{Var}(X) = (10-17)^2(0.20) + (15-17)^2(0.35) + (20-17)^2(0.30) + (25-17)^2(0.15)$

$= 49(0.20) + 4(0.35) + 9(0.30) + 64(0.15) = 9.8 + 1.4 + 2.7 + 9.6 = 23.5$

$\sigma = \sqrt{23.5} \approx 4.85$

The nurse manager knows: expect 17 patients, but plan for the fact that actual arrivals will typically be within 5 of that on either side. The standard deviation is not a curiosity — it is the staffing number.

---

## The binomial: when you repeat the same trial over and over

Now suppose your random variable counts something specific: the number of *successes* in a series of independent, identical trials. This is such a common situation — so common in medicine, quality control, polling, biology — that it gets its own distribution.

A situation is **binomial** when four conditions hold. First, the number of trials is fixed at $n$ — you flip the coin exactly 10 times, test exactly 50 circuit boards, survey exactly 200 voters. Second, each trial has exactly two outcomes, which we call success and failure. Third, the probability of success, $p$, is the same on every trial. Fourth, the trials are independent — what happens on one does not change what happens on the next.

If all four conditions hold, the number of successes $X$ follows a binomial distribution, written $X \sim \text{Binomial}(n, p)$, and the probability of exactly $k$ successes is:

$$P(X = k) = \binom{n}{k} p^k (1-p)^{n-k}$$

The three factors tell a clean story. The term $p^k (1-p)^{n-k}$ is the probability of one specific sequence with $k$ successes and $n-k$ failures — say, the first $k$ trials succeed and the remaining $n-k$ fail. But there are $\binom{n}{k}$ such sequences (that many ways to arrange $k$ successes among $n$ trials), and they are all equally likely, so you multiply. The independence of the trials is exactly what allows you to multiply probabilities like this.

The mean and standard deviation of the binomial follow directly:

$$\mu = np, \qquad \sigma = \sqrt{np(1-p)}$$

These are worth pausing over. The expected number of successes grows linearly with the number of trials — obvious enough. But the standard deviation grows only as $\sqrt{n}$. If you quadruple the number of trials, the expected count quadruples but the spread only doubles. As $n$ grows, the results become relatively more predictable, even as the absolute spread increases. This is the law of large numbers beginning to show its face.

A clinic vaccinated 20 people against the flu. The vaccine is 70% effective. What is the probability that at least 15 of them are protected?

$n = 20$, $p = 0.70$. We need $P(X \geq 15) = P(X=15) + P(X=16) + \cdots + P(X=20)$.

Computing each term using the formula and adding: $P(X \geq 15) \approx 0.416$.

The expected number protected is $\mu = 20 \times 0.70 = 14$, with standard deviation $\sigma = \sqrt{20 \times 0.70 \times 0.30} = \sqrt{4.2} \approx 2.05$. Most outcomes will fall between 12 and 16.

Now consider a quality-control engineer sampling 50 circuit boards from a line where 3% are defective. She wants to know what to expect. $\mu = 50 \times 0.03 = 1.5$ defects, $\sigma \approx 1.21$. On an ordinary day, she sees zero to three defects. If she sees six, she is more than four standard deviations above the mean. That does not happen by luck. Something on the line has changed.

This is the practical power of the distribution: you compute the baseline, you watch the data, and when the data departs too far from baseline, you know to go looking for a cause. You are no longer guessing — you are using a calibrated model of normal variation to detect abnormal variation.

---

## When the binomial conditions fail: the Poisson

The binomial requires a fixed $n$. But many count problems do not have one. Calls arriving at a phone center in the next hour: there is no fixed number of "trials," no moment where the hour pauses and asks whether the next call will arrive. Typos per page, defects per kilometer of fiber-optic cable, goals per soccer match, emergency room arrivals per night shift — in all of these, you fix a window (time, space, length) and count events that arrive randomly within it.

The right distribution for this situation is the **Poisson**.

The Poisson distribution applies when events occur at a constant average rate $\mu$ per interval, independently of each other. The probability of exactly $k$ events in one interval is:

$$P(X = k) = \frac{\mu^k e^{-\mu}}{k!}$$

where $e \approx 2.71828$ is the base of the natural logarithm and $k!$ is $k$ factorial. The remarkable thing about the Poisson is that it has only one parameter: $\mu$, the average rate. Once you know the mean, you know everything about the distribution.

And here is something that does not hold for the binomial: the variance of a Poisson random variable equals its mean.

$$E(X) = \mu, \qquad \text{Var}(X) = \mu, \qquad \sigma = \sqrt{\mu}$$

This equality of mean and variance is a diagnostic. If your count data has variance much larger than the mean, the Poisson is the wrong model — your events are not arriving independently, or the rate is not constant. Statisticians call this overdispersion, and it is a signal that something more interesting is happening in the data.

An emergency room averages 5 patients per hour. What is the probability that exactly 8 arrive in the next hour?

$$P(X = 8) = \frac{5^8 e^{-5}}{8!} = \frac{390{,}625 \times 0.00674}{40{,}320} \approx 0.065$$

About 6.5%. The standard deviation is $\sqrt{5} \approx 2.24$, so the manager expects most hours to see between 3 and 7 arrivals. Staffing for 9 or 10 covers almost all likely scenarios.

What is the probability of 5 or fewer arrivals in that same hour?

$$P(X \leq 5) = \sum_{k=0}^{5} \frac{5^k e^{-5}}{k!} \approx 0.007 + 0.034 + 0.084 + 0.140 + 0.175 + 0.175 \approx 0.616$$

There is a 62% chance the hour stays at or below average. The Poisson distribution puts a precise number on what "typical" means for this ER.

---

## Where the Poisson comes from

There is a beautiful connection between the Poisson and the binomial that clarifies why the Poisson formula looks the way it does.

Imagine you are watching for events in a one-hour window. Divide the hour into $n$ tiny intervals. Each interval is so small that at most one event can occur in it. The probability of an event in any single interval is $p = \mu/n$, where $\mu$ is the average number of events per hour. Across $n$ intervals, you have $n$ independent trials each with probability $p$ — a binomial situation.

Now take the limit as $n \to \infty$ (intervals become infinitesimally small) while holding $\mu = np$ constant. The binomial formula, in this limit, becomes exactly the Poisson formula. The $e^{-\mu}$ emerges naturally from the limit of $(1 - \mu/n)^n$ as $n$ grows without bound. The $\mu^k / k!$ comes from the binomial coefficient and the remaining probability terms.

So the Poisson is not a separate invention. It is the binomial with an infinite number of very unlikely trials. The formula is not arbitrary — it is what the binomial becomes when you push the number of trials to infinity while keeping the expected count fixed.

This also tells you when the Poisson approximates the binomial in practice: when $n$ is large and $p$ is small, so that $np = \mu$ is moderate. A batch of 10,000 items with a defect rate of 0.01% gives a binomial with $n = 10{,}000$ and $p = 0.0001$. Computing that directly is tedious; using Poisson with $\mu = 1$ is accurate and immediate.

---

## Choosing the right model

The practical question is always: which distribution applies here?

The binomial answers when you have a fixed number of trials, each trial binary and independent with the same probability of success. Fixed $n$ is the key. Ten patients in a clinical trial. Fifty boards sampled from the line. Two hundred voters surveyed. The thing being counted is the number of successes among those trials.

The Poisson answers when you have a fixed window — time, space, distance — and events arrive at random within it. No fixed $n$. What is fixed is the window and the rate. Calls per hour. Defects per kilometer. Patients per night shift. The thing being counted is how many events fell into the window.

The distinction sometimes blurs. If you sample 1,000 items from a production line and count defects, that looks binomial ($n = 1000$, $p = $ defect rate). If you count defects on a 100-meter roll of material, the Poisson is more natural — you are not saying each centimeter is a "trial," you are saying defects arrive at some rate along the roll. The physics of the situation tells you which model makes sense.

What both distributions share is independence. The binomial requires that trials do not influence each other. The Poisson requires that events arrive independently — knowing one event occurred does not change the probability of the next one. When independence fails — when events cluster, when contagion operates, when one failure triggers others — both models will fit poorly. The variance will be larger than the model predicts, and you will need something more sophisticated. But for the vast majority of practical count problems, one of these two distributions is the right starting point.

---

## The logic of the whole chapter

A random variable is a number attached to uncertainty. A discrete random variable is one that counts. Once you have a distribution — either empirical (computed from data) or theoretical (a named distribution fitted to the situation) — you know the expected value and variance, and those two numbers let you distinguish normal variation from something that needs an explanation.

The quality-control engineer who knows her baseline (mean 1.5 defects per batch, standard deviation 1.21) can look at a batch with 6 defects and say with confidence: this is not random variation. Something has changed. That statement would be impossible without a model of what normal variation looks like. The distribution provides the model. The expected value and standard deviation calibrate it.

The same logic runs through every application — clinical trials, call centers, public health surveillance, financial risk. In each case, you are asking: given what I know about the process, what should the count look like? And when the actual count differs substantially from what the model predicts, you have learned something real about the world.

---

## LLM Exercise — Chapter 4: Discrete Random Variables (Analyze One Dataset Project)

**Project:** Analyze One Real Dataset.  
**What you're building this chapter:** discrete-distribution analysis of a count variable in your dataset.  
**Tool:** **Claude Code** for fitting; **Claude Project** for the section.

---

**The Prompt:**

```
Chapter 4 of my Analyze-One-Dataset project. Chapter 4 covered:
discrete random variables; pmf; expected value E(X) = Σ x·P(X=x);
variance Var(X) = E[(X−μ)²]; SD = √Var; binomial (n fixed trials,
probability p of "success"); geometric (number of trials to first
success); hypergeometric (sampling without replacement);
Poisson (rare-event count over a fixed interval; mean = variance =
λ).

Write the brief's "Discrete Variable Analysis" section in 400-700
words.

1. **Pick a discrete variable.** From your variable inventory,
   identify one quantitative variable that's discrete (integer
   counts of something — number of goals scored, number of
   children, count of patients, defects per batch, etc.).

2. **Build the empirical pmf.** From your data, compute the
   relative frequency P(X = x) for each observed value of X.
   Plot it as a bar chart.

3. **Compute E(X) and SD(X) from the data.**
   - E(X) = Σ x · P(X=x) — should match the sample mean from
     Chapter 2.
   - Var(X) = Σ (x − E(X))² · P(X=x) — should match the sample
     variance.

4. **Try fitting a named distribution.** Two plausible candidates:
   - **Binomial(n, p)**: if your variable is the count of
     successes in n fixed trials. Estimate p̂ = sample mean / n.
     Compute the binomial pmf P(X=k) = C(n,k) p^k (1-p)^(n-k) for
     each k and compare to the empirical pmf.
   - **Poisson(λ)**: if your variable is count of rare events.
     Estimate λ̂ = sample mean. Compute Poisson pmf P(X=k) =
     e^(-λ) λ^k / k!. Compare to empirical.

   Which fits better? (Often: rare events with no fixed n → Poisson;
   number of successes in n trials → binomial.) Justify.

5. **The check.** For your fitted distribution, does the
   distribution's mean match the sample mean? Does its variance
   match the sample variance? For Poisson specifically, mean
   should equal variance — is that true in your data, or is your
   variable over-dispersed (variance > mean)?

End with a "How well does the named distribution describe my
data" verdict. Honest answer: real data is messier than textbook
distributions. The textbook distribution is a model — useful but
imperfect.
```

---

**What this produces:** A 400–700 word section with empirical pmf + fitted theoretical distribution + a fit-quality assessment. The "honest fit" verdict is the discipline this chapter trains.

**How to adapt this prompt:**

- *For your own project:* If your dataset has no obvious discrete variable, find one — counts often hide in continuous-looking columns (number of customers per day, etc.).
- *For ChatGPT / Gemini:* Works as written.
- *For Claude Code:* `scipy.stats.binom`, `scipy.stats.poisson` for theoretical pmfs.
- *For a Claude Project:* Append.

**Connection to previous chapters:** Ch 2's mean and SD computations get reinterpreted as E(X) and SD(X) here.

**Preview of next chapter:** Chapter 5 covers continuous random variables. You'll identify a continuous variable and try fitting uniform or exponential.

---

## AI Wayback Machine

**Siméon Denis Poisson** was a French mathematician whose 1837 work on the distribution bearing his name underlies modern modeling of rare events.

**Run this:**

```
Who is Siméon Denis Poisson, and how does their work connect to
discrete random variables we covered in this chapter? Keep it to
three paragraphs. End with the single most surprising thing about
their career or ideas.
```

→ Search **"Siméon Denis Poisson"** on Wikipedia.

**Now make the prompt better.** Try one of these:

- Ask it to apply Siméon Denis Poisson's framework to a small dataset you actually have.
- Add a constraint: "Answer including criticisms or limits of Siméon Denis Poisson's framework."

What changes? What gets better? What gets worse?
