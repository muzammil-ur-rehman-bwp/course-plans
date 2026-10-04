# Lab Assignment 4 — Operator Overloading II (Graded)

**Weight:** part of the weekly Lab Work component (20% of course grade, averaged across all labs).

## Deliverable
Submit `lab04.cpp` containing working, correct solutions to Tasks A–D from `lab-manuals/lab-04.md`.

## Grading Rubric
| Criterion | Points |
|---|---|
| Task A correct (`operator<<`, correct format, chainable) | 2 |
| Task B correct (`operator>>`, correct parsing, rejects bad input) | 3 |
| Task C correct (round-trip print/read demonstrated) | 2 |
| Task D correct (`friend`-free version, correct explanation) | 3 |
| **Total** | **10** |

## Notes
- Code must compile cleanly with `g++ -std=c++17 -Wall`.
- Both stream operators must return the stream by reference; failing to do so caps Task A/B/C
  credit even if single calls appear to work.
