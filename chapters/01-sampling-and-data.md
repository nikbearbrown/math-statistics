# Chapter 1 — Sampling and Data


## TL;DR

- Here is a puzzle that should bother you.
- The chapter moves through The question behind the question, What a good sample looks like, What you are measuring, and why it matters, The deepest distinction: seeing versus doing, and related ideas.
- Read it for the main argument, the vocabulary it introduces, and the practical judgment it asks you to develop.

Here is a puzzle that should bother you.

In October 1936, a magazine called the *Literary Digest* predicted the outcome of the U.S. presidential election. They had been doing this since 1916. Sixteen elections in a row, they got it right. Their method was simple: send out postcards, count the returns, announce the winner. That year they sent ten million postcards. Two million came back. By any reasonable standard, that is an enormous amount of data — more data than almost anyone had ever collected about anything, at that point in history.

They predicted Alf Landon would beat Franklin Roosevelt, 57% to 43%.

Roosevelt won 61% to 39%.

They were not a little wrong. They were catastrophically wrong, in the wrong direction, by eighteen points. The magazine folded within a year, partly from the embarrassment.

Now here is the thing that should bother you: how does that happen? Two million responses. How do you get two million responses and still be that wrong? If you flipped a coin two million times, the fraction of heads you'd get would be so close to 50% that the difference would be microscopic. With two million data points, how do you miss by eighteen percentage points?

The answer to that puzzle is most of what this chapter is about. And the answer is not complicated — but it is the kind of thing that, once you see it, you cannot unsee it. You will start noticing it everywhere, in polls and medical studies and product reviews and news reports, and you will find yourself asking the question that the *Literary Digest* never asked.

---

## The question behind the question

When you do a survey, you are trying to answer a question about a population — everyone, the whole group you care about. The *Literary Digest* wanted to know what all American voters thought. You cannot ask all American voters. There are too many of them. So you ask some of them — a sample — and you hope the sample tells you the truth about the whole.

That hope is the central act of statistics. Everything depends on whether it is justified.

The number you are trying to estimate — the true percentage of the whole population who planned to vote for Landon — that is called a **parameter**. You never know it exactly. You cannot know it. You would have to ask everyone, and that is exactly what you cannot do. So instead you compute a **statistic**: the percentage in your sample who said they would vote for Landon. The statistic is your guess at the parameter. Your best estimate. The thing you report in the newspaper.

The question is: how good is that guess?

Here is the deep insight, the thing the *Literary Digest* missed: the quality of the guess depends almost entirely on *how you chose the sample*, not on how many people are in it. Size matters a little — we will get to that — but method matters overwhelmingly more. A bad method with two million responses is worse than a good method with two thousand.

The *Literary Digest* got their mailing list from three sources: magazine subscriptions, telephone directories, and automobile registration lists. Think about what year this is. 1936. The Great Depression. Unemployment near 25%. Most Americans could not afford a magazine subscription, a telephone, or a car. The people who *could* afford these things — the wealthy — were exactly the people who disliked Roosevelt's New Deal programs and planned to vote for Landon. The poor, the working class, the people who loved Roosevelt: they were not in the mailing list. They had no chance of being included, no matter how many postcards went out.

The sample was not a random slice of the population. It was a slice of the *wealthy* population. And the wealthy population had different opinions than the population as a whole.

This is called **bias**. A sample is biased when it systematically favors certain members of the population over others — when some people have a higher chance of being included than they should, and others have a lower chance. Bias does not go away when you collect more data. It gets amplified. Two million biased responses are worse than ten unbiased ones, in the sense that two million responses make you very confident in a very wrong answer.

---

## What a good sample looks like

So how should you choose a sample?

The right idea is this: every member of the population should have an equal chance of being selected. Not approximately equal — exactly equal, enforced by a random process that does not involve your judgment or convenience.

This is called a **simple random sample**, and it is harder to achieve than it sounds. It requires two things. First, you need a complete list of the entire population — every single member — called the **sampling frame**. Second, you need to choose from that list using a genuinely random mechanism, like a random number generator, not your intuition.

George Gallup, in 1936, did something much closer to this. He selected a small sample — around 50,000 people — but he selected them carefully, making sure the sample matched the demographic composition of the country: right proportions of income levels, regions, urban and rural, and so on. His sample was smaller than the *Literary Digest*'s by a factor of forty. He predicted Roosevelt would win. He was right.

The reason a small random sample beats a large biased one is essentially this: a random sample resembles the population in expectation. If you pick randomly, then on average, the sample will have the same mixture of opinions, ages, incomes, and circumstances as the whole population. No group is systematically excluded. The statistic you compute from a random sample tends to be close to the true parameter — and you can say *how* close, using probability theory. (That comes later.) With a biased sample, you cannot say how close your estimate is to the truth, because you do not know how badly the bias distorts things. You are not just uncertain; you are uncertain about your uncertainty.

