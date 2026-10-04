# Lab Manual 9 — Two-Stage-Least-Squares IV Estimation and Do-Calculus by Hand

**Duration:** 3 hours | **Prerequisite:** Week 9 lecture

## Objectives
Implement a from-scratch two-stage-least-squares IV estimator on simulated confounded data, and
apply the three do-calculus rules by hand to one identifiable and one non-identifiable graph.

## Setup
1. Reuse your virtual environment.
2. Create `lab09.ipynb` (code portion) and `lab09_docalculus.md` (written portion).

## Procedure
1. **Task A — Implementation:** implement the OLS, 2SLS, and direct covariance-ratio estimators
   exactly as in the Week 9 lecture content.
2. **Task B — Bias comparison:** run the simulation and report the true $\beta$, the biased OLS
   estimate, the 2SLS estimate, and the direct covariance-ratio estimate; confirm the latter two
   agree closely and are much closer to the truth than OLS.
3. **Task C — Do-calculus, identifiable graph:** by hand, apply the three rules to
   $X\leftarrow Z\to Y$, $X\to Y$ to re-derive $P(y\mid\mathrm{do}(x))=\sum_z P(y\mid x,z)P(z)$,
   citing which rule justifies each step (as in the Week 9 lecture content's Worked Example 1).
4. **Task D — Do-calculus, non-identifiable graph:** by hand, attempt the same derivation for
   $X\leftarrow U\to Y$, $X\to Y$ with $U$ unobserved, show explicitly where the attempt fails,
   and state why no sequence of the three rules can succeed here.

## Expected Output
`lab09.ipynb` with Tasks A–B; `lab09_docalculus.md` with Tasks C–D, each step labeled with the
rule applied (or, for Task D, the point of failure).

## Submission
Submit both files via the course submission system by the end of the lab session.
