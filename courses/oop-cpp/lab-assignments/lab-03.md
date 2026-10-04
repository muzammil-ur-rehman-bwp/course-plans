# Lab Assignment 3 — Operator Overloading I (Graded)

**Weight:** part of the weekly Lab Work component (20% of course grade, averaged across all labs).

## Deliverable
Submit `lab03.cpp` containing working, correct solutions to Tasks A–D from `lab-manuals/lab-03.md`.

## Grading Rubric
| Criterion | Points |
|---|---|
| Task A correct (`operator+`/`operator-`, non-mutating, correct results) | 3 |
| Task B correct (`operator+=` mutating correctly; `operator+` delegating) | 2 |
| Task C correct (`operator==`/`operator<`, mutually consistent) | 3 |
| Task D correct (`maxFraction`, correct on equal-value edge cases) | 2 |
| **Total** | **10** |

## Notes
- Code must compile cleanly with `g++ -std=c++17 -Wall`.
- `operator+`/`operator-` must not mutate either operand; violating this caps Task A/B credit.
