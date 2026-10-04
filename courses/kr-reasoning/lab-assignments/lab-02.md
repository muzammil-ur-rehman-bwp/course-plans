# Lab Assignment 2 — Normal Forms, Resolution Refutation, and SAT (Graded)

**Weight:** part of the weekly Lab Work component (20% of course grade, averaged across all labs).

## Deliverable
Submit `lab02.ipynb` with working solutions to Tasks A–E from `lab-manuals/lab-02.md`.

## Grading Rubric
| Criterion | Points |
|---|---|
| Task A correct (`resolve` matches hand-computed resolvents, including the empty-list case) | 2 |
| Task B correct (resolution refutation proves a true entailment, trace shown) | 2 |
| Task C correct (correctly fails to prove a false entailment) | 2 |
| Task D correct (SAT checker agrees with the resolution result) | 2 |
| Task E correct (runtimes recorded and discussed sensibly) | 2 |
| **Total** | **10** |

## Notes
Task C and Task D together are weighted to confirm genuine soundness: an implementation that
always returns `True` would pass Task B by accident but must fail both Task C and the consistency
check in Task D.
