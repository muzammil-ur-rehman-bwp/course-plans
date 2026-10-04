# Lab Assignment 9 — Abstract Classes (Graded)

**Weight:** part of the weekly Lab Work component (20% of course grade, averaged across all labs).

## Deliverable
Submit `lab09.cpp` containing working, correct solutions to Tasks A–D from `lab-manuals/lab-09.md`.

## Grading Rubric
| Criterion | Points |
|---|---|
| Task A correct (`Shape` abstract, confirmed non-instantiable) | 2 |
| Task B correct (`Circle`/`Rectangle` concrete, correct `area()`) | 3 |
| Task C correct (polymorphic use via `Shape*`, no leaks) | 3 |
| Task D correct (default `describe()` using `area()` polymorphically) | 2 |
| **Total** | **10** |

## Notes
- Code must compile cleanly with `g++ -std=c++17 -Wall`.
- `Shape` must include a `virtual` destructor; every `new`-allocated `Shape` in Task C must have
  a matching `delete`.
