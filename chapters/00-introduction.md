<!--
00-introduction.md — Book-level introduction.

The Introduction does different work than the Preface:
  - Preface  = why the book exists, why you wrote it (author's voice)
  - Introduction = what the book argues and how it is organized (reader's roadmap)

This file is a stub. Sections 1–10 and 12–13 are placeholders for a later pass.
Section 11 (A note about AI) is substantive and written.

A good model for the full version: Pearl's "The Mind Over Data" introduction,
Molnar's Interpretable ML introduction. Both are argument-first and tell the
reader exactly what to expect from each chapter.
-->

# Introduction

<!-- [1] COLD OPEN
     A specific named scene with real stakes.
     No "this book will...", no throat-clearing.
     Open on a sentence that contains the whole problem.
     Like the Swedish triage case in computational-skepticism-for-ai. -->

[COLD OPEN PLACEHOLDER]

<!-- [2] THE CENTRAL CLAIM — one sentence.
     "This book is about the gap between [X] and [Y]." -->

[CENTRAL CLAIM PLACEHOLDER]

<!-- [3] THE CENTRAL ARGUMENT — a testable, contestable claim
     about what the book is doing. -->

[CENTRAL ARGUMENT PLACEHOLDER]

<!-- [4] AUDIENCE LOCATION — one sentence locating who this is for. -->

[AUDIENCE PLACEHOLDER]

---

## What This Book Is

<!-- [5] Scope. The work the book names. Vocabulary it teaches. -->

[SCOPE PLACEHOLDER]

## What This Book Is Not

<!-- [6] Explicit exclusions. Prerequisites. -->

[EXCLUSIONS PLACEHOLDER]

---

## A Central Concept That Runs Throughout

<!-- [7] A recurring idea readers should watch for across chapters.
     Like "the fluency trap" in computational-skepticism-for-ai. -->

[CENTRAL CONCEPT PLACEHOLDER]

<!-- [8] (OPTIONAL) A RUNNING NARRATIVE THREAD
     A case that recurs across chapters as a worked example.
     Like "Ash" in computational-skepticism-for-ai.
     Delete this section if not using a running thread. -->

## A Running Narrative Thread

[NARRATIVE THREAD PLACEHOLDER — delete this section if not using one]

---

## How This Book Is Organized

<!-- [9] Chapter-by-chapter map. Group into movements (clusters of 3–5)
     if applicable. One sentence per chapter is enough. -->

[CHAPTER MAP PLACEHOLDER]

## How to Read This Book

<!-- [10] Order. Prerequisites for skipping around.
     Self-contained chapters. Chapter-closing features
     (e.g., "What would change my mind", "Still puzzling", exercises). -->

[READING GUIDE PLACEHOLDER]

---

## A Note about AI

Statistics is the field where the gap between knowing what to do and doing it correctly is widest, and where the model's fluency most directly maps to one half of that gap. The note examines what that means for students learning to think statistically.

The model can recite the procedures for any introductory statistical method — descriptive statistics, probability distributions, confidence intervals, hypothesis tests, simple regression, chi-square, ANOVA. It can walk through a worked example. It can explain when each method is appropriate at the level the textbook teaches. The procedural fluency is useful for orientation. What the model handles less well — and what statistics actually requires — is the interpretation: what a confidence interval actually tells you, what a p-value does and does not mean, what to do when the assumptions of a test do not hold.

Where the model genuinely helps: explaining the *purpose* of a method (why we use a t-test rather than a z-test in this situation, what the assumptions of regression are doing), walking through a worked example at the textbook's level of detail, producing alternative framings of confusing concepts (a confidence interval explained three ways), and surfacing the common misinterpretations of statistical results that the textbook is trying to teach students past. The model is also useful as a structural critic of your homework — paste your reasoning and ask where it deviates from the procedure.

Where the model does damage: actually computing values from data, interpreting the practical significance of a result, choosing between methods when the choice depends on details of your data the model has not seen. The model will produce confident wrong arithmetic on simple calculations. It will declare a result "significant" or "not significant" when the question of whether the result *matters* depends on context the model does not have. It will also confabulate sample statistics — give a model a tiny dataset and it will produce summary statistics that have the right form and the wrong values.

A specific failure mode worth naming: the model is particularly fluent in *p-value misinterpretations* because it has been trained on enormous quantities of writing that interprets p-values incorrectly. Even when you ask the model what a p-value means and get the textbook answer, the model's subsequent applications of the concept may carry the misinterpretations the textbook is trying to teach you out of.

The rule that covers all three: concepts and procedures from the model; the actual computation and interpretation from your work. Statistics is taught through the discipline of taking real data and reasoning about it carefully. The model can explain the discipline. The discipline lives in what you do when the data is in front of you and the model is not.

---

## Closing

<!-- [12] Callback to the opening scene. End with a directive. -->

[CLOSING PLACEHOLDER]

---

**Tags:** <!-- [13] 5–8 discoverability tags --> [TAGS PLACEHOLDER]
