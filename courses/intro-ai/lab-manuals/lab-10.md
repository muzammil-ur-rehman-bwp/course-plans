# Lab Manual 10 — Uncertainty: Bayes' Rule for Diagnostic Testing

**Duration:** 3 hours | **Prerequisite:** Week 10 lecture

## Objectives
Implement Bayes' rule in Python for a medical-diagnostic-test example and explore how the
posterior changes with the prior and test accuracy.

## Setup
Create `lab10.ipynb`; use the `bayes_diagnostic` function from the lecture content as a
starting point.

## Procedure
1. **Task A — Implementation:** implement/paste `bayes_diagnostic(prior, sensitivity,
   false_positive_rate)`; verify it reproduces the lecture's worked example
   (prior=0.01, sensitivity=0.99, false_positive_rate=0.05 → approx. 0.1667).
2. **Task B — Sensitivity analysis:** compute `P(disease | positive test)` for prior values of
   0.001, 0.01, 0.1, and 0.5 (holding sensitivity and false-positive rate fixed); plot or
   tabulate the results and describe the trend.
3. **Task C — Negative test:** derive and implement `bayes_diagnostic_negative(prior,
   sensitivity, false_positive_rate)` computing `P(disease | negative test)`; compare it to the
   positive-test result for the same prior.
4. **Task D — Two independent tests:** given two independent tests with the same sensitivity/
   false-positive rate, compute `P(disease | both positive)` by treating the Task A posterior
   as the new prior for a second application of Bayes' rule; report the result.

## Expected Output
A notebook with Tasks A–D; Task A must reproduce the lecture's numeric result to within
rounding; Task B must include at least 4 prior values.

## Submission
Submit `lab10.ipynb` by the end of the lab session.
