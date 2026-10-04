# Lab Manual 2 — NTK: Lazy Training and Kernel Drift Across Width

**Duration:** 3 hours | **Prerequisite:** Week 2 lecture

## Objectives
Compute an empirical NTK for a one-hidden-layer network at several widths, and measure relative
parameter movement and kernel drift during training at each width, reproducing the empirical
signature of approaching the lazy-training/NTK limit.

## Setup
1. Reuse your Week 1 virtual environment.
2. Create `lab02.ipynb`.

## Procedure
1. **Task A — Implementation:** implement `MLP`, `ntk_matrix`, and `run` exactly as in the Week 2
   lecture content.
2. **Task B — Width sweep:** run `run(width)` for at least 5 widths spanning at least two orders of
   magnitude (e.g., 10, 50, 200, 1000, 5000), recording relative parameter movement and kernel
   drift at each.
3. **Task C — Plotting:** plot both quantities against width on a log-x axis, and confirm both
   trend downward as width grows.
4. **Task D — Mini-challenge:** repeat Task B holding width fixed at a moderate value (e.g., 200)
   but sweeping the learning rate across at least 3 values spanning an order of magnitude; report
   how relative parameter movement responds, connecting the result to Week 2 §3's point that the
   lazy regime depends on training dynamics, not width alone.

## Expected Output
A notebook with four clearly labeled sections (A–D), including the two required plots.

## Submission
Export/submit `lab02.ipynb` via the course submission system by the end of the lab session.
