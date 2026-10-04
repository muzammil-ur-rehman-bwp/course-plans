# Lab Assignment 14 — UML Diagramming & SOLID Refactoring (Graded)

**Weight:** part of the weekly Lab Work component (20% of course grade, averaged across all labs).

## Deliverable
Submit `lab14.cpp` and `lab14-diagram` containing working, correct solutions to Tasks A–D from
`lab-manuals/lab-14.md`.

## Grading Rubric
| Criterion | Points |
|---|---|
| Task A correct (accurate UML diagram, correct arrow types) | 3 |
| Task B correct (SRP refactor, genuinely separated responsibilities) | 2 |
| Task C correct (OCP refactor, polymorphism replaces type-string branching) | 3 |
| Task D correct (new shape added with no existing-code changes) | 2 |
| **Total** | **10** |

## Notes
- Code must compile cleanly with `g++ -std=c++17 -Wall`.
- Task C/D credit requires the type-string `if`/`else if` chain to be fully replaced, not merely
  hidden behind another conditional mechanism.
