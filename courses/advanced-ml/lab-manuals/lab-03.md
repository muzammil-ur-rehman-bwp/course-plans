# Lab Manual 3 — Simulating Sub-Gaussian and Sub-Exponential Concentration

**Duration:** 3 hours | **Prerequisite:** Week 3 lecture

## Objectives
Empirically verify the sub-Gaussian tail bound and Bernstein's two-regime inequality, and confirm
that a sub-Gaussian-style bound is violated for a sub-exponential variable at large $t$.

## Setup
1. Reuse your virtual environment.
2. Create `lab03.ipynb`.

## Procedure
1. **Task A — Implementation:** implement the Rademacher (sub-Gaussian) and centered-$\chi^2_1$
   (sub-exponential) simulations exactly as in the Week 3 lecture content.
2. **Task B — Bound verification table:** for $t\in\{0.1,0.2,\dots,1.5\}$, tabulate the empirical
   tail probability against the sub-Gaussian bound (Rademacher case) and against Bernstein's
   two-regime bound (chi-square case), confirming the empirical value never exceeds the bound.
3. **Task C — Where sub-Gaussian fails:** apply the (incorrect) sub-Gaussian bound $e^{-nt^2/2}$
   to the chi-square case at the same $t$ values, and identify the smallest $t$ at which it is
   violated (empirical > bound).
4. **Task D — Mini-challenge:** vary $n\in\{50,200,800\}$ and confirm the violation threshold
   identified in Task C shifts in a way consistent with Bernstein's crossover point
   $t\approx n\nu^2/\alpha$ (for the chi-square case's parameters $\nu^2=2,\alpha=4$, this is
   where the two-regime bound switches from its quadratic-exponent form to its linear-exponent
   form — explain, in 2–3 sentences, how your observed violation threshold relates to it).

## Expected Output
A notebook with four clearly labeled cells/sections (A–D), each producing correct, runnable
output and the required tables.

## Submission
Export/submit `lab03.ipynb` via the course submission system by the end of the lab session.
