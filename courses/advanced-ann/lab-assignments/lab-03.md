# Lab Assignment 3 — Mean-Field Signal Propagation (Graded)

**Weight:** part of the weekly Lab Work component (10% of course grade, averaged across all labs).

## Deliverable
Submit `lab03.ipynb` containing working, correct solutions to Tasks A–D from
`lab-manuals/lab-03.md`.

## Grading Rubric
| Criterion | Points |
|---|---|
| Task A correct (variance-propagation recursion implemented and three traces reproduced) | 2 |
| Task B correct (fixed-point behavior verified/quantified for all three cases) | 2 |
| Task C correct (leaky-ReLU fixed-point $\sigma_w^2$ found for ≥3 $\alpha$ values and compared to the derivation) | 3 |
| Task D correct (correlation recursion implemented and plotted, with a reasoned fixed-point conclusion) | 3 |
| **Total** | **10** |

## Notes
- Code must run top-to-bottom without errors in a fresh kernel (Kernel → Restart & Run All).
- Task C is graded on the comparison to the derived formula, not merely on finding *some*
  $\sigma_w^2$ that works numerically.
