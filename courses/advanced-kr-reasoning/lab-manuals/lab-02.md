# Lab Manual 2 — Simply-Typed Lambda Calculus Interpreter

**Duration:** 3 hours | **Prerequisite:** Week 2 lecture

## Objectives
Implement a simply-typed lambda calculus type-checker and β-reducer; use it to verify a
compositional-semantics-style derivation.

## Setup
1. Reuse your Week 1 environment.
2. Create `lab02.ipynb`.

## Procedure
1. **Task A — Implementation:** implement `Base`, `Arrow`, `Var`, `Abs`, `App`, `type_check`,
   `substitute`, and `beta_reduce` exactly as in the Week 2 lecture content.
2. **Task B — Worked example:** reproduce the Week 2 `R x y` worked example; confirm the type is
   `t` and the reduced term is `R a b`.
3. **Task C — Ill-typed detection:** construct a term that applies a function to an
   argument of the wrong type (e.g., applying an `e → t` function to an `e → e` argument) and
   confirm `type_check` raises `TypeError`.
4. **Task D — Compositional-semantics exercise:** translate "Alice introduced Bob to Carol"
   using `introduce : e → e → e → t` as in the Week 2 exercise; type-check and β-reduce each
   intermediate application, printing every intermediate type and term.

## Expected Output
A notebook with four clearly labeled cells/sections (A–D), each producing correct, runnable
output, including the full step-by-step Task D derivation.

## Submission
Export/submit `lab02.ipynb` via the course submission system by the end of the lab session.
