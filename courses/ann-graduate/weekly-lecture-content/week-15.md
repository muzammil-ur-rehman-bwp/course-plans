# Week 15 — Lecture Content: Research Methods and Capstone Work Session

## 1. A Checklist for Critiquing a Theoretical ML Paper
When reading a paper for critique (for this week's assignment, and for the capstone literature
review), extract and write down, explicitly:
1. **The precise claim.** State it as a single, falsifiable sentence. Distinguish a claim about
   an *existence* result (e.g., "a solution with property X exists") from a claim about what
   *actually happens* in practice (e.g., "gradient descent finds such a solution") — Weeks 2 and
   12 (Universal Approximation, NTK) are exactly this distinction in practice.
2. **The assumptions it depends on.** Infinite width? A specific parameterization? A specific
   data distribution? Convexity? Note every assumption that, if false, would break the result.
3. **The strength and kind of evidence.** A proof? A proof under stated assumptions, evaluated
   empirically outside them? A single experiment? Many seeds? One architecture or several?
4. **The limitations, stated by the authors or found by later critique.** What does the paper
   *not* establish, even though a casual read might assume it does?

## 2. Reproducibility in Machine Learning Research
A result that cannot be reproduced is of limited scientific value. Common, well-documented
sources of irreproducibility in ML experiments:
- **Seed variance.** A single run's reported number can be far from the method's typical
  performance; a reproducibility-conscious report gives **multiple random seeds**, with the
  mean *and* the spread (e.g., standard deviation or min–max range), not one number.
- **Undocumented hyperparameters.** Learning rate schedule, weight decay, exact initialization,
  batch size, and number of training steps all materially affect results and must be stated.
- **Hardware/library-version differences.** Floating-point non-determinism, differing default
  settings across library versions, and GPU-vs-CPU numerical differences can all shift results
  slightly; large claimed effects should be robust to these, and reported effect sizes should be
  compared against this kind of incidental variation before being called significant.
- **Baseline fairness.** A reproduced "improvement" must compare against a baseline tuned with
  comparable effort/budget to the proposed method, not an under-tuned strawman.

## 3. Guided In-Class Critique Exercise
Working in pairs, apply the Section 1 checklist to a short provided excerpt (claim, method
summary, and results table) from a theoretical ML paper not previously discussed in this course.
Produce one paragraph: the claim, one assumption it depends on, and one limitation — submitted as
this week's formative check.

## 4. Capstone Work Session Guidance
Use the remaining session time to advance:
- **Literature review**: read and summarize (problem, method, result) each of your 3–5 chosen
  papers; identify the specific gap or question your experiment will address.
- **Experiment/derivation plan**: pin down exactly what you will run or derive, what the baseline
  or comparison point is, and — if your experiment is at all stochastic — how many seeds you will
  run and how you will report spread (per Section 2).
- **Instructor consultation**: bring a specific, concrete question (e.g., "is this experiment
  scoped small enough to finish by Week 16?") rather than an open-ended status update.

This week's lab (`lab-manuals/lab-15.md`) requires applying Section 1's checklist in writing to
your *own* capstone paper(s) and producing a short reproducibility assessment of your *own*
planned experiment, due alongside the Paper Critique & Presentation assignment and the capstone
draft.
