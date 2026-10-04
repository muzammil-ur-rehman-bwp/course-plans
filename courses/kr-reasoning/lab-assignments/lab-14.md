# Lab Assignment 14 — An Integrated Knowledge-Based Agent (Graded)

**Weight:** part of the weekly Lab Work component (20% of course grade, averaged across all labs).

## Deliverable
Submit `lab14.ipynb` with working solutions to Tasks A–D from `lab-manuals/lab-14.md`.

## Grading Rubric
| Criterion | Points |
|---|---|
| Task A correct (unified agent class implemented correctly) | 3 |
| Task B correct (toy domain genuinely exercises both rules and frames) | 2 |
| Task C correct (3 queries agree across forward and backward strategies) | 3 |
| Task D correct (2 full, correct derivation traces) | 2 |
| **Total** | **10** |

## Notes
Task B is checked for a genuine integration test: a domain where every rule's premises could be
satisfied by plain facts alone (never requiring `_expand_taxonomic_facts`) does not meet the
assignment's intent and will be asked to add at least one frame-dependent rule.
