# Lab Assignment 7 — Inheritance II (Graded)

**Weight:** part of the weekly Lab Work component (20% of course grade, averaged across all labs).

## Deliverable
Submit `lab07.cpp` containing working, correct solutions to Tasks A–D from `lab-manuals/lab-07.md`.

## Grading Rubric
| Criterion | Points |
|---|---|
| Task A correct (`speak()` overridden, marked `override`) | 2 |
| Task B correct (name hiding demonstrated and correctly explained) | 3 |
| Task C correct (`Duck` multiple inheritance, both methods callable) | 3 |
| Task D correct (diamond problem correctly identified and explained) | 2 |
| **Total** | **10** |

## Notes
- Code must compile cleanly with `g++ -std=c++17 -Wall`.
- `Flyable`/`Swimmable` in Task C must share no common base class (that would recreate the
  diamond problem Task D asks about only conceptually).
