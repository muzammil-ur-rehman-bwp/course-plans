# Lab Assignment 10 — Function Templates (Graded)

**Weight:** part of the weekly Lab Work component (20% of course grade, averaged across all labs).

## Deliverable
Submit `lab10.cpp` containing working, correct solutions to Tasks A–D from `lab-manuals/lab-10.md`.

## Grading Rubric
| Criterion | Points |
|---|---|
| Task A correct (`maxOf` works for 3+ types) | 2 |
| Task B correct (`swapValues` genuinely swaps caller's variables, by reference) | 3 |
| Task C correct (deduction failure shown, explicit instantiation fixes it) | 2 |
| Task D correct (`printAll` works for 2+ element types) | 3 |
| **Total** | **10** |

## Notes
- Code must compile cleanly with `g++ -std=c++17 -Wall`.
- `swapValues` parameters must be references; a by-value version that fails to swap caller state
  receives no credit for Task B regardless of compiling.
