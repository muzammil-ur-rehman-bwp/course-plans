# Lab Manual 15 — Diagnosing Broken Training Runs and Hyperparameter Tuning

**Duration:** 3 hours | **Prerequisite:** Week 15 lecture

## Objectives
Diagnose the fault in several pre-generated "broken" training runs from their loss curves alone,
then run a small, systematic hyperparameter sweep.

## Setup
Create `lab15.ipynb`. The instructor provides (or you construct) four training configurations,
each with one deliberately introduced issue.

## Procedure
1. **Task A — Reproduce the four faults:** construct four training runs on the same dataset/
   model: (1) shuffled/misaligned labels, (2) unnormalized inputs, (3) learning rate 100× too
   high, (4) a correct, healthy baseline. Plot all four loss curves on one figure, clearly
   labeled.
2. **Task B — Diagnosis without peeking:** swap notebooks with a classmate (or use instructor-
   provided anonymized curves) and, from the curves alone, identify which fault each corresponds
   to; write your reasoning for each in a markdown cell.
3. **Task C — Fix and verify:** for each of the three faulty runs, apply the correct fix and
   re-plot; confirm each fixed run now matches the healthy baseline's qualitative shape.
4. **Task D — Hyperparameter sweep:** run a small grid search (at least 2 learning rates × 2
   hidden sizes) on the healthy configuration; report the best configuration by validation loss
   in a table, and plot training/validation loss for the best and worst configurations found.

## Expected Output
A notebook with Tasks A–D, including all required plots and the diagnosis write-up.

## Submission
Submit `lab15.ipynb` by the end of the lab session.
