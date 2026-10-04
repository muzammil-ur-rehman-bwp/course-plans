# Lab Assignment 6 — Sharpness Estimation and SAM (Graded)

**Weight:** part of the weekly Lab Work component (10% of course grade, averaged across all labs).

## Deliverable
Submit `lab06.ipynb` containing working, correct solutions to Tasks A–D from
`lab-manuals/lab-06.md`.

## Grading Rubric
| Criterion | Points |
|---|---|
| Task A correct (Hessian-vector-product and SAM step implemented correctly) | 2 |
| Task B correct (SGD vs. SAM comparison run and reported for loss and sharpness) | 3 |
| Task C correct (rho sensitivity run across ≥3 values with a discussed tradeoff) | 2 |
| Task D correct (reparameterization check run and correctly interpreted) | 3 |
| **Total** | **10** |

## Notes
- Code must run top-to-bottom without errors in a fresh kernel (Kernel → Restart & Run All).
- Full credit on Task D requires confirming test loss is unchanged under rescaling before
  reporting the sharpness change as meaningful.
