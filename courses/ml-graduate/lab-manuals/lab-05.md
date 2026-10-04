# Lab Manual 5 — Hoeffding's Inequality: Verification and Assumption Failure

**Duration:** 3 hours | **Prerequisite:** Week 5 lecture

## Objectives
Verify Hoeffding's inequality empirically for i.i.d. bounded variables, and observe its guarantee
breaking down when the independence assumption is violated.

## Setup
Create `lab05.ipynb`. NumPy, SciPy.

## Procedure
1. **Task A — i.i.d. verification:** implement the coin-flip simulation from the lecture content
   for $m=50$, $\epsilon=0.1$; compute the empirical tail probability and the Hoeffding bound.
2. **Task B — Vary $m$:** repeat Task A for $m\in\{10,50,200,1000\}$ and tabulate how both the
   empirical tail probability and the bound shrink.
3. **Task C — Correlated sequence:** implement the correlated (random-walk-like) sequence from
   the lecture content; compute its empirical tail probability at the same $m,\epsilon$.
4. **Task D — Compare:** plot the i.i.d. and correlated empirical tail probabilities alongside the
   (nominal, no-longer-guaranteed) Hoeffding bound on one chart.
5. **Task E — Reflection:** explain, in 3–4 sentences, exactly which step of Hoeffding's proof
   (Week 5, Section 5) breaks when the data is correlated.

## Expected Output
A notebook with Tasks A–E and the comparison plot from Task D.

## Submission
Submit `lab05.ipynb` by the end of the lab session.
