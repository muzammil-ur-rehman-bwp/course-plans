# Lab Manual 11 — Bayesian Networks: Inference by Enumeration

**Duration:** 3 hours | **Prerequisite:** Week 11 lecture; Bayes' rule from Lab 10

## Objectives
Implement inference by enumeration on a small hand-built Bayesian network in Python.

## Setup
Create `lab11.ipynb`; use the Burglary/Earthquake/Alarm CPTs from the lecture content as a
starting point.

## Procedure
1. **Task A — Network encoding:** implement/paste the CPT dicts for `Burglary`, `Earthquake`,
   and `Alarm`; implement `joint_prob(b, e, a)` and verify it sums to 1 over all 8
   combinations of `(b, e, a)`.
2. **Task B — Query by enumeration:** implement `p_burglary_given_alarm(alarm_observed)`; report
   `P(Burglary | Alarm=True)` and `P(Burglary | Alarm=False)`.
3. **Task C — Extend the network:** add a 4th node `JohnCalls` with `P(JohnCalls | Alarm)`
   (values provided by the instructor or reasonably chosen); compute `P(Burglary | JohnCalls =
   True)` by enumerating over the hidden variables `Earthquake` and `Alarm`.
4. **Task D — Hand-check:** for Task B's `P(Burglary | Alarm=True)`, show the hand computation
   (the sum-of-products by hand, as in the lecture) in a markdown cell and confirm it matches
   your code's output.

## Expected Output
A notebook with Tasks A–D; the hand computation in Task D must match the code output to within
rounding.

## Submission
Submit `lab11.ipynb` by the end of the lab session.
