# Lab Assignment 2 — Constructors & Destructors (Graded)

**Weight:** part of the weekly Lab Work component (20% of course grade, averaged across all labs).

## Deliverable
Submit `lab02.cpp` containing working, correct solutions to Tasks A–D from `lab-manuals/lab-02.md`.

## Grading Rubric
| Criterion | Points |
|---|---|
| Task A correct (`Point3D` constructors with initializer lists) | 2 |
| Task B correct (`IntBuffer` construction/destruction, no leaks) | 2 |
| Task C correct (deep-copying copy constructor, correct explanation) | 3 |
| Task D correct (`makeFilledBuffer`, correct copy/return behavior) | 3 |
| **Total** | **10** |

## Notes
- Code must compile cleanly with `g++ -std=c++17 -Wall`.
- Any double-free, leak, or shallow-copy bug in `IntBuffer` caps Task C/D credit regardless of
  otherwise-correct logic.
