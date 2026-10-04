# Lab Manual 7 — Dalal Belief Revision Operator

**Duration:** 3 hours | **Prerequisite:** Week 7 lecture

## Objectives
Implement Dalal's Hamming-distance revision operator and verify the core AGM postulates on a
toy example.

## Setup
1. Reuse your course virtual environment.
2. Create `lab07.ipynb`.

## Procedure
1. **Task A — Dalal implementation:** implement `all_valuations`, `hamming`, `models_of`, and
   `dalal_revise` from the Week 7 lecture content. Reproduce the Tweety/penguin worked example
   (§4) and confirm K∗φ = {bird=True, flies=False}.
2. **Task B — AGM postulate check:** for the Task A result, explicitly check postulates (K∗1),
   (K∗2), (K∗3), (K∗4), (K∗6) by writing a short assertion or printed justification for each.
3. **Task C — Revision vs. update:** construct the two-coin example from §5 (two models in K,
   differing on coin B) and compute the Dalal revision by φ = "at least one coin shows tails";
   in a markdown cell, describe what a Katsuno–Mendelzon update construction would do
   differently (per-model minimal change) without necessarily implementing it in full.
4. **Task D — Mini-challenge:** construct a case where ¬φ ∉ K (φ does not contradict K) and
   confirm K∗φ = K+φ (expansion), verifying postulate (K∗4) directly.

## Expected Output
A notebook with four clearly labeled cells/sections (A–D), each producing correct, runnable
output (A, B, D) or a clear written analysis (C).

## Submission
Export/submit `lab07.ipynb` via the course submission system by the end of the lab session.
