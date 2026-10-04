# Lab Manual 10 — POMDP Belief-State Updates

**Duration:** 3 hours | **Prerequisite:** Week 10 lecture

## Objectives
Implement the belief-state update for a small POMDP and trace it over a sequence of actions and
observations.

## Setup
1. Reuse your course virtual environment.
2. Create `lab10.ipynb`.

## Procedure
1. **Task A — Belief update implementation:** implement `belief_update` from the Week 10
   lecture content; reproduce the Week 10 worked example (Healthy/Sick, "stay" action, one
   "Positive" observation) and confirm your result matches the lecture's hand-worked numbers.
2. **Task B — Sequential updates:** starting from the Task A result, apply a second "Positive"
   observation; then, separately, apply a "Negative" observation instead; report and compare the
   resulting beliefs.
3. **Task C — Larger POMDP:** extend the model to three underlying states (e.g., Healthy, Mild,
   Severe) with a corresponding observation model, and trace a belief update over a short
   sequence of 3 actions/observations.
4. **Task D — Mini-challenge:** show, by computing beliefs after 5 consecutive "Positive"
   observations, that belief in "Sick"/"Severe" approaches (but does not necessarily reach) 1,
   and explain why it never reaches exactly 1 given the observation model's nonzero
   false-positive/false-negative rates.

## Expected Output
A notebook with four clearly labeled cells/sections (A–D), each producing correct, runnable
output.

## Submission
Export/submit `lab10.ipynb` via the course submission system by the end of the lab session.
