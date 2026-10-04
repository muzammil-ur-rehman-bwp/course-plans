# Assignment 3 — Generalization Theory (Weeks 9–10)

**Weight:** 5% of course grade (one of 3 problem-set assignments, 15% total) | **Assigned:** Week
10 | **Due:** Start of Week 12

## Instructions
Submit a single Jupyter notebook `assignment03.ipynb` answering all questions below. Show your
work (derivations and/or code) for each question.

## Questions
1. **(VC dimension, 20 pts)** Prove, by exhibiting a shattered set and ruling out shattering any
   larger set, that the VC dimension of axis-aligned rectangles in $\mathbb{R}^2$ (hypotheses of
   the form $\mathbb{1}[a_1\le x_1\le b_1 \text{ and } a_2\le x_2\le b_2]$) is exactly 4. Include
   a diagram of your shattered 4-point set and its 16 realized labelings (or a clear argument why
   all 16 are realizable).
2. **(VC dimension vs. parameter count, 15 pts)** Construct one hypothesis class with exactly 2
   real-valued parameters but VC dimension at least 10 (or argue convincingly that no such class
   exists — you may consult, but must cite, any source used for this argument). Explain why this
   does or does not contradict the intuition that "more parameters means more capacity."
3. **(Empirical risk gap, 20 pts)** Reproduce Week 9's `fit_best_halfplane` risk-gap experiment
   for a *harder* target concept (a non-linear decision boundary, e.g., a circle) fit by the same
   linear hypothesis class; report the training/true error gap vs. $m$ and discuss whether it
   still shrinks, and why the *absolute* errors differ from the linearly-separable case.
4. **(Rademacher complexity, 20 pts)** Compute the empirical Rademacher complexity estimate (per
   Week 10's method) for linear hypotheses with $\|w\|\le 1$ on two different datasets of the same
   size $m=50$: one with features drawn from $\mathcal{N}(0, 1)$, one with features drawn from
   $\mathcal{N}(0, 9)$ (3x the standard deviation). Report both estimates and explain the
   difference using the Rademacher complexity's definition.
5. **(Random labels, margin, and generalization, 25 pts)** Using the Week 10 MLP setup, run the
   real-labels and random-labels experiments for 3 different hidden widths
   ($\{8, 64, 512\}$). For each width and condition, report training accuracy, test accuracy
   (against the true concept), and mean margin. Write a short (6–8 sentence) synthesis connecting
   your 6 data points to the lecture's claim that capacity alone does not determine
   generalization.

## Submission
Upload `assignment03.ipynb` via the course submission system. Late policy per syllabus
(`course-plan.md` §7).
