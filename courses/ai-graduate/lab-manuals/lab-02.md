# Lab Manual 2 — Advanced Search: IDA*

**Duration:** 3 hours | **Prerequisite:** Week 2 lecture

## Objectives
Implement IDA* from scratch and empirically compare its memory/time behavior against plain A*.

## Setup
1. Reuse your Week 1 virtual environment.
2. Create `lab02.ipynb`.

## Procedure
1. **Task A — Plain A*:** implement A* with `heapq` for the 8-puzzle using the Manhattan-distance
   heuristic. Track and report peak frontier size (a proxy for memory use).
2. **Task B — IDA*:** implement IDA* per the Week 2 lecture content's pseudocode, using the same
   heuristic. Track and report the number of contour iterations run.
3. **Task C — Comparison:** on at least 5 random solvable 8-puzzle instances of varying solution
   depth, run both algorithms and report: solution length, wall-clock time, and peak memory proxy
   (frontier size for A*; max recursion depth for IDA*) for each.
4. **Task D — Mini-challenge:** implement bidirectional search for a simple unweighted maze
   (grid with walls) and verify it finds the same shortest path as BFS from the start alone.

## Expected Output
A notebook with four clearly labeled cells/sections (A–D), each producing correct, runnable
output and a short (2–3 sentence) written comparison for Task C.

## Submission
Export/submit `lab02.ipynb` via the course submission system by the end of the lab session.
