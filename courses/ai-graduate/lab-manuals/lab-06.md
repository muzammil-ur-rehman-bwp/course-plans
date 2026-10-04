# Lab Manual 6 — DPLL SAT Solver

**Duration:** 3 hours | **Prerequisite:** Week 6 lecture

## Objectives
Implement DPLL with unit propagation from scratch and test it on both satisfiable and
unsatisfiable CNF instances.

## Setup
1. Reuse your course virtual environment.
2. Create `lab06.ipynb`.

## Procedure
1. **Task A — DPLL implementation:** implement `dpll`/`_simplify` from the Week 6 lecture
   content. Test on the worked example (¬A ∨ B) ∧ (A ∨ ¬B ∨ C) ∧ (¬C) and confirm the result.
2. **Task B — Unsatisfiable instance:** construct a small CNF formula that is unsatisfiable
   (e.g., (A) ∧ (¬A)) and a slightly larger one (at least 4 variables) requiring real
   branching to prove UNSAT; confirm your solver reports UNSAT correctly on both.
3. **Task C — Random 3-SAT sweep:** using `random_3sat` from the Week 12 lecture content (you
   may implement a simplified version now), generate instances at clause/variable ratios 2.0,
   4.3, and 6.0 for 15 variables, and report whether your solver found each SAT or UNSAT and how
   long it took.
4. **Task D — Mini-challenge:** add pure-literal elimination to your DPLL implementation (if not
   already included) and measure whether it changes the number of recursive calls made on the
   Task B instances.

## Expected Output
A notebook with four clearly labeled cells/sections (A–D), each producing correct, runnable
output.

## Submission
Export/submit `lab06.ipynb` via the course submission system by the end of the lab session.
