# Lab Assignment 11 — Computing a PAC-Bayes Bound (Graded)

**Weight:** part of the weekly Lab Work component (10% of course grade, averaged across all labs).

## Deliverable
Submit `lab11.ipynb` containing working, correct solutions to Tasks A–D from
`lab-manuals/lab-11.md`.

## Grading Rubric
| Criterion | Points |
|---|---|
| Task A correct (PAC-Bayes bound computation implemented and reproduced correctly) | 2 |
| Task B correct (posterior-variance sweep run and the bound-minimizing value identified) | 2 |
| Task C correct (SAM-vs-SGD comparison run and correctly connected to the sharpness argument) | 3 |
| Task D correct (sample-size sweep run and the $1/\sqrt n$-type dependence assessed) | 3 |
| **Total** | **10** |

## Notes
- Code must run top-to-bottom without errors in a fresh kernel (Kernel → Restart & Run All).
- Full credit on Task C requires comparing each model's own minimized bound, not bounds at a
  shared, arbitrary `sigma_q2` value.
