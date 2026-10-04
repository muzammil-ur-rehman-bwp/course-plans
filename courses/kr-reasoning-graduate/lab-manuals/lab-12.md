# Lab Manual 12 — Muddy Children Simulator

**Duration:** 3 hours | **Prerequisite:** Week 12 lecture

## Objectives
Implement the muddy-children public-announcement simulator and verify the round-k result for
several (n, k) instances.

## Setup
1. Reuse your course virtual environment.
2. Create `lab12.ipynb`.

## Procedure
1. **Task A — Simulator:** implement `all_worlds`, `indistinguishable`, `knows_own_status`, and
   `simulate_muddy_children` from the Week 12 lecture content. Reproduce the n=3, k=2 trace from
   §4 and confirm it returns round 2.
2. **Task B — Sweep:** run the simulator for (n,k) ∈ {(2,1), (4,1), (4,3), (5,2), (6,4)} and
   confirm, in every case, the returned round equals k.
3. **Task C — Clean-child check:** for the n=3, k=2 case, confirm the clean child (the one not
   in `actual_muddy`) does **not** satisfy `knows_own_status` at the returned world set at round
   2, matching the lecture's claim that only muddy children resolve their uncertainty at round k.
4. **Task D — Mini-challenge:** compute, by hand, E_G, C_G, and D_G for a small 2-agent Kripke
   model you construct where all three differ (reusing the Week 12 definitions), and verify your
   hand computation against a small script evaluating each over the model's worlds.

## Expected Output
A notebook with four clearly labeled cells/sections (A–D), each producing correct, runnable
output.

## Submission
Export/submit `lab12.ipynb` via the course submission system by the end of the lab session.
