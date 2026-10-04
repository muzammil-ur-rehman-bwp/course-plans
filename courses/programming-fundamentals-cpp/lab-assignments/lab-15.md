# Lab Assignment 15 — Debugging Exercise (Graded)

**Weight:** part of the weekly Lab Work component (20% of course grade, averaged across all labs).

## Deliverable
Submit the corrected `buggy.cpp` and a short written bug list from Tasks A–C of
`lab-manuals/lab-15.md`. Task D (capstone work session) is a supervised check-in and is not
separately graded here — capstone progress is graded under `assignments/capstone-rubric.md`.

## Grading Rubric
| Criterion | Points |
|---|---|
| Task A correct (all compiler warnings identified and explained) | 3 |
| Task B correct (all bugs actually fixed; debugger used to confirm at least one) | 4 |
| Task C correct (const-correctness pass applied and still compiles) | 3 |
| **Total** | **10** |

## Notes
- The corrected program must compile cleanly with `g++ -std=c++17 -Wall -Wextra` with zero
  warnings remaining.
- Partial credit given for correctly identified but not-yet-fixed bugs; no credit for a program
  that does not compile at all.
