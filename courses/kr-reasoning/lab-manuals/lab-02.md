# Lab Manual 2 — Normal Forms, Resolution Refutation, and SAT

**Duration:** 3 hours | **Prerequisite:** Week 2 lecture

## Objectives
Implement CNF/DNF conversion, resolution refutation, and a brute-force SAT checker for small
propositional knowledge bases.

## Setup
Create `lab02.ipynb`; represent clauses as Python sets of literal strings (e.g., `{"P", "~Q"}`).

## Procedure
1. **Task A — Resolve step:** implement `resolve(clause1, clause2)`; test it on 3 hand-picked
   clause pairs against hand-computed expected resolvents, including one pair with no
   complementary literals (expect an empty list of resolvents).
2. **Task B — Resolution refutation:** implement `resolution_refutation(clauses, query_literal)`;
   verify it proves a query that should follow from a 4–5-clause KB, tracing each resolvent
   derived.
3. **Task C — Negative control:** run your implementation on a query that should **not** follow
   from the same KB; confirm it correctly returns `False`.
4. **Task D — SAT checker:** implement `is_satisfiable(clauses, symbols)` by brute-force model
   enumeration; use it to independently check the satisfiability of the KB-plus-negated-query
   clause set from Task B and confirm it agrees with your resolution result (unsatisfiable exactly
   when resolution proves the query).
5. **Task E — Scaling discussion:** time `is_satisfiable` on clause sets over 8, 12, and 16
   symbols (randomly generated 3-literal clauses); record the runtimes and discuss, in 2–3
   sentences, how they relate to SAT's NP-completeness.

## Expected Output
A notebook with Tasks A–E; correct resolution and SAT results that agree with each other on the
same clause sets, and a short runtime-scaling discussion.

## Submission
Submit `lab02.ipynb` by the end of the lab session.
