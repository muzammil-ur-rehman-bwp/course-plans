# Lab Manual 1 — Research Landscape Mapping & Environment Setup

**Duration:** 3 hours | **Prerequisite:** Week 1 lecture

## Objectives
Set up the course Python environment and practice mapping research questions onto this course's
four pillars, as direct early preparation for the capstone.

## Setup
1. Create a virtual environment: `python -m venv venv` then activate it.
2. `pip install jupyter numpy`.
3. Launch `jupyter notebook` and create `lab01.ipynb`.

## Procedure
1. **Task A — Self-assessment checklist:** for each of the six assumed graduate-AI topics
   (advanced search, game theory, SAT/SMT, planning complexity, MDPs/RL, AI complexity), write
   one sentence stating your current confidence level and, for any topic below full confidence,
   one specific concept you will review before Week 2.
2. **Task B — Pillar mapping:** for five instructor-provided research-question prompts, identify
   which of this course's four pillars each belongs to, and name one graduate-AI topic each
   assumes as background.
3. **Task C — Research-interest paragraph:** write a one-paragraph, tentative statement of a
   research area within this course's scope that interests you (it is fine and expected for this
   to change substantially by Week 13).
4. **Task D — Mini-challenge:** write a short Python function `pillar_keywords(text)` that takes
   a short text and returns a list of this course's four pillar names whose associated keyword
   list (you define each pillar's keyword list, e.g., "regret," "bandit" for Pillar 1) appears in
   the text; test it against the five prompts from Task B.

## Expected Output
A notebook with four clearly labeled cells/sections (A–D), each producing correct, runnable
output with a one-line comment explaining the approach.

## Submission
Export/submit `lab01.ipynb` via the course submission system by the end of the lab session.
