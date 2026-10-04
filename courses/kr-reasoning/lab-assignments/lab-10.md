# Lab Assignment 10 — Grounded STRIPS and Partial-Order Planning (Graded)

**Weight:** part of the weekly Lab Work component (20% of course grade, averaged across all labs).

## Deliverable
Submit `lab10.ipynb` with working solutions to Tasks A–D from `lab-manuals/lab-10.md`.

## Grading Rubric
| Criterion | Points |
|---|---|
| Task A correct (grounding produces correct, sensible actions) | 2 |
| Task B correct (partial-order planner builds a valid plan with causal links/orderings) | 3 |
| Task C correct (genuine threat found and correctly resolved) | 3 |
| Task D correct (planning-graph level and mutex pair correct and justified) | 2 |
| **Total** | **10** |

## Notes
Task C requires a threat the planner's own logic discovers, not one hand-asserted in a comment —
submissions that cannot demonstrate `pop_resolve_threats` actually firing will not receive credit
for this task even if the final plan happens to be valid.
