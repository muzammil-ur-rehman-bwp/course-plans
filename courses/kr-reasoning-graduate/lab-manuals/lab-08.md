# Lab Manual 8 — Grounded and Preferred Extension Calculator

**Duration:** 3 hours | **Prerequisite:** Week 8 lecture

## Objectives
Implement grounded- and preferred-extension computation for small Dung argumentation frameworks,
including a case where the two semantics diverge.

## Setup
1. Reuse your course virtual environment.
2. Create `lab08.ipynb`.

## Procedure
1. **Task A — Grounded extension:** implement `characteristic_function` and
   `grounded_extension` from the Week 8 lecture content. Reproduce the §3 chain example
   (A={a,b,c}, attacks={(a,b),(b,c)}) and confirm the grounded extension is {a,c}.
2. **Task B — Preferred extensions:** implement `is_admissible` and `preferred_extensions`.
   Reproduce the §4 mutual-attack example (A={a,b}, attacks={(a,b),(b,a)}) and confirm the
   grounded extension is ∅ while the preferred extensions are {a} and {b}.
3. **Task C — Divergence resolved:** extend the §4 mutual-attack pair with a third argument d
   that attacks a only (and is attacked by nothing); confirm the grounded extension becomes
   {d, b} as derived in the Week 8 lecture's §7 exercise.
4. **Task D — Mini-challenge:** construct your own 5-argument AF with at least one cycle, predict
   its grounded extension by hand, and confirm with your code.

## Expected Output
A notebook with four clearly labeled cells/sections (A–D), each producing correct, runnable
output.

## Submission
Export/submit `lab08.ipynb` via the course submission system by the end of the lab session.
