# Lab Manual 8 — Specification Gaming in a Toy Gridworld

**Duration:** 3 hours | **Prerequisite:** Week 8 lecture

## Objectives
Reproduce a toy specification-gaming failure, diagnose it as an outer-alignment, inner-alignment,
or mixed failure, and construct a reward-specification fix — then show that a fix is not a
one-shot cure by constructing a second misspecification that still games a naive patch.

## Setup
1. Reuse your course virtual environment.
2. Create `lab08.ipynb`.

## Procedure
1. **Task A — Reproduce the demo:** run `gridworld_reward_hacking_demo` exactly as in the Week 8
   lecture content. Extract and print the learned greedy policy's action at every state (0–4).
2. **Task B — Diagnosis:** in a markdown cell, write 2–3 sentences diagnosing the failure as an
   outer-alignment failure, an inner-alignment failure, or both, explicitly justifying the
   classification against the Week 8 definitions.
3. **Task C — Fix and verify:** propose and implement one concrete reward-specification fix that
   removes the checkpoint-looping opportunity (e.g., reward only at the true goal cell, or a
   reward shaped to strictly increase only along the direct path to the goal). Retrain with the
   fixed reward and confirm the new greedy policy reaches cell 4 from every starting state.
4. **Task D — Mini-challenge:** construct a second, harder misspecification — a reward structure
   that still produces gaming behavior even after applying a Task-C-style naive fix (e.g., a
   checkpoint reward that does not decay and sits directly on the shortest path to the goal, so
   removing *that* exploit requires a different fix than Task C's). Demonstrate the new gaming
   behavior and state, in 1–2 sentences, why Task C's specific fix does not address it.

## Expected Output
A notebook with four clearly labeled cells/sections (A–D), each producing correct, runnable
output, including the printed policies for Tasks A, C, and D.

## Submission
Export/submit `lab08.ipynb` via the course submission system by the end of the lab session.
