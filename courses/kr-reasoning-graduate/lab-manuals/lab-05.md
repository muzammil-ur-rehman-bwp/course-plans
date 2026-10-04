# Lab Manual 5 — Set-of-Support Resolution

**Duration:** 3 hours | **Prerequisite:** Week 5 lecture

## Objectives
Implement set-of-support resolution and compare its search-space size against unrestricted
resolution on the same clause set.

## Setup
1. Reuse your course virtual environment.
2. Create `lab05.ipynb`.

## Procedure
1. **Task A — SOS implementation:** implement `resolve` and `sos_resolution` from the Week 5
   lecture content. Reproduce the §5 in-class exercise (the 3-clause propositional example) and
   confirm it finds a refutation.
2. **Task B — Unrestricted comparison:** implement a plain unrestricted-resolution loop (every
   pair of clauses eligible) on the same clause sets used in Task A, and report how many distinct
   resolvents each approach generates before finding the empty clause.
3. **Task C — Larger instance:** construct a 6–8 clause unsatisfiable set with a clearly
   identifiable negated-goal clause, designate it as the set of support, and compare SOS vs.
   unrestricted resolution's resolvent counts on this larger instance.
4. **Task D — Mini-challenge:** sketch (in a markdown cell) a first-order tableau derivation by
   hand for the Week 5 lecture's {∀x.(P(x)→Q(x)), P(a), ¬Q(a)} example, explicitly marking the
   eigenvariable/fresh-constant steps.

## Expected Output
A notebook with four clearly labeled cells/sections (A–D), Tasks A–C producing correct, runnable
output and Task D a clear hand-worked derivation.

## Submission
Export/submit `lab05.ipynb` via the course submission system by the end of the lab session.
