# Lab Manual 3 — Finite-Trace LTL Evaluator

**Duration:** 3 hours | **Prerequisite:** Week 3 lecture

## Objectives
Implement a finite-trace LTL evaluator and use it to check planning-style goal/safety/liveness
properties on sample execution traces.

## Setup
1. Reuse your course virtual environment.
2. Create `lab03.ipynb`.

## Procedure
1. **Task A — Evaluator:** implement `ltl_eval` from the Week 3 lecture content. Reproduce the
   lecture's `G(request → F response)` worked example on the given 5-step trace and confirm the
   result.
2. **Task B — Failing case:** remove the response at the final step (as in the lecture) and
   confirm the same formula now evaluates to False; identify, by hand, exactly which `F response`
   sub-evaluation caused the failure.
3. **Task C — New formula set:** for a provided 6-step trace with atoms `door_open`,
   `alarm_armed`, write and evaluate: (i) a safety property "the alarm is never armed while the
   door is open," and (ii) a liveness-style property "the door is eventually closed."
4. **Task D — Mini-challenge:** implement a small, explicit comment in code (not an assertion)
   documenting exactly where your evaluator's finite-trace convention for G/F diverges from the
   infinite-trace textbook semantics, with one concrete trace where the divergence would matter.

## Expected Output
A notebook with four clearly labeled cells/sections (A–D), each producing correct, runnable
output.

## Submission
Export/submit `lab03.ipynb` via the course submission system by the end of the lab session.
