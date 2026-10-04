# Lab Assignment 12 — Exception Handling (Graded)

**Weight:** part of the weekly Lab Work component (20% of course grade, averaged across all labs).

## Deliverable
Submit `lab12.cpp` containing working, correct solutions to Tasks A–D from `lab-manuals/lab-12.md`.

## Grading Rubric
| Criterion | Points |
|---|---|
| Task A correct (correct standard exception types thrown) | 2 |
| Task B correct (`try`/`catch` by reference, correct messages printed) | 2 |
| Task C correct (`InsufficientFundsError` correctly implemented and thrown) | 3 |
| Task D correct (three ordered `catch` clauses, each correctly triggered) | 3 |
| **Total** | **10** |

## Notes
- Code must compile cleanly with `g++ -std=c++17 -Wall`.
- All `catch` clauses must catch by (`const`) reference; catching by value caps the relevant
  task's credit even if output happens to look correct.
