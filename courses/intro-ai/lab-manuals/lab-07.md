# Lab Manual 7 — Propositional Logic Inference: Resolution

**Duration:** 3 hours | **Prerequisite:** Week 7 lecture; evaluator from Lab 6

## Objectives
Implement resolution refutation for a small propositional knowledge base.

## Setup
Create `lab07.ipynb`; represent clauses as Python sets of literal strings (e.g., `{"P", "~Q"}`).

## Procedure
1. **Task A — Resolve step:** implement `resolve(clause1, clause2)` returning all resolvents of
   two clauses; test it on 2–3 hand-picked clause pairs against hand-computed expected results.
2. **Task B — Resolution refutation:** implement `resolution_refutation(clauses, query_literal)`;
   verify it correctly proves a query that should follow from a small 3–4 clause KB.
3. **Task C — Negative control:** run your implementation on a query that should **not** follow
   from the same KB, and confirm it correctly returns `False` (does not derive the empty clause).
4. **Task D — Forward chaining cross-check:** for a Horn-clause version of the same KB,
   implement `forward_chain(facts, rules, query)` and confirm it agrees with your resolution
   result.

## Expected Output
A notebook with Tasks A–D; resolution must correctly prove at least one true entailment and
correctly fail to prove at least one false one, with forward chaining agreeing on the Horn-
clause case.

## Submission
Submit `lab07.ipynb` by the end of the lab session.