There are variations on the basic random sample worth knowing about. **Stratified sampling** divides the population into subgroups — strata — and samples randomly from each one. This is useful when you care about making sure subgroups are represented, or when one subgroup is small and you do not want it to disappear by bad luck. **Cluster sampling** divides the population into clusters — schools, neighborhoods, zip codes — randomly selects some clusters, and then surveys everyone (or a random sample) within those clusters. This is practical when the population is geographically dispersed and visiting every selected person individually would be too expensive.

But all of these methods share the essential property: randomness. The selection is not left to convenience, to who happens to be around, to who answers the phone, or to who feels like responding. Randomness is the protection against bias, because randomness, over many samples, balances out all the differences between groups.

The worst sampling method is **convenience sampling**: you ask whoever is available, whoever walks by, whoever is easy to reach. This is tempting — it is cheap and fast — and it is almost always biased. If you survey students leaving the library on a Tuesday afternoon, you are not sampling all students. You are sampling library-going students on Tuesday afternoons. Those students probably differ from the general student population in ways that could matter a great deal, depending on what you are studying.

---

## What you are measuring, and why it matters

Before you even get to sampling, there is a prior question: what are you measuring?

This sounds obvious, but it is not. The nature of a variable — what kind of thing it is — determines what you can do with the data. Get this wrong and you will do arithmetic that is technically possible but completely meaningless.

Some variables put things into categories. Blood type. Eye color. College major. The name "categorical" or "qualitative" is used for these. The categories have labels, but the labels are just labels. There is no sense in which A-positive blood is "greater than" O-negative. You cannot add blood types. You cannot compute the average blood type of a group of patients. What you *can* do is count: how many patients have each type, what fraction of the sample falls into each category.

Other variables are numbers, and numbers you can actually do arithmetic with. But even among numbers, there is an important distinction.

Some numbers come from counting. Number of children. Number of times you have visited a doctor this year. Number of goals scored in a soccer match. These are **discrete** quantities — they jump from one whole number to the next; there is no such thing as 2.7 children. Other numbers come from measuring. Height. Temperature. Time. These are **continuous** quantities — they can take any value in a range, and the only limit on their precision is the precision of your measuring instrument.

The practical importance of this is that discrete and continuous data behave differently in statistical analysis. But the deeper importance is something else.

There is a further distinction that even many practicing researchers get confused about. Some numerical variables have a true zero — a zero that means *none of the thing*. If your salary is zero dollars, you have no income. If your height is zero inches, you do not exist as a physical object. A value of zero means the absence of the quantity. For variables like this, ratios make sense: someone earning $80,000 earns twice as much as someone earning $40,000.

But some numerical variables do not have a true zero in that sense. Temperature in Celsius is the classic example. Zero degrees Celsius is not the absence of heat — it is the freezing point of water, chosen arbitrarily as a reference. Negative temperatures are perfectly real. You cannot say that 40°C is twice as hot as 20°C (though you can say it is 20 degrees hotter). The ratio is meaningless even though the difference is meaningful.

And then there are variables that have an order but no meaningful distance between values at all. If I rank five movies from best to worst, the difference between rank 1 and rank 2 might be enormous, while the difference between rank 4 and rank 5 might be negligible. The ranking tells you the order but not the gap.

Why does any of this matter? Because certain operations are only valid for certain types of data. You can compute the average score on a 100-point exam and get something meaningful. You can compute the average temperature in Boston in July. But if you compute the average college major — well, what would that even mean? The question is incoherent. And if someone computes the average satisfaction rating on a 1-to-5 scale and reports it to two decimal places, you should be at least a little skeptical: the difference between a 3 and a 4 might not be the same psychological quantity as the difference between a 4 and a 5.

The discipline of asking "what kind of variable is this, and what can I legitimately do with it?" is not pedantry. It is the practice of not fooling yourself.

---

## The deepest distinction: seeing versus doing

Now we get to the most important conceptual divide in all of statistics.

Suppose I notice that people who drink coffee tend to get Parkinson's disease less often than people who do not drink coffee. I collect data on 10,000 people — their coffee habits, their health history — and I find a real, consistent pattern. Coffee drinkers have lower rates of Parkinson's. Is this evidence that coffee prevents Parkinson's?

Maybe. Or maybe not.

