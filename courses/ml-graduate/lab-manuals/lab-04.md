# Lab Manual 4 — Estimating Rademacher Complexity

**Duration:** 3 hours | **Prerequisite:** Week 4 lecture

## Objectives
Estimate empirical Rademacher complexity by Monte Carlo for a norm-bounded linear class and
compare against the closed-form bound.

## Setup
Create `lab04.ipynb`. NumPy only.

## Procedure
1. **Task A — Monte Carlo estimator:** implement `rademacher_estimate_linear` as in the lecture
   content.
2. **Task B — Scaling with $m$:** compute the estimate for $m\in\{10,50,200,1000,5000\}$ on fixed
   dimension $d=5$ data; plot estimate vs. $m$ on a log-log scale.
3. **Task C — Closed-form comparison:** overlay the closed-form bound $BR/\sqrt m$ on the same
   plot (using the sample's typical norm $R$); confirm the empirical estimate tracks the $1/\sqrt
   m$ trend.
4. **Task D — Dimension independence:** repeat Task B at $d=5$ and $d=500$ with $m$ fixed at 200;
   confirm the estimate does not grow substantially with $d$, unlike a VC-dimension-based bound
   would for this class.
5. **Task E — Reflection:** explain, in 3–4 sentences, why this dimension-independence matters
   for high-dimensional linear models.

## Expected Output
A notebook with Tasks A–E and two plots (Task B/C combined, and Task D).

## Submission
Submit `lab04.ipynb` by the end of the lab session.
