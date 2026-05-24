# Chapter 3 — Probability Topics
*The mathematics of what you don't yet know.*

---

There's a puzzle that has been embarrassing doctors for decades.

A woman in her fifties gets a routine mammogram. The radiologist calls back: the test is positive. She's terrified. She asks her doctor: "Given that I tested positive, what is the probability I actually have cancer?"

Her doctor — who is not incompetent, who finished medical school and a residency and sees patients every day — gives her a number around 90%. Because the test is 98% accurate, and she tested positive, so surely it's very likely she has cancer.

The correct answer is closer to 9%.

The doctor confused two probabilities that feel the same but are entirely different things. The test's accuracy — the probability you test positive *given* that you have cancer — is not the same as the probability you have cancer *given* that you tested positive. These are reversed conditional probabilities, and they can differ by a factor of ten.

This chapter is about the machinery that produces the correct answer and the vocabulary needed to understand why the doctor's intuition was so badly wrong.

![Split showing the two reversed conditionals: left panel](images/03-probability-topics-fig-01.png)
*Figure 3.1 — Split showing the two reversed conditionals: left panel*

---

## What probability is

Probability is how we talk precisely about things we don't yet know. It translates *maybe* into a number.

The number lives between 0 and 1. Zero means impossible. One means certain. Half means equally likely to occur or not. The probability of an event is the fraction of all possible equally likely outcomes in which that event occurs.

That definition hides a restriction: it only works directly when outcomes are equally likely. If you roll a fair die, all six faces are equally likely, so the probability of rolling a 3 is 1/6. But if the die is loaded, all six faces are not equally likely, and the formula $1/6$ is wrong. This matters more than most textbooks admit. Many real-world probability problems are badly set up because someone assumed equal likelihood without checking whether the assumption is defensible.

Let me give you the formal structure and then show you how to use it.

An **experiment** is any process with more than one possible outcome. Flipping a coin is an experiment. Rolling a die. Drawing a card. Running a medical test.

The **sample space** is the complete list of all possible outcomes, written $S$. For a coin, $S = \{H, T\}$. For a single die, $S = \{1, 2, 3, 4, 5, 6\}$. The sample space must be exhaustive — every possible outcome must be in it — and each outcome must appear exactly once.

An **event** is any subset of the sample space. "Rolling an even number" is the event $\{2, 4, 6\}$. "Drawing a heart from a deck of cards" is the set of all 13 hearts. Events can be as large as the whole sample space (certain to happen) or as small as a single outcome.

When all outcomes are equally likely:

$$P(A) = \frac{\text{number of outcomes in } A}{\text{total number of outcomes in } S}$$

![Single fair die with all six faces shown](images/03-probability-topics-fig-02.png)
*Figure 3.2 — Single fair die with all six faces shown*

Roll a fair die. The event "rolling at least a 5" contains two outcomes: $\{5, 6\}$. So $P(\text{at least 5}) = 2/6 = 1/3$.

The probability of the complement — the event *not* $A$, written $A'$ — is:

