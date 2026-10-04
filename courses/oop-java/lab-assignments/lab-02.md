# Lab Assignment 2 — Constructors & the `Object` Contract (Graded)

**Weight:** part of the weekly Lab Work component (20% of course grade, averaged across all labs).

## Deliverable
Submit your `.java` files containing working, correct solutions to Tasks A–D from
`lab-manuals/lab-02.md`.

## Grading Rubric
| Criterion | Points |
|---|---|
| Task A correct (three constructors, correct `this(...)` chaining) | 3 |
| Task B correct (`toString()` format) | 1 |
| Task C correct (`equals`/`hashCode`, `@Override`, correct signature) | 4 |
| Task D correct (bug demonstrated, then properly fixed) | 2 |
| **Total** | **10** |

## Notes
- Code must compile cleanly with `javac`.
- `equals` must have the exact signature `equals(Object obj)`; `hashCode()` must be built from the
  same fields `equals()` compares.
