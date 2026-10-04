# Lab Manual 9 — VC Dimension and the Empirical/True Risk Gap

**Duration:** 3 hours | **Prerequisite:** Week 9 lecture

## Objectives
Implement a VC-dimension shattering demonstration for simple hypothesis classes and empirically
illustrate the gap between empirical risk and true risk as sample size grows.

## Setup
Create `lab09.ipynb`.

## Procedure
1. **Task A — Shattering, 1D intervals:** for the hypothesis class of 1D intervals
   $h_{a,b}(x) = \mathbb{1}[a\le x\le b]$, find a set of 2 points it shatters and show (by
   enumeration) that it cannot shatter any set of 3 points; state the resulting VC dimension.
2. **Task B — Shattering, 2D half-planes:** reproduce the lecture's claim that linear threshold
   classifiers in $\mathbb{R}^2$ shatter some 3-point set (show all 8 labelings realized) and
   cannot shatter 4 points (demonstrate one specific labeling of a chosen 4-point set that fails).
3. **Task C — Empirical vs. true risk gap:** reproduce the lecture's `fit_best_halfplane`
   experiment for sample sizes $m\in\{5,10,20,50,200,1000,5000\}$; plot the gap
   (true error $-$ training error) vs. $m$ on a log-x axis and overlay the VC bound's predicted
   $O(1/\sqrt m)$ shape (up to a constant you fit by eye or least squares).
4. **Task D — Capacity vs. parameter count:** build a 2-parameter hypothesis class (e.g.,
   thresholds $h_t(x)=\mathbb{1}[x>t]$, 1 parameter) and a 50-parameter class (e.g., a union of
   50 disjoint intervals) and compute/state the VC dimension of each **by definition**, not by
   counting parameters; report whether the two coincide.

## Expected Output
A notebook with Tasks A–D, including Task C's required plot.

## Submission
Submit `lab09.ipynb` by the end of the lab session.
