# Lab Assignment 13 — Fuzzy Sets, Membership Functions, and Rule Evaluation (Graded)

**Weight:** part of the weekly Lab Work component (20% of course grade, averaged across all labs).

## Deliverable
Submit `lab13.ipynb` with working solutions to Tasks A–D from `lab-manuals/lab-13.md`.

## Grading Rubric
| Criterion | Points |
|---|---|
| Task A correct (both membership functions correct, including boundary cases) | 3 |
| Task B correct (fuzzy AND/OR/NOT correct on all 3 test pairs) | 2 |
| Task C correct (3-rule fuzzy rule base evaluated correctly on 3 inputs) | 3 |
| Task D correct (clear, correct discussion of non-crisp dual membership) | 2 |
| **Total** | **10** |

## Notes
Task A's boundary-case tests are individually checked: a correct-looking membership function
that returns a slightly wrong value exactly at `x == a`, `x == b`, or `x == c` will lose points
specifically there, even if the general shape is otherwise right.