The problem is that coffee drinkers and non-coffee drinkers might differ in a dozen other ways. Maybe coffee drinkers are more likely to be employed, or to live in cities, or to exercise more, or to have higher incomes — and maybe one of *those* things is what actually protects against Parkinson's. The coffee correlation is real, but the cause might be hiding somewhere else.

This is called a **confounding variable**: a third variable that influences both the thing you're studying (coffee drinking) and the outcome you're measuring (Parkinson's rates), making it look like there's a direct relationship when there might not be.

You cannot fix this problem by having more data. You cannot fix it by being more careful with your measurements. The problem is inherent in how the data was collected. You went out and *observed* people as they already were — their existing habits, their existing health — without intervening in anything. This is called an **observational study**. Observational studies are enormously useful for finding patterns, generating hypotheses, and understanding what the world looks like. But they cannot prove causation.

To prove causation, you have to do something different. You have to *intervene*. You have to take control of the explanatory variable — the thing you think might be causing the effect — and assign it to people randomly, rather than letting them assign it to themselves.

Here is the logic. Suppose you want to know whether a meditation app reduces test anxiety. You recruit 400 students. You randomly assign 200 of them to use the app and 200 to a control group. "Randomly" means you flip a coin, or use a random number generator — not that you let them choose, not that you put the more anxious students in the treatment group, not that you put your friends in the app group. Truly, blindly, randomly.

After several weeks, you measure anxiety levels. The app group is less anxious.

Now ask: why? The two groups were randomly assigned. Before the intervention, they should have been similar in every way — same distribution of ages, same distribution of initial anxiety, same study habits, same family stresses. The only systematic difference between them, the only thing you *changed*, was whether they used the app. So the difference in anxiety after the intervention must be *caused* by the app (or by the process of using an app — the placebo effect is real, but that is a separate subtlety).

Random assignment is the key. It does not make the two groups identical — by chance, you might get slightly more anxious people in one group than the other. But it makes the groups *balanced in expectation*, meaning that if you repeated the experiment many times, the groups would be similar on average. And it means that any systematic difference you observe after the intervention cannot be blamed on a preexisting difference between the groups, because you randomly shuffled the groups before anyone received any treatment.

This is the **randomized experiment**, and it is the gold standard for causal inference. The pharmaceutical industry, the medical research establishment, and a large fraction of social science are built on this logic.

The contrast is worth saying clearly. An observational study tells you what is true — what patterns exist in the world right now, among people making their own choices. A randomized experiment tells you what *causes* what — what would happen if you intervened, if you changed one thing while holding everything else fixed (as much as possible). These are different questions, and they require different methods.

In practice, randomized experiments are expensive, sometimes impossible, and sometimes unethical. You cannot randomly assign people to smoke cigarettes for twenty years to test whether cigarettes cause cancer. You have to work with observational data — and statisticians have developed clever methods for extracting something close to causal inference from observational data, under certain assumptions. But those methods are careful and difficult, and they require you to already know which confounders to control for. The fundamental advantage of random assignment is that it controls for *all* confounders, including the ones you have not thought of.

---

## The machinery

Let me bring this together.

Every statistical study has the same basic architecture. There is a **population** — the full group you want to make claims about. There is a **parameter** — some true numerical fact about that population, which you do not know and cannot directly measure. There is a **sample** — a subset of the population that you actually observe. And there is a **statistic** — a number computed from your sample, which is your estimate of the parameter.

The relationship between the statistic and the parameter is the whole problem of statistics. Is your estimate close? How close? How confident can you be? Those questions get answered by probability theory, which comes later. But the prerequisite for all of it is that the sample was chosen in a way that makes estimation possible. And that requires a sound method.

A biased method produces a statistic that is systematically different from the parameter — not just randomly off, but consistently off in one direction. No amount of data fixes this. A random method produces a statistic that is randomly off — it might be a little high or a little low, but it tends to cluster around the true value, and the more data you collect, the more tightly it clusters. Size helps when the method is sound. Size does nothing when the method is broken.

The Literary Digest had a broken method. They were measuring what wealthy, magazine-subscribing, telephone-owning Americans thought. They were not measuring what all Americans thought. Their sample, however enormous, told them nothing reliable about the parameter they cared about.

Gallup had a better method. His sample was forty times smaller. He got the right answer.

Method is everything. Before you believe any statistical claim — any poll, any product review, any medical result, any news headline that says "a new study shows" — the first question to ask is: *how was the sample chosen?* If the answer is "we asked whoever responded to our online survey," or "we sent it to our mailing list," or "people who walked into our store," the estimate might be badly biased and the size of the sample tells you nothing reassuring.

Ask for the method. The method is where the truth lives or dies.

---

## LLM Exercise — Chapter 1: Sampling and Data (Analyze One Dataset Project)

**Project:** Analyze One Real Dataset End-to-End — across the semester, pick one publicly-available dataset and apply each chapter's techniques to it. By Ch 13: a 5,000–8,000 word portfolio-quality statistical analysis report.  
**What you're building this chapter:** the foundation — pick your dataset, document its sampling design, classify its variables.  
**Tool:** **Claude Project** "Stats Analysis — [Your Dataset]" — every later chapter adds a section.

---

**The Prompt:**

```
I'm starting a semester-long project to do a complete statistical
analysis of one real public dataset. Each chapter, I'll apply that
chapter's techniques. By Chapter 13 I'll have a portfolio-quality
analysis report.

Help me set up. Ask me ONE question at a time, waiting for my
answer.

1. Pick the dataset. The dataset must be:
   - Real (no synthetic data — that defeats the project).
   - Public (you can share it without violating privacy).
   - At least 100 rows (for hypothesis testing later); 1000+ is
     better.
   - At least 4 variables, mixed categorical + quantitative.
   - About a topic you care about.

   Good sources for ideas:
   - Kaggle (huge variety; pick one with a clear documentation
     story).
   - data.gov (US government data).
   - UCI ML repository.
   - Sports: Lahman Baseball Database; FBref for soccer; NBA API.
   - Climate: NOAA, ECMWF.
   - Health: WHO, CDC datasets, MIMIC (research access).
   - Surveys: ANES, GSS, World Values Survey.
   - Finance: Yahoo Finance, FRED.

2. Why this dataset. One paragraph — what made you pick it. Having
   a stated reason helps you finish.

3. The big question. Pick ONE specific question you most want to
   answer about the dataset by Chapter 13. Examples:
   - "Does the 3-point shot revolution show up in the data?"
   - "Has urban temperature in Boston risen significantly since
     1950?"
   - "Are health outcomes for low-income vs. high-income patients
     different after controlling for [X]?"
   - "Do reviewers rate restaurants with higher prices higher,
     after controlling for cuisine?"

4. The sampling story. Document how the data was collected:
   - What's the population the dataset claims to represent?
   - What was the sampling frame (the list of units that could
     potentially be sampled)?
   - What sampling method was used (SRS, stratified, convenience,
     census)?
   - What sources of bias might be present? (Selection bias,
     non-response bias, response bias, survivorship bias —
     name any that apply.)

5. Variable types. List EVERY variable in the dataset. For each:
   categorical (nominal or ordinal) / quantitative discrete /
   quantitative continuous. This list shapes every later chapter's
   methods.

After all five answers, write a 400–600 word **Project Foundation**
section. Include:
- The dataset's name and source URL.
- The big question (in one sentence).
- The sampling story.
- The variable inventory (a table with name, type, description).
- A "what I'd revise about how this data was collected" note —
  what bias might affect every conclusion the project reaches.

Save the Foundation Document as the first section of the
analysis report.
```

---

**What this produces:** A 400–600 word Project Foundation section. Most students at this stage realize their dataset has 1-2 sampling issues they hadn't noticed. Name them now — the rest of the project lives or dies on how those biases affect later conclusions.

**How to adapt this prompt:**

- *For your own project:* If you have access to a real dataset from your own work or research, use it. The investment in the project pays off when the dataset is one you care about.
- *For ChatGPT / Gemini:* Works as written.
- *For Claude Code:* Useful for downloading and exploring the dataset (`pandas.read_csv`, `df.info()`, `df.describe()`).
- *For a Claude Project:* Essential. The Foundation Document lives in the project context for every later chapter.

**Connection to previous chapters:** This is the project's opening.

**Preview of next chapter:** Chapter 2 covers descriptive statistics. You'll compute mean, median, SD, percentiles, and produce histograms + box plots for the key variables in your dataset.

---

##  AI Wayback Machine
**Florence Nightingale David** was a 20th-century British statistician — Karl Pearson's protégée — whose work on combinatorial probability and survey sampling is still cited.

![Florence Nightingale David](../images/florence-nightingale-david-287.png)

*Puppet Art by [Nik Bear Brown](https://www.nikbearbrown.com/).*

**Run this:**

```
Who is Florence Nightingale David, and how does their work connect
to sampling and data we covered in this chapter? Keep it to three
paragraphs. End with the single most surprising thing about their
career or ideas.
```

→ Search **"Florence Nightingale David"** on Wikipedia.

**Now make the prompt better.** Try one of these:

- Ask it to apply Florence Nightingale David's framework to a small dataset you actually have.
- Add a constraint: "Answer including criticisms or limits of Florence Nightingale David's framework."

What changes? What gets better? What gets worse?
