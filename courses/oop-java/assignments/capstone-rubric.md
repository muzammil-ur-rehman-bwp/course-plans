# Capstone Project — Grading Rubric

**Due:** Week 16 | **Weight:** 20% of course grade total (includes proposal, implementation, presentation)

| Component | Weight (of 20%) | Criteria |
|---|---|---|
| Proposal | 2% | Clear problem statement, sound hierarchy/interface design, feasible generics/collections and error-handling plan |
| Implementation — OOP correctness | 6% | Correct use of inheritance and/or interfaces: `@Override` used correctly, no missing abstract-method implementations, correct `equals`/`hashCode` pair if used in a collection key, no raw generic types |
| Implementation — generics, collections & exceptions | 6% | At least one generic type or Collections Framework container used correctly; at least one realistic error condition handled via `try`/`catch`/`throw` with an appropriately checked or unchecked custom exception |
| Report | 2% | Concise written report: problem, class design (with a UML sketch), approach, 1+ limitation/next step |
| Presentation | 4% | Clear 5–7 min talk; demo shown; answers Q&A questions accurately, including identifying the specific inheritance/interface, generics/collections, and exception-handling code (per Week 16's in-class Q&A check) |

## Grading Notes
- A project that honestly reports a feature that didn't fully work, with sound analysis of why,
  can score as well as a project with more features but careless type or exception handling —
  safety and correctness matter as much as feature count.
- A missing `@Override` that silently created an overload, an `equals` override without a
  matching `hashCode`, or a raw generic type left in the submitted code is treated as a
  correctness defect, not a style note, and is capped accordingly on the OOP-correctness
  component regardless of how many features the program otherwise implements.
- Pairs must clearly attribute each member's contribution in the report; grading can differ
  between partners if contributions were significantly uneven.
- Code that compiles only with unchecked-type warnings suppressed, or that crashes on reasonable
  test input (e.g. an empty collection, a withdrawal exceeding balance with no exception thrown),
  is capped at partial credit on the correctness components regardless of feature count.
