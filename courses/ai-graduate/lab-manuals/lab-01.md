# Lab Manual 1 — Formal Problem Formulation & Environment Setup

**Duration:** 3 hours | **Prerequisite:** Week 1 lecture

## Objectives
Set up the course Python environment and practice restating real-world scenarios as formal
search problems at graduate-level precision.

## Setup
1. Create a virtual environment: `python -m venv venv` then activate it.
2. `pip install jupyter`.
3. Launch `jupyter notebook` and create `lab01.ipynb`.

## Procedure
1. **Task A — Formal problem definition:** write a Python `dataclass` (or plain functions)
   implementing the `SearchProblem` tuple ⟨S, s₀, A, T, G, c⟩ from the Week 1 lecture content
   for the 8-puzzle (state = a tuple of 9 tile positions).
2. **Task B — PEAS at graduate rigor:** for a self-driving taxi, write a PEAS description, then
   additionally state its formal search-problem tuple (what is S, A, T, G, c concretely for this
   domain) — going beyond the undergraduate-level PEAS-only treatment.
3. **Task C — Subfield mapping:** for three given AI research-question prompts (provided by the
   instructor), identify which of this course's subfields each belongs to, and name one sibling
   graduate course (ANN/ML/DL/KR&R) each explicitly does NOT belong to.
4. **Task D — Mini-challenge:** implement a generic `successors(state)` function for the
   8-puzzle (returns all states reachable by sliding one tile), and verify it produces exactly 2,
   3, or 4 successors depending on the blank tile's position.

## Expected Output
A notebook with four clearly labeled cells/sections (A–D), each producing correct, runnable
output with a one-line comment explaining the approach.

## Submission
Export/submit `lab01.ipynb` via the course submission system by the end of the lab session (or
per instructor-announced deadline).
