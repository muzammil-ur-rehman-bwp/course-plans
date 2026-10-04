# Capstone Project — Grading Rubric

**Due:** Week 16 | **Weight:** 20% of course grade total (includes proposal, implementation, presentation)

| Component | Weight (of 20%) | Criteria |
|---|---|---|
| Proposal | 2% | Clear problem statement, sound hierarchy design, feasible generics/STL and error-handling plan |
| Implementation — OOP correctness | 6% | Correct use of inheritance and polymorphism: `virtual`/`override` used correctly, a `virtual` destructor present on every polymorphic base, no unintended object slicing, abstract base class correctly non-instantiable |
| Implementation — generics, exceptions & memory safety | 6% | At least one template or STL container used correctly; at least one realistic error condition handled via `try`/`catch`/`throw`; no raw owning `new`/`delete` left unmanaged (smart pointers or well-justified RAII) |
| Report | 2% | Concise written report: problem, class design (with a UML sketch), approach, 1+ limitation/next step |
| Presentation | 4% | Clear 5–7 min talk; demo shown; answers Q&A questions accurately, including identifying the specific polymorphism/exception-handling code (per Week 16's in-class Q&A check) |

## Grading Notes
- A project that honestly reports a feature that didn't fully work, with sound analysis of why,
  can score as well as a project with more features but careless resource/exception handling —
  safety and correctness matter as much as feature count.
- A missing `virtual` destructor on a polymorphically-used base class, or object slicing that
  silently discards derived behavior, is treated as a correctness defect, not a style note, and
  is capped accordingly on the OOP-correctness component regardless of how many features the
  program otherwise implements.
- Pairs must clearly attribute each member's contribution in the report; grading can differ
  between partners if contributions were significantly uneven.
- Code that compiles only with warnings suppressed, or that crashes on reasonable test input
  (e.g. an empty container, a withdrawal exceeding balance with no exception thrown), is capped
  at partial credit on the correctness components regardless of feature count.
