# Lab Manual 2 — Verifying the Finite-Class PAC Bound

**Duration:** 3 hours | **Prerequisite:** Week 2 lecture

## Objectives
Empirically verify that the derived finite-hypothesis-class sample complexity bound
$m_H(\epsilon,\delta)=\lceil\frac{1}{2\epsilon^2}\ln\frac{|H|}{\delta}\rceil$ delivers at least
its promised success probability.

## Setup
Create `lab02.ipynb`. NumPy only.

## Procedure
1. **Task A — Bound computation:** implement a function computing $m_H(\epsilon,\delta)$ for
   given $|H|,\epsilon,\delta$.
2. **Task B — Simulation:** for $|H|=50$, $\epsilon=0.1$, $\delta=0.05$, simulate 2000 independent
   realizable-learning trials at $m=m_H(\epsilon,\delta)$ as in the lecture content, and record
   the true risk of the hypothesis ERM selects each trial.
3. **Task C — Empirical failure rate:** compute the fraction of trials with true risk $>\epsilon$;
   compare against $\delta$.
4. **Task D — Vary $m$:** repeat Task B/C at $m = 0.5\,m_H$, $m_H$, and $2\,m_H$, and tabulate how
   the empirical failure rate changes.
5. **Task E — Reflection:** explain, in 3–4 sentences, why the empirical failure rate is typically
   far below $\delta$ even at exactly $m=m_H$, referencing the conservativeness of a union bound.

## Expected Output
A notebook with Tasks A–E and a results table for Task D.

## Submission
Submit `lab02.ipynb` by the end of the lab session.
