# Lab Manual 6 — Propositional Logic: Truth Tables & Equivalence

**Duration:** 3 hours | **Prerequisite:** Week 6 lecture

## Objectives
Build a propositional-logic truth-table generator/evaluator in Python and use it to check
logical equivalence of two sentences.

## Setup
Create `lab06.ipynb`; use the nested-tuple sentence representation (`('and', a, b)`, etc.) and
`evaluate(expr, model)` function from the lecture content.

## Procedure
1. **Task A — Evaluator:** implement/paste `evaluate(expr, model)` supporting `not`, `and`,
   `or`, `implies`, `iff`; test it on 3 hand-picked sentences against hand-computed expected
   values.
2. **Task B — Truth table generator:** implement `truth_table(expr, symbols)`; generate the
   full truth table for `(P -> Q) <-> (not P or Q)` and confirm every row is `True`.
3. **Task C — Satisfiability/validity checker:** implement `is_valid` and `is_satisfiable`;
   classify 4 given sentences (provided by the instructor) as valid, satisfiable-but-not-valid,
   or unsatisfiable.
4. **Task D — Equivalence checker:** implement `are_equivalent(expr1, expr2, symbols)`; verify
   De Morgan's law `not(P and Q)` ≡ `(not P) or (not Q)` and one additional equivalence of your
   choice.

## Expected Output
A notebook with Tasks A–D; the equivalence checker must correctly confirm De Morgan's law and
correctly reject at least one deliberately non-equivalent pair of sentences.

## Submission
Submit `lab06.ipynb` by the end of the lab session.
