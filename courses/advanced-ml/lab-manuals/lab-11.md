# Lab Manual 11 — Fairness Metrics and the Impossibility Result

**Duration:** 3 hours | **Prerequisite:** Week 11 lecture

## Objectives
Compute demographic-parity, equalized-odds, and calibration/PPV metrics on simulated two-group
data, and empirically confirm the calibration/equalized-odds impossibility result in both
directions.

## Setup
1. Reuse your virtual environment.
2. Create `lab11.ipynb`.

## Procedure
1. **Task A — Implementation:** implement `simulate_group`, `fairness_metrics`, and
   `ppv_formula` exactly as in the Week 11 lecture content.
2. **Task B — Equalized-odds-enforced case:** run the §6 simulation (equal TPR/FPR across
   groups, differing base rates) and confirm the resulting PPV gap matches the `ppv_formula`
   prediction.
3. **Task C — Calibration-enforced case:** following the Week 11 in-class exercise, find a
   TPR/FPR pair for Group B (base rate 0.40) that matches Group A's PPV (base rate 0.10,
   TPR=0.75, FPR=0.15) by solving the `ppv_formula` equation for TPR given a fixed FPR=0.15; run
   the simulation with this pair and report the resulting TPR/FPR gap between groups.
4. **Task D — Mini-challenge:** sweep the base-rate gap $|p_a - p_b|$ across 5 values (holding
   TPR/FPR fixed and equal across groups) and plot the resulting PPV gap against the base-rate
   gap, confirming the PPV gap grows as the base-rate gap widens.

## Expected Output
A notebook with four clearly labeled cells/sections (A–D), each producing correct, runnable
output and the required comparisons/plot.

## Submission
Export/submit `lab11.ipynb` via the course submission system by the end of the lab session.
