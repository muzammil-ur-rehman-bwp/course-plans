# Lab Assignment 7 — Propositional Logic Inference: Resolution (Graded)

**Weight:** part of the weekly Lab Work component (20% of course grade, averaged across all labs).

## Deliverable
Submit `lab07.ipynb` with working solutions to Tasks A–D from `lab-manuals/lab-07.md`.

## Grading Rubric
| Criterion | Points |
|---|---|
| Task A correct (`resolve` matches hand-computed resolvents) | 2 |
| Task B correct (resolution refutation proves a true entailment) | 3 |
| Task C correct (correctly fails to prove a false entailment) | 3 |
| Task D correct (forward chaining agrees with resolution result) | 2 |
| **Total** | **10** |

## Notes
Task C is weighted the same as Task B deliberately — a resolution implementation that always
returns `True` would pass Task B by accident but must fail Task C, confirming genuine soundness.
