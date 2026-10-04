# Lab Assignment 8 — Virtual Functions & Object Slicing (Graded)

**Weight:** part of the weekly Lab Work component (20% of course grade, averaged across all labs).

## Deliverable
Submit `lab08.cpp` containing working, correct solutions to Tasks A–D from `lab-manuals/lab-08.md`.

## Grading Rubric
| Criterion | Points |
|---|---|
| Task A correct (non-virtual behavior observed and explained) | 2 |
| Task B correct (virtual fixes dynamic dispatch, `override` used) | 2 |
| Task C correct (virtual destructor fix correctly demonstrated) | 3 |
| Task D correct (slicing demonstrated, reference-version correct, explanation accurate) | 3 |
| **Total** | **10** |

## Notes
- Code must compile cleanly with `g++ -std=c++17 -Wall`.
- The final submitted `Dog`/`Animal` hierarchy must use a `virtual` destructor; the broken
  version from Task C must be clearly commented out or isolated, not left as the active code.
