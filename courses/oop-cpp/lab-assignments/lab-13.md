# Lab Assignment 13 — Introduction to the STL (Graded)

**Weight:** part of the weekly Lab Work component (20% of course grade, averaged across all labs).

## Deliverable
Submit `lab13.cpp` containing working, correct solutions to Tasks A–D from `lab-manuals/lab-13.md`.

## Grading Rubric
| Criterion | Points |
|---|---|
| Task A correct (`std::vector` sum/average correct) | 2 |
| Task B correct (`std::map` word tally correct) | 2 |
| Task C correct (both iteration styles, matching output) | 3 |
| Task D correct (`sumAll` template, iterator-based, works for 2+ types) | 3 |
| **Total** | **10** |

## Notes
- Code must compile cleanly with `g++ -std=c++17 -Wall`.
- Task D's loop must use iterators (`begin()`/`end()`) or range-`for`, not `[]` indexing, to
  receive full credit.
