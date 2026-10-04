# Lab Assignment 10 — Uncertainty: Bayes' Rule for Diagnostic Testing (Graded)

**Weight:** part of the weekly Lab Work component (20% of course grade, averaged across all labs).

## Deliverable
Submit `lab10.ipynb` with working solutions to Tasks A–D from `lab-manuals/lab-10.md`.

## Grading Rubric
| Criterion | Points |
|---|---|
| Task A correct (reproduces lecture's worked-example value) | 2 |
| Task B correct (4-prior sensitivity analysis with correct trend description) | 3 |
| Task C correct (negative-test posterior correctly derived and computed) | 3 |
| Task D correct (two-test chaining correctly uses updated posterior as new prior) | 2 |
| **Total** | **10** |

## Notes
Task D is the most commonly miscoded part: the second application of Bayes' rule must use the
Task A/B posterior as its prior, not the original prior again.
