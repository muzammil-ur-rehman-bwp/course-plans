# Lab Manual 14 — Multi-Agent Q-Learning & Feature-Sensitivity Explanation

**Duration:** 3 hours | **Prerequisite:** Week 14 lecture

## Objectives
Implement a toy multi-agent Q-learning setup and a simple model-agnostic explanation method,
connecting back to Weeks 3–4 and 9.

## Setup
1. Reuse your course virtual environment.
2. Create `lab14.ipynb`.

## Procedure
1. **Task A — Independent Q-learners:** implement `independent_q_learning_step` from the
   Week 14 lecture content for a repeated Prisoner's Dilemma played by two independent
   Q-learners; run for several thousand rounds.
2. **Task B — Convergence check:** report the joint action frequencies over the final 100
   rounds and compare to the single-shot Nash equilibrium found in Week 3's lab.
3. **Task C — Feature sensitivity:** implement `feature_sensitivity` from the Week 14 lecture
   content for a simple rule-based decision function (e.g., a toy loan-approval rule using
   income and credit-score features); report which feature the function is most sensitive to
   for a given example input.
4. **Task D — Mini-challenge:** construct one concrete toy example of reward misspecification
   (e.g., a simple grid-world "cleaning robot" reward that an RL agent could exploit in an
   unintended way) and briefly explain the mismatch between stated reward and intended goal.

## Expected Output
A notebook with four clearly labeled cells/sections (A–D), each producing correct, runnable
output.

## Submission
Export/submit `lab14.ipynb` via the course submission system by the end of the lab session.
