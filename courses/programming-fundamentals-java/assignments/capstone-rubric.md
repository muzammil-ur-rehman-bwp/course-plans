# Capstone Project — Grading Rubric

**Due:** Week 16 | **Weight:** 20% of course grade total (includes proposal, implementation, presentation)

| Component | Weight (of 20%) | Criteria |
|---|---|---|
| Proposal | 2% | Clear problem statement, sound data design (classes/arrays/`ArrayList`), feasible operations and persistence plan |
| Implementation — correctness | 6% | Code compiles cleanly; core operations (add/remove/search/update/report) work correctly |
| Implementation — safety & design | 5% | No uncaught `NullPointerException`/`ArrayIndexOutOfBoundsException` on reasonable input; every file open/read checked; private fields properly encapsulated |
| Report | 3% | Concise written report: problem, design (classes/arrays/`ArrayList` used), approach, 1+ limitation/next step |
| Presentation | 4% | Clear 5–7 min talk; demo shown; answers Q&A questions accurately |

## Grading Notes
- A project that honestly reports a feature that didn't fully work, with sound analysis of why,
  can score as well as a project with more features but careless exception/file handling —
  safety and correctness matter as much as feature count.
- Pairs must clearly attribute each member's contribution in the report; grading can differ
  between partners if contributions were significantly uneven.
- Code that crashes with an uncaught exception on reasonable test input (e.g., an empty or
  missing data file on first run), or that leaves fields `public` with no validation at all, is
  capped at partial credit on the correctness and safety/design components regardless of feature
  count.
