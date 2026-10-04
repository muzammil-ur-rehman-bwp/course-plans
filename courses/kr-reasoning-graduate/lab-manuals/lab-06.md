# Lab Manual 6 — Stable-Model Checker and Graph Coloring

**Duration:** 3 hours | **Prerequisite:** Week 6 lecture

## Objectives
Implement a brute-force stable-model checker and use it to solve a small graph-coloring
instance encoded as a normal logic program.

## Setup
1. Reuse your course virtual environment.
2. Create `lab06.ipynb`.

## Procedure
1. **Task A — Stable-model checker:** implement `gl_reduct`, `least_model`, `is_stable_model`,
   and `find_stable_models` from the Week 6 lecture content. Reproduce the §3 worked example
   ({p :- not q. q :- not p.}) and confirm the two stable models {p} and {q} are found.
2. **Task B — Constraint check:** add an integrity constraint to the §3 program (e.g.,
   `:- p, q.`, trivially already impossible here) and construct a *new* small program where an
   integrity constraint genuinely eliminates one of several candidate stable models; confirm your
   checker reflects this.
3. **Task C — Graph coloring:** encode a 4-vertex, 2-color instance (with at least one edge
   forcing adjacent vertices to differ) as `assign_V_C` atoms with mutual-exclusion and
   at-least-one-color constraints; enumerate all valid colorings via `find_stable_models`.
4. **Task D — Mini-challenge:** write the equivalent `clingo`-syntax ASP program for the Task C
   instance (as a markdown/text cell, not required to run) and compare its choice-rule idiom to
   your Python constraint encoding.

## Expected Output
A notebook with four clearly labeled cells/sections (A–D), each producing correct, runnable
output (A–C) or a clear written comparison (D).

## Submission
Export/submit `lab06.ipynb` via the course submission system by the end of the lab session.
