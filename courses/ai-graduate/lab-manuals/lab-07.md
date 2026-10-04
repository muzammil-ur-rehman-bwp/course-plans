# Lab Manual 7 — Relaxed Planning Graphs & HTN Decomposition

**Duration:** 3 hours | **Prerequisite:** Week 7 lecture

## Objectives
Build a relaxed planning graph to compute a heuristic value, and implement a simple HTN
decomposition for a toy logistics domain.

## Setup
1. Reuse your course virtual environment.
2. Create `lab07.ipynb`.

## Procedure
1. **Task A — Relaxed planning graph:** implement `build_relaxed_planning_graph` from the
   Week 7 lecture content for a small logistics domain (move a package between two locations via
   a vehicle); compute h_level for the goal "package at destination" from the initial state.
2. **Task B — Heuristic sanity check:** compute h_level from two different initial states (one
   closer to the goal, one farther) and confirm the heuristic values are ordered sensibly
   (closer state gets a smaller or equal h_level).
3. **Task C — HTN decomposition:** implement `htn_decompose` from the Week 7 lecture content for
   the abstract task `Deliver(package, destination)`, decomposing into primitive
   `Load`/`Drive`/`Unload` actions; verify it produces a valid primitive sequence.
4. **Task D — Mini-challenge:** add a second method for `Deliver` that routes through an
   intermediate hub (two `Drive` legs instead of one), and show that `htn_decompose` can select
   between the two methods when the first one's preconditions are not met.

## Expected Output
A notebook with four clearly labeled cells/sections (A–D), each producing correct, runnable
output.

## Submission
Export/submit `lab07.ipynb` via the course submission system by the end of the lab session.
