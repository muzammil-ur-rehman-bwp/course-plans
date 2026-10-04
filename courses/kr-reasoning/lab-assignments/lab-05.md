# Lab Assignment 5 — A General-Purpose Rule Engine (Graded)

**Weight:** part of the weekly Lab Work component (20% of course grade, averaged across all labs).

## Deliverable
Submit `lab05.ipynb` with working solutions to Tasks A–E from `lab-manuals/lab-05.md`.

## Grading Rubric
| Criterion | Points |
|---|---|
| Task A correct (`Rule`/`is_applicable`) | 1 |
| Task B correct (forward chaining with accurate `derived_by` trace) | 2 |
| Task C correct (backward chaining with working cycle protection) | 2 |
| Task D correct (two domains run on the same unmodified engine) | 3 |
| Task E correct (6 queries, all agree between strategies) | 2 |
| **Total** | **10** |

## Notes
Task D is graded partly by inspection: engine functions that reference any domain-specific
literal (an animal or fault-diagnosis string) lose points here even if both domains happen to
produce correct answers.
