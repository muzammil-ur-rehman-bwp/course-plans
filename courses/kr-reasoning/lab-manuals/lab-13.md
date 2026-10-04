# Lab Manual 13 — Fuzzy Sets, Membership Functions, and Rule Evaluation

**Duration:** 3 hours | **Prerequisite:** Week 13 lecture

## Objectives
Implement fuzzy membership functions and fuzzy set operations; evaluate a small fuzzy rule base.

## Setup
Create `lab13.ipynb`.

## Procedure
1. **Task A — Membership functions:** implement `triangular_membership(x, a, b, c)`; also
   implement `trapezoidal_membership(x, a, b, c, d)` (flat top between `b` and `c`); test both on
   at least 4 input values each, including boundary cases (`x == a`, `x == c`).
2. **Task B — Fuzzy operations:** implement `fuzzy_and`, `fuzzy_or`, `fuzzy_not`; test on 3 pairs
   of membership degrees.
3. **Task C — Rule base:** define at least 3 fuzzy rules for a toy controller of your choosing
   (e.g., temperature/humidity → fan speed, or distance/speed → braking force), each combining 2
   membership degrees with fuzzy AND; implement `evaluate_rule` for each and compute firing
   strengths for 3 different input combinations.
4. **Task D — Discussion:** in a markdown cell, describe in 2–3 sentences one input value that
   has nonzero membership in two different fuzzy sets simultaneously in your domain, and why that
   would be meaningless under classical (crisp) set membership.

## Expected Output
A notebook with Tasks A–D; correct membership functions (including boundary cases), correct
fuzzy operations, and a working 3-rule fuzzy rule base evaluated on 3 inputs.

## Submission
Submit `lab13.ipynb` by the end of the lab session.
