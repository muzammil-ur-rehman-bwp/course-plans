# Lab Assignment 8 — AC-3 and Heuristic Backtracking Search (Graded)

**Weight:** part of the weekly Lab Work component (20% of course grade, averaged across all labs).

## Deliverable
Submit `lab08.ipynb` with working solutions to Tasks A–D from `lab-manuals/lab-08.md`.

## Grading Rubric
| Criterion | Points |
|---|---|
| Task A correct (AC-3 correctly prunes domains, worklist re-queuing correct) | 3 |
| Task B correct (backtracking with MRV/degree/LCV finds a correct map-coloring) | 3 |
| Task C correct (unsatisfiable-despite-arc-consistent case correctly returns `None`) | 2 |
| Task D correct (scheduling CSP correctly formulated and solved) | 2 |
| **Total** | **10** |

## Notes
Task C is weighted specifically to confirm students understand that arc consistency is not
solvability — a submission that cannot produce such a case, or whose AC-3 incorrectly reports it
as inconsistent, has not understood this distinction.
