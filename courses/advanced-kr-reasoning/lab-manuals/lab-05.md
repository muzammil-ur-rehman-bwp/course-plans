# Lab Manual 5 — ASPIC+ Argument-Construction-and-Attack Calculator

**Duration:** 3 hours | **Prerequisite:** Week 5 lecture

## Objectives
Implement an ASPIC+ argument builder and attack classifier, and feed its output into a reused
Dung grounded-extension computation.

## Setup
1. Reuse your Week 1 environment, plus your graduate course's Dung grounded-extension code
   (reused, not re-derived).
2. Create `lab05.ipynb`.

## Procedure
1. **Task A — Implementation:** implement `Rule`, `Argument`, `build_arguments`, and `attacks`
   exactly as in the Week 5 lecture content.
2. **Task B — Tweety example:** build the Week 5 `tweety` example (strict rule
   `penguin(tweety) → not_flies`, defeasible rule `bird(tweety) ⇒ flies`) and confirm the strict
   argument rebuts the defeasible one; report which attack type `attacks` returns.
3. **Task C — Undercutting example:** add the defeasible rule
   `penguin(tweety) ⇒ not_appl(bird(tweety)⇒flies)` and confirm `attacks` now reports an
   **undercut**, not a rebut, targeting the flies-argument's top rule.
4. **Task D — Dung integration:** feed the full attack relation (tagged by type) from Tasks B–C
   into your reused grounded-extension computation and report which arguments survive.

## Expected Output
A notebook with four clearly labeled cells/sections (A–D), each producing correct, runnable
output, with attack types explicitly printed and justified.

## Submission
Export/submit `lab05.ipynb` via the course submission system by the end of the lab session.
