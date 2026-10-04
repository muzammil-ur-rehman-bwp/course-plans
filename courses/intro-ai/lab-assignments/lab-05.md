# Lab Assignment 5 — Adversarial Search: Minimax & Alpha-Beta Pruning (Graded)

**Weight:** part of the weekly Lab Work component (20% of course grade, averaged across all labs).

## Deliverable
Submit `lab05.ipynb` with working solutions to Tasks A–D from `lab-manuals/lab-05.md`.

## Grading Rubric
| Criterion | Points |
|---|---|
| Task A correct (game functions verified on example boards) | 2 |
| Task B correct (minimax with node-count instrumentation) | 3 |
| Task C correct (alpha-beta agrees with minimax on 3+ boards) | 3 |
| Task D correct (nodes-visited comparison with explanation) | 2 |
| **Total** | **10** |

## Notes
Alpha-beta pruning's chosen move must match minimax's on every test board; a discrepancy
indicates a pruning-logic bug and loses Task C credit even if nodes are reduced.
