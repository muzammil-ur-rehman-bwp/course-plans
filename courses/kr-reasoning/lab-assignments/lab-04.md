# Lab Assignment 4 — Unification and First-Order Resolution (Graded)

**Weight:** part of the weekly Lab Work component (20% of course grade, averaged across all labs).

## Deliverable
Submit `lab04.ipynb` with working solutions to Tasks A–D from `lab-manuals/lab-04.md`.

## Grading Rubric
| Criterion | Points |
|---|---|
| Task A correct (term helpers work on compound and atomic terms) | 2 |
| Task B correct (unification correct on all 5+ pairs, including both failure cases) | 3 |
| Task C correct (`standardize_apart` and `fol_resolve` correct on a non-trivial pair) | 3 |
| Task D correct (both Skolemizations correct) | 2 |
| **Total** | **10** |

## Notes
Task B's occurs-check failure case is weighted deliberately: an implementation that unifies `x`
with `f(x)` "successfully" has a latent infinite-term bug and cannot earn full marks on Task B
even if every other test passes.
