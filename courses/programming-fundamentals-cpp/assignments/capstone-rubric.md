# Capstone Project — Grading Rubric

**Due:** Week 16 | **Weight:** 20% of course grade total (includes proposal, implementation, presentation)

| Component | Weight (of 20%) | Criteria |
|---|---|---|
| Proposal | 2% | Clear problem statement, sound data design (structs/arrays), feasible operations and persistence plan |
| Implementation — correctness | 6% | Code compiles cleanly (`-Wall`); core operations (add/remove/search/update/report) work correctly |
| Implementation — memory & file safety | 5% | Every `new` matched by `delete` (or none used); every file open checked; no out-of-bounds array access |
| Report | 3% | Concise written report: problem, design (structs/arrays used), approach, 1+ limitation/next step |
| Presentation | 4% | Clear 5–7 min talk; demo shown; answers Q&A questions accurately |

## Grading Notes
- A project that honestly reports a feature that didn't fully work, with sound analysis of why,
  can score as well as a project with more features but careless memory/file handling — safety
  and correctness matter as much as feature count.
- Pairs must clearly attribute each member's contribution in the report; grading can differ
  between partners if contributions were significantly uneven.
- Code that compiles only with warnings suppressed, or that crashes on reasonable test input
  (e.g., an empty file on first run), is capped at partial credit on the correctness and safety
  components regardless of feature count.
