# Lab Manual 8 — Embedding-Plus-Constraint Candidate Filtering

**Duration:** 3 hours | **Prerequisite:** Week 8 lecture

## Objectives
Filter TransE-style link-prediction candidates against hand-written hard constraints, and
critically assess what the filtering reveals about embedding confidence vs. correctness.

## Setup
1. Reuse your Week 1 environment and your graduate course's Week 10 TransE implementation.
2. Create `lab08.ipynb`.

## Procedure
1. **Task A — Implementation:** implement `filter_candidates`, `no_self_loop`, and
   `type_constraint` exactly as in the Week 8 lecture content.
2. **Task B — Reproduce the worked example:** reproduce the `alice`/`bob`/`acme` filtering
   example; confirm only the valid candidate survives.
3. **Task C — Real TransE candidates:** rank link-prediction candidates using your reused TransE
   model from a prior course's lab; apply at least two hard constraints (one type constraint, one
   structural constraint of your choice) and report which top-ranked candidates survive or are
   vetoed.
4. **Task D — Honest assessment:** for any top-ranked candidate your TransE model proposed with
   high confidence (high score) that a constraint vetoes, write 2–3 sentences on what this reveals
   about embedding-based confidence vs. correctness, connecting explicitly to the lecture
   content's §3 "honest assessment."

## Expected Output
A notebook with four clearly labeled cells/sections (A–D), each producing correct, runnable
output and the required written assessment.

## Submission
Export/submit `lab08.ipynb` via the course submission system by the end of the lab session.
