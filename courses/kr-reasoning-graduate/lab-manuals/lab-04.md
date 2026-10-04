# Lab Manual 4 — ALC Tableau Satisfiability Checker

**Duration:** 3 hours | **Prerequisite:** Week 4 lecture

## Objectives
Implement a tableau-based ALC satisfiability checker and test it on satisfiable and
unsatisfiable concepts, including one requiring disjunction branching.

## Setup
1. Reuse your course virtual environment.
2. Create `lab04.ipynb`.

## Procedure
1. **Task A — Tableau checker:** implement `tableau_satisfiable` from the Week 4 lecture content.
   Reproduce both worked examples (the clashing C and the satisfiable C′) and confirm the results.
2. **Task B — Branching case:** construct a concept containing a disjunction that forces
   branching to avoid a clash (as in the Week 4 in-class exercise), trace by hand which disjunct
   must be chosen, and confirm with your checker.
3. **Task C — Nested roles:** construct a concept with two levels of role nesting (e.g.,
   ∃hasChild.∃hasChild.Doctor combined with a conflicting ∀ restriction) and determine
   satisfiability by hand and in code.
4. **Task D — Mini-challenge:** for one satisfiable test concept, print out the model your
   checker's clash-free branch implicitly describes (which atomic concepts hold at which node,
   and which role edges connect them).

## Expected Output
A notebook with four clearly labeled cells/sections (A–D), each producing correct, runnable
output.

## Submission
Export/submit `lab04.ipynb` via the course submission system by the end of the lab session.
