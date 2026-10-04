# Lab Assignment 15 — Smart Pointers & Debugging (Graded)

**Weight:** part of the weekly Lab Work component (20% of course grade, averaged across all labs).

## Deliverable
Submit `lab15.cpp` containing working, correct solutions to Tasks A–D from `lab-manuals/lab-15.md`.

## Grading Rubric
| Criterion | Points |
|---|---|
| Task A correct (`IntBuffer` converted to `unique_ptr`, no manual destructor/copy ctor) | 3 |
| Task B correct (`shared_ptr` use_count behaves correctly) | 3 |
| Task C correct (all three ownership choices correctly justified) | 2 |
| Task D correct (three meaningful test cases, all passing) | 2 |
| **Total** | **10** |

## Notes
- Code must compile cleanly with `g++ -std=c++17 -Wall`.
- No raw `new`/`delete` may remain in Tasks A or B; any remaining manual memory management caps
  the relevant task's credit.
