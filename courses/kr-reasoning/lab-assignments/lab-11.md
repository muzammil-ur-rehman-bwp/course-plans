# Lab Assignment 11 — Allen's Interval Algebra and Path Consistency (Graded)

**Weight:** part of the weekly Lab Work component (20% of course grade, averaged across all labs).

## Deliverable
Submit `lab11.ipynb` with working solutions to Tasks A–D from `lab-manuals/lab-11.md`.

## Grading Rubric
| Criterion | Points |
|---|---|
| Task A correct (relation function correct on ≥8 of the 13 relations tested) | 3 |
| Task B correct (composition table entries correct for the network used) | 2 |
| Task C correct (path consistency correctly tightens a partially-known network) | 3 |
| Task D correct (inconsistency correctly detected via an emptied relation set) | 2 |
| **Total** | **10** |

## Notes
Task A's boundary-condition cases (`meets`/`met-by`, `starts`/`started-by`, `finishes`/
`finished-by`, `equals`) are checked individually in grading, since these are exactly where the
common `<` vs. `<=` pitfall from `lab-notes/lab-11.md` tends to surface.
