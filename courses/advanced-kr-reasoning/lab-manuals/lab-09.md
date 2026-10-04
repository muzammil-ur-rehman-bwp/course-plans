# Lab Manual 9 — Explicit-State LTL Model Checker

**Duration:** 3 hours | **Prerequisite:** Week 9 lecture

## Objectives
Implement a small explicit-state LTL model checker and use it to check safety and liveness
properties, including producing a counterexample trace.

## Setup
1. Reuse your Week 1 environment.
2. Create `lab09.ipynb`.

## Procedure
1. **Task A — Implementation:** implement `Kripke`, `reachable`, `check_safety_always_not`,
   `find_cycle_through`, and `check_liveness_req_eventually_resp` exactly as in the Week 9 lecture
   content.
2. **Task B — Safety check:** build a small Kripke structure with a `bad` proposition reachable
   from an initial state; confirm `check_safety_always_not` correctly reports the violation and
   the offending states.
3. **Task C — Liveness check, flawed system:** build the 5-state request/response example from
   the Week 9 exercise with a deliberate flaw (a cycle where `query_received` holds forever
   without `query_answered`); confirm `check_liveness_req_eventually_resp` reports the violation
   and print the returned cycle as a readable counterexample trace.
4. **Task D — Patch and re-verify:** patch the transition relation to remove the flaw and confirm
   the checker now reports the liveness property holds.

## Expected Output
A notebook with four clearly labeled cells/sections (A–D), each producing correct, runnable
output, including a human-readable counterexample trace in Task C.

## Submission
Export/submit `lab09.ipynb` via the course submission system by the end of the lab session.