$$P(A') = 1 - P(A)$$

This is more useful than it looks. Sometimes it is much easier to count the outcomes where something does *not* happen and subtract from 1. A bridge player who wants the probability of being dealt at least one ace can either count all the hands with one or more aces (complicated) or subtract from 1 the probability of being dealt no aces (one calculation).

---

## The gambler's fallacy and what probability does not say

A casino roulette wheel lands on red seventeen times in a row. Many gamblers, watching this, feel certain that black is "due." They bet heavily on black.

This is called the gambler's fallacy, and it costs people real money.

The wheel has no memory. It does not know it has landed on red seventeen times. Each spin is independent of every previous spin. The probability of black on the eighteenth spin is the same as it was on the first: roughly $18/38 \approx 0.47$ on an American wheel.

What probability *does* say is this: in the long run, the fraction of spins landing on red approaches the true probability. But "in the long run" says nothing about what happens next. The past outcomes don't pull future outcomes toward some average. They simply are what they are.

This is the hard thing to internalize about probability. It describes populations and long-run frequencies. It says very little about any single event.

![Line chart showing 5 simulated runs of 500](images/03-probability-topics-fig-03.png)
*Figure 3.3 — Line chart showing 5 simulated runs of 500*

---

## Two events: AND and OR

When we want the probability of multiple events, we need to understand how events combine.

**AND: the multiplication rule**

The probability that both event $A$ and event $B$ occur is:

$$P(A \cap B) = P(A) \cdot P(B \mid A)$$

The symbol $\cap$ means "and" — outcomes that are in both sets. The vertical bar means "given that." So $P(B \mid A)$ is the probability that $B$ occurs given that $A$ has already occurred.

Sometimes, knowing $A$ happened doesn't change the odds for $B$ at all. In that case $P(B \mid A) = P(B)$, and the rule simplifies:

$$P(A \cap B) = P(A) \cdot P(B)$$

When this simpler formula holds, we say $A$ and $B$ are **independent**. One event has no effect on the probability of the other.

Roll two fair dice. The result of the first die has no effect on the result of the second die. They are independent. The probability of a 3 on the first and a 5 on the second is $(1/6)(1/6) = 1/36$.

But suppose instead you're drawing cards from a deck without replacement. Draw one card; now draw a second without putting the first back. Is the second draw independent of the first?

No. If the first card was the ace of spades, then the second card cannot be the ace of spades. The sample space has changed — 51 cards remain instead of 52, and the first ace of spades is gone. Knowing what the first card was changes the probabilities for the second draw.

The probability both cards are hearts: the first card is a heart with probability $13/52$. If the first card was a heart, there are now 12 hearts among 51 remaining cards. So the second card is a heart with probability $12/51$.

$$P(\text{both hearts}) = \frac{13}{52} \cdot \frac{12}{51} = \frac{156}{2652} \approx 0.059$$

Compare this to drawing with replacement — putting the first card back before drawing the second. Now the draws are independent:

$$P(\text{both hearts}) = \frac{13}{52} \cdot \frac{13}{52} \approx 0.063$$

The numbers are close but not equal. Whether you replace or not changes the answer. It changes the answer because replacement restores independence; without it, the events are dependent.

**OR: the addition rule**

The probability that at least one of two events occurs — $A$ or $B$ or both — is:

$$P(A \cup B) = P(A) + P(B) - P(A \cap B)$$

The union symbol $\cup$ means "or." Why subtract the intersection? Because $P(A) + P(B)$ counts the outcomes where both occur twice. You need to subtract once to avoid double-counting.

A student applies to two colleges: college A (40% chance of admission) and college B (30% chance). Assuming independence, the probability of getting into at least one is:

$$P(A \cup B) = 0.40 + 0.30 - (0.40)(0.30) = 0.70 - 0.12 = 0.58$$

There's a special case. If $A$ and $B$ are **mutually exclusive** — they share no outcomes, so they cannot both occur — then $P(A \cap B) = 0$, and the addition rule simplifies:

$$P(A \cup B) = P(A) + P(B) \quad \text{(mutually exclusive)}$$

A single card cannot be both a heart and a diamond. "Heart" and "diamond" are mutually exclusive. The probability the card is a heart or a diamond is $13/52 + 13/52 = 26/52 = 1/2$.

---

## The most important distinction: independence vs. mutual exclusivity

Students confuse these constantly. They are not the same thing. They are not even close to the same thing.

**Mutually exclusive** means the events cannot both happen. If $A$ occurs, $B$ definitely doesn't. They share no outcomes.

**Independent** means knowing whether $A$ occurred doesn't change the probability that $B$ occurs. They don't affect each other.

Here is the key: if two events are mutually exclusive and both have positive probability, they cannot be independent. Why? Because if you know $A$ occurred, you immediately know $B$ did *not* occur — so $P(B \mid A) = 0$, which is different from $P(B)$. Knowing $A$ happened changed the probability of $B$ (from positive to zero). That's dependence.

Think of it this way. "Heart" and "diamond" are mutually exclusive — one card can't be both. But they're not independent — if you know the card is a heart, you know it's not a diamond. The events are completely dependent.

"The first die shows a 3" and "the second die shows a 5" are independent — knowing one tells you nothing about the other. They're not mutually exclusive — both could happen on the same roll. And in fact they will happen together with probability $1/36$.

Independent events usually can and do sometimes happen together. Mutually exclusive events never do. Keep these straight.

| Knowing A changes P(B) | Knowing A does not change P(B) |
| --- | --- |
| "Can occur together | Cannot occur together" |
| columns: "Knowing A changes P(B) | Knowing A does not change P(B)". Fill in each cell with an example and the label (Dependent & not ME |
| Independent | A concrete checkpoint for applying the chapter concept. |
| Mutually exclusive = always dependent | A concrete checkpoint for applying the chapter concept. |
| impossible for positive-probability events). Goal: student sees at a glance that the two concepts are orthogonal axes, not synonyms. | A concrete checkpoint for applying the chapter concept. |

---

## Conditional probability

Here is where the mathematics gets genuinely interesting.

The **conditional probability** of $A$ given $B$ is:

$$P(A \mid B) = \frac{P(A \cap B)}{P(B)}$$

Intuitively: you've learned that $B$ occurred. The sample space has been restricted to the outcomes where $B$ is true. You want the fraction of those outcomes that also have $A$ true.

Roll a fair die. You learn the result is even. Given this, what's the probability the result is less than 5?

The event "even" restricts the sample space to $\{2, 4, 6\}$. Of these three equally likely outcomes, two are less than 5: $\{2, 4\}$. So $P(\text{less than 5} \mid \text{even}) = 2/3$.

Using the formula: $P(\text{less than 5 AND even}) = P(\{2, 4\}) = 2/6 = 1/3$. And $P(\text{even}) = 3/6 = 1/2$. So $P(\text{less than 5} \mid \text{even}) = (1/3)/(1/2) = 2/3$. Same answer.

This formula has a rearrangement that is just the multiplication rule again:

$$P(A \cap B) = P(B) \cdot P(A \mid B)$$

The two forms are the same equation written differently. Learn to flip between them.

---

## Bayes' theorem: how evidence changes belief

Now we can resolve the mammogram puzzle.

The doctor's error was confusing $P(\text{positive} \mid \text{cancer})$ with $P(\text{cancer} \mid \text{positive})$. These are different questions. The test's 98% accuracy tells you the first. The patient wants to know the second.

Thomas Bayes, an 18th-century English minister with apparently a lot of time to think, worked out the relationship between these two reversed conditionals. The result is:

$$P(A \mid B) = \frac{P(B \mid A) \cdot P(A)}{P(B)}$$

Let me name the pieces so you can see what each one does.

$P(A)$ is called the **prior** — your belief about $A$ before you learned anything about $B$. Before the test, the woman's probability of having cancer is just the base rate in her population: $P(\text{cancer}) = 0.01$.

$P(B \mid A)$ is the **likelihood** — how probable the evidence is if the hypothesis is true. Here, $P(\text{positive} \mid \text{cancer}) = 0.98$.

$P(B)$ is the **marginal probability** of the evidence — how likely you are to see a positive test result regardless of whether cancer is present. This requires accounting for both possibilities.

$P(A \mid B)$ is the **posterior** — your updated belief after seeing the evidence. This is what the patient needs.

Let me compute $P(\text{positive})$ first, using the **law of total probability**:

$$P(\text{positive}) = P(\text{positive} \mid \text{cancer}) \cdot P(\text{cancer}) + P(\text{positive} \mid \text{no cancer}) \cdot P(\text{no cancer})$$

With a false positive rate of $P(\text{positive} \mid \text{no cancer}) = 0.10$:

$$P(\text{positive}) = (0.98)(0.01) + (0.10)(0.99) = 0.0098 + 0.099 = 0.1088$$

Now apply Bayes:

$$P(\text{cancer} \mid \text{positive}) = \frac{(0.98)(0.01)}{0.1088} = \frac{0.0098}{0.1088} \approx 0.090$$

About 9%.

The test is accurate. But cancer is rare. Only 1% of women in this population have it. When the test produces a positive result, it is far more likely to have flagged one of the 99 cancer-free women falsely than to have correctly identified one of the 1 women who has it. The rare true positives are swamped by the more common false positives.

---

## Making Bayes intuitive: the frequency table

Most people find the formula less clear than counting actual cases. Here's an equivalent approach.

Imagine 10,000 women screened.

1% have cancer: 100 women.
99% don't: 9,900 women.

Of the 100 with cancer: 98% test positive = 98 women. 2 test negative.
Of the 9,900 without cancer: 10% test positive = 990 women. 8,910 test negative.

Total who test positive: $98 + 990 = 1,088$.

Of those 1,088 positive tests, how many actually have cancer? 98.

$$P(\text{cancer} \mid \text{positive}) = \frac{98}{1088} \approx 0.090$$

Same answer. The frequency table forces you to see what you're actually counting — and it makes the result intuitive. Almost all positive tests come from the large pool of cancer-free women. The true positives are a small fraction.

![Icon array of 10,000 women (100×100 grid of](images/03-probability-topics-fig-04.png)
*Figure 3.4 — Icon array of 10,000 women (100×100 grid of*

Notice what happens when you raise the base rate. Suppose cancer affects 10% of this population instead of 1%. Now:

1,000 women have cancer; 9,000 don't.
980 test positive (true positives); 900 false positives.
Total positives: 1,880.
$P(\text{cancer} \mid \text{positive}) = 980/1880 \approx 0.52$.

The same test, on the same kind of woman, gives a very different posterior — because the prior changed. The base rate matters as much as the test's accuracy.

This is the insight Bayes' theorem encodes. The evidence doesn't stand alone. Evidence updates prior beliefs. The strength of the update depends on both the quality of the evidence and where you started.

![Line chart ](images/03-probability-topics-fig-05.png)
*Figure 3.5 — Line chart *

---

## What the drug test gets wrong

Return to the company testing employees for drug use. The test is 95% accurate — 95% sensitivity (catches 95% of users), 95% specificity (clears 95% of non-users). An employee tests positive.

Suppose 5% of employees actually use drugs. Run the frequency table on 10,000 employees.

500 use drugs: 475 test positive (true positives), 25 test negative.
9,500 don't use drugs: 475 test positive (false positives), 9,025 test negative.

Total positives: 950.
Of those, 475 are true positives.

$$P(\text{uses drugs} \mid \text{positive}) = \frac{475}{950} = 0.50$$

A 95%-accurate test, applied to a population where 5% use drugs, gives a coin-flip result on a positive test. There are equal numbers of true positives and false positives.

This is not a flaw in the test. It is a consequence of the base rate. A policy of firing employees who test positive will fire as many innocent people as guilty ones. Whether that is acceptable is not a mathematical question. But the mathematics is necessary before you can make that judgment clearly.

---

## Two tools for organizing complex problems

When a problem involves multiple stages or multiple variables, two tools help keep the calculation straight.

**Contingency tables** organize outcomes in rows and columns. Each cell shows the count of cases with a particular combination of values. Marginal totals give you $P(A)$ and $P(B)$ directly; cell counts give you $P(A \cap B)$; and dividing a cell by its row or column total gives a conditional probability.

**Tree diagrams** organize conditional probabilities in stages. You draw branches at each stage, label them with the probability of that branch given the preceding path, and multiply along a path to get a joint probability. To find a marginal probability, sum across all paths that lead to the same outcome.

Both tools do the same thing: they make the bookkeeping visible. They force you to track the full set of possibilities so you don't accidentally leave something out or double-count something. Which one to use depends on the problem's structure — trees work well for sequential events; tables work well for two categorical variables measured simultaneously.

![Both tools encode the same information; choose based on your problem's structure.](images/03-probability-topics-fig-06.png)
*Figure 3.6 — Comparison *

---

## The structure of all conditional reasoning

Here is what this chapter teaches in the most compressed form I can give it.

There are two directions of conditional probability. From hypothesis to evidence: $P(\text{evidence} \mid \text{hypothesis})$. From evidence to hypothesis: $P(\text{hypothesis} \mid \text{evidence})$. These feel like the same question. They are not.

Scientists and tests and measurements give you the first. You want the second. Bayes' theorem does the translation. And the translation requires the prior — the base rate — which is not in the test itself. It is in the world.

This is why a test that is 98% accurate can give you a result that is only 9% reliable. The accuracy of a test and the reliability of its result are not the same thing. Confusing them is not stupidity. It is a failure of probability training. This chapter is that training.

The mammogram patient who asks "given that I tested positive, how likely am I to have cancer?" is asking the right question. The probability machinery you now have is what lets you answer it — precisely, without pretending to more certainty than the numbers warrant, and without the kind of intuitive error that can send a healthy woman toward unnecessary surgery.

---

## Exercises

### Warm-up

**3.1** *(Sample space and equally likely outcomes.)* A student draws one card at random from a standard 52-card deck. (a) List the sample space for the suit of the card. (b) What is the probability the card is a club? (c) What is the probability the card is not a club?

**3.2** *(Complement rule.)* A fair six-sided die is rolled three times. Rather than counting directly, use the complement: what is the probability that at least one roll shows a 6?

**3.3** *(Identifying independence vs. mutual exclusivity.)* For each pair of events, state whether they are independent, mutually exclusive, both, or neither — and briefly explain why. (a) Rolling a 4 on a die; rolling an even number on the same roll. (b) Flipping heads on coin 1; flipping tails on coin 2. (c) Drawing an ace from a deck; drawing a king on the same single draw.

### Application

**3.4** *(Multiplication rule — dependent events.)* A bag holds 5 red chips and 3 blue chips. You draw two chips without replacement. (a) What is the probability both chips are red? (b) What is the probability the first chip is red and the second is blue?

**3.5** *(Addition rule.)* A software test suite has two independent components: component A fails 8% of the time, component B fails 5% of the time. What is the probability that at least one component fails on a given run?

**3.6** *(Conditional probability.)* In a university's intro statistics course, 55% of students are first-year students and 45% are upperclassmen. Of first-year students, 30% earn an A. Of upperclassmen, 45% earn an A. A randomly selected student earned an A. What is the probability that student is a first-year student?

### Synthesis

**3.7** *(Bayes' theorem — frequency table method.)* A screening test for a genetic condition has 97% sensitivity and 92% specificity. The condition affects 2% of the population. (a) Build a frequency table for 100,000 people screened. (b) What is the probability a person who tests positive actually has the condition? (c) What is the probability a person who tests negative is actually clear?

**3.8** *(Tree diagram.)* A factory has two machines producing the same part. Machine 1 produces 60% of all parts and has a 3% defect rate. Machine 2 produces 40% and has a 5% defect rate. A part is drawn at random and found to be defective. (a) Draw a tree diagram for this problem. (b) What is the probability the defective part came from Machine 1?

### Challenge

**3.9** *(Base rate sensitivity.)* Return to the mammogram scenario: sensitivity 98%, false positive rate 10%. (a) Compute $P(\text{cancer} \mid \text{positive})$ for base rates of 1%, 5%, 10%, and 20%. (b) At what base rate does a positive test become more likely to indicate cancer than not? (c) What does this tell you about the appropriateness of mass screening programs for rare conditions?

**3.10** *(Connecting to design.)* A spam filter is 99% accurate: it correctly flags 99% of spam and correctly passes 99% of legitimate email. (a) If 20% of all email is spam, what fraction of flagged emails are actually spam? (b) If the filter is applied in a corporate setting where only 2% of email is spam, what fraction of flagged emails are legitimate? (c) A colleague argues: "It's 99% accurate, so it's fine." What is wrong with this reasoning, and what additional information does your colleague need?

---

## LLM Exercise — Chapter 3: Probability Topics (Analyze One Dataset Project)

**Project:** Analyze One Real Dataset.
**What you're building this chapter:** the probability section — conditional probabilities, contingency tables, Bayes-style reasoning on real data.
**Tool:** **Claude Code** for the computation; **Claude Project** for the section.

---

**The Prompt:**

```
Chapter 3 of my Analyze-One-Dataset project. Prior sections in
this Claude Project. Chapter 3 covered: sample space and events;
P(A) and P(A^c); the addition rule P(A∪B) = P(A) + P(B) − P(A∩B);
independence P(A∩B) = P(A)·P(B); conditional probability P(B|A) =
P(A∩B)/P(A); tree diagrams and two-way tables; Bayes-style
reasoning (base rate + likelihood → posterior).

Write the brief's "Probability Analysis" section in 400-700 words.

1. **Pick TWO categorical variables from your dataset.** They
   should plausibly be related. Example: in a sports dataset,
   "home/away" and "win/loss"; in a survey dataset, "education
   level" and "political party"; in a medical dataset,
   "smoker/non-smoker" and "disease/no disease."

2. **Build the contingency table.** Use pandas crosstab. The table
   shows joint counts of the two variables.

3. **Compute marginal probabilities.** P(variable_1 = level_A) =
   row total / grand total. Same for variable_2. These are your
   baseline rates.

4. **Compute joint probabilities.** P(A ∩ B) = cell / grand total.

5. **Compute conditional probabilities.** P(B | A) = cell / row
   total. The conditioning is what tells you whether A and B are
   related.

6. **Check independence.** If A and B were independent, P(B | A)
   = P(B). Compare your computed P(B | A) to P(B). Are they
   close? Big differences suggest dependence — which is the
   interesting case. (Chapter 11 will formally test independence
   with chi-square.)

7. **Apply Bayes' thinking.** Pick a question that requires
   inverting: "given event A occurred, how likely is event B?"
   This is often the clinically/practically important question.
   Show the math:
   P(B | A) = P(A | B) · P(B) / P(A).

End with a "What the conditional structure reveals" paragraph.
Often the most interesting finding is the gap between marginal
and conditional probabilities — that gap is the relationship
between the two variables, made visible.
```

---

**What this produces:** A 400-700 word section with a real contingency table, marginal/joint/conditional probabilities, and a Bayesian reframing of the relationship.

**How to adapt this prompt:**

- *For your own project:* The two-variable choice shapes the section. Pick variables where dependence is interesting AND defensible.
- *For ChatGPT / Gemini:* Works as written.
- *For Claude Code:* `pd.crosstab()` is the workhorse. Add `normalize='index'` for conditional probabilities by row.
- *For a Claude Project:* Append.

**Connection to previous chapters:** Chapter 2's descriptive analysis identified the variables; Chapter 3 examines their joint structure.

**Preview of next chapter:** Chapter 4 covers discrete random variables — pmfs, E(X), binomial, Poisson. You'll identify a discrete variable in your dataset and compute its expected value and SD.

---

## AI Wayback Machine

**Andrey Kolmogorov** was a Soviet mathematician who axiomatized probability theory in 1933 — the foundation of all modern statistical reasoning.

**Run this:**

```
Who is Andrey Kolmogorov, and how does their work connect to probability we covered in this chapter? Keep it to three paragraphs. End with the single most surprising thing about their career or ideas.
```

→ Search **"Andrey Kolmogorov"** on Wikipedia.

**Now make the prompt better.** Try one of these:

- Ask it to apply Andrey Kolmogorov's framework to a small dataset you actually have.
- Add a constraint: "Answer including criticisms or limits of Andrey Kolmogorov's framework."

What changes? What gets better? What gets worse?

## Prompts

Use these prompts with Claude to generate interactive D3 v7 versions of the
figures in this chapter. Each produces a standalone HTML file you can open
in a browser and modify freely.

**Prerequisites:** Load `brutalist/CLAUDE.md` and `brutalist/DESIGN.md` into
your Claude project context before using these prompts. They define the stack,
naming conventions, color system, and typography the figures use.

---

### Figure 3.1 — Split showing the two reversed conditionals: left panel

Create a standalone D3 v7 HTML file for Figure Split showing the two reversed conditionals: left panel. Use the CDN https://cdnjs.cloudflare.com/ajax/libs/d3/7.9.0/d3.min.js, inline CSS, ResizeObserver redraw, SVG role="img", aria-labelledby, title, and desc. Build the figure from this structural brief: Two-panel split showing the two reversed conditionals: left panel labeled "What the test measures: P(positive | cancer) = 98%" with arrow from Cancer → Test; right panel labeled "What the patient needs: P(cancer | positive) = ?" with arrow from Test → Cancer. Visual goal: student immediately sees these are different questions pointing in opposite directions.. Use the described data shape and labels; when exact values are not supplied, use plausible illustrative values that preserve the relationships in the brief. Use a zero baseline for bars or areas, direct labels where possible, and annotations named in the brief. Use only DESIGN.md color variables and the required serif/mono font split.

> Reference implementation: `d3/03-probability-topics-fig-01.html`

---

### Figure 3.2 — Single fair die with all six faces shown

Create a standalone D3 v7 HTML file for Figure Single fair die with all six faces shown. Use the CDN https://cdnjs.cloudflare.com/ajax/libs/d3/7.9.0/d3.min.js, inline CSS, ResizeObserver redraw, SVG role="img", aria-labelledby, title, and desc. Build the figure from this structural brief: Single fair die with all six faces shown; two faces (5 and 6) shaded to illustrate the event "rolling at least a 5." Caption should note: shaded outcomes = event A; total faces = sample space S; fraction = probability.. Use the described data shape and labels; when exact values are not supplied, use plausible illustrative values that preserve the relationships in the brief. Use a zero baseline for bars or areas, direct labels where possible, and annotations named in the brief. Use only DESIGN.md color variables and the required serif/mono font split.

> Reference implementation: `d3/03-probability-topics-fig-02.html`

---

### Figure 3.3 — Line chart showing 5 simulated runs of 500

Create a standalone D3 v7 HTML file for Figure Line chart showing 5 simulated runs of 500. Use the CDN https://cdnjs.cloudflare.com/ajax/libs/d3/7.9.0/d3.min.js, inline CSS, ResizeObserver redraw, SVG role="img", aria-labelledby, title, and desc. Build the figure from this structural brief: Line chart showing 5 simulated runs of 500 roulette spins — y-axis is running fraction of "red" outcomes, x-axis is spin number. All lines start erratically (0 or 1) and converge toward 0.474 by spin 500. Student should see: short-run variability is enormous; convergence is real but says nothing about the next spin.. Use the described data shape and labels; when exact values are not supplied, use plausible illustrative values that preserve the relationships in the brief. Use a zero baseline for bars or areas, direct labels where possible, and annotations named in the brief. Use only DESIGN.md color variables and the required serif/mono font split.

> Reference implementation: `d3/03-probability-topics-fig-03.html`

---

### Figure 3.4 — Icon array of 10,000 women (100×100 grid of

Create a standalone D3 v7 HTML file for Figure Icon array of 10,000 women (100×100 grid of. Use the CDN https://cdnjs.cloudflare.com/ajax/libs/d3/7.9.0/d3.min.js, inline CSS, ResizeObserver redraw, SVG role="img", aria-labelledby, title, and desc. Build the figure from this structural brief: Icon array of 10,000 women (100×100 grid of small person icons). 100 icons shaded red = cancer. Of those 100, 98 circled = true positives. 9,900 icons in grey = no cancer. Of those, 990 circled in orange = false positives. Final callout: "Of 1,088 circled women, only 98 have cancer." Visual makes the base-rate swamping viscerally clear.. Use the described data shape and labels; when exact values are not supplied, use plausible illustrative values that preserve the relationships in the brief. Use a zero baseline for bars or areas, direct labels where possible, and annotations named in the brief. Use only DESIGN.md color variables and the required serif/mono font split.

> Reference implementation: `d3/03-probability-topics-fig-04.html`

---

### Figure 3.5 — Line chart 

Create a standalone D3 v7 HTML file for Figure Line chart . Use the CDN https://cdnjs.cloudflare.com/ajax/libs/d3/7.9.0/d3.min.js, inline CSS, ResizeObserver redraw, SVG role="img", aria-labelledby, title, and desc. Build the figure from this structural brief: Line chart — x-axis: base rate (prior) from 0% to 50%; y-axis: P(cancer | positive test). Two curves: one for the 98%/10% test parameters from this chapter, one for a 95%/5% test. Student should see: both curves start near zero at low base rates and climb; the posterior is highly sensitive to the prior, especially at low base rates. The knee of the curve is the key teaching point.. Use the described data shape and labels; when exact values are not supplied, use plausible illustrative values that preserve the relationships in the brief. Use a zero baseline for bars or areas, direct labels where possible, and annotations named in the brief. Use only DESIGN.md color variables and the required serif/mono font split.

> Reference implementation: `d3/03-probability-topics-fig-05.html`

---

### Figure 3.6 — Comparison 

Create a standalone D3 v7 HTML file for Figure Comparison . Use the CDN https://cdnjs.cloudflare.com/ajax/libs/d3/7.9.0/d3.min.js, inline CSS, ResizeObserver redraw, SVG role="img", aria-labelledby, title, and desc. Build the figure from this structural brief: Side-by-side comparison — left: a two-stage tree diagram for the mammogram problem (first branch: cancer/no cancer; second branch from each: positive/negative test), with joint probabilities written at the end of each path and marginal probability computed by summing paths. Right: the equivalent contingency table with the same numbers. Caption: "Both tools encode the same information; choose based on your problem's structure.". Use the described data shape and labels; when exact values are not supplied, use plausible illustrative values that preserve the relationships in the brief. Use a zero baseline for bars or areas, direct labels where possible, and annotations named in the brief. Use only DESIGN.md color variables and the required serif/mono font split.

> Reference implementation: `d3/03-probability-topics-fig-06.html`
