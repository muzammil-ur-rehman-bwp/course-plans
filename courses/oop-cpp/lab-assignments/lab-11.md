# Lab Assignment 11 — Class Templates (Graded)

**Weight:** part of the weekly Lab Work component (20% of course grade, averaged across all labs).

## Deliverable
Submit `lab11.cpp` containing working, correct solutions to Tasks A–D from `lab-manuals/lab-11.md`.

## Grading Rubric
| Criterion | Points |
|---|---|
| Task A correct (`Stack<T>` correctly implemented) | 3 |
| Task B correct (two instantiations both work correctly) | 2 |
| Task C correct (`Pair<T, U>` correctly implemented, including `operator==`) | 3 |
| Task D correct (`peekAndPop`, empty case handled reasonably) | 2 |
| **Total** | **10** |

## Notes
- Code must compile cleanly with `g++ -std=c++17 -Wall`.
- `Stack<T>::top()`/`pop()` must not be called on an empty stack anywhere in the submitted code's
  own test driver.
