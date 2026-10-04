# Lab Manual 1 — Foundations Diagnostic & Advanced-KR Landscape Mapping

**Duration:** 3 hours | **Prerequisite:** Week 1 lecture

## Objectives
Confirm the undergraduate KR&R foundations this course assumes are solid, and practice mapping
advanced-KR research questions onto this course's week map.

## Setup
1. Create a virtual environment: `python -m venv venv` then activate it.
2. `pip install jupyter`.
3. Launch `jupyter notebook` and create `lab01.ipynb`.

## Procedure
1. **Task A — Resolution/unification diagnostic:** implement a small propositional resolution
   refutation (reusing/re-deriving the undergraduate pattern) to show {p∨q, ¬p, ¬q} is
   unsatisfiable, and implement a basic unification function on {P(x,f(y)), P(a,f(b))}. This is a
   diagnostic, not new instruction — flag any gap to the instructor immediately.
2. **Task B — Rule-engine diagnostic:** implement (or reuse) a small forward-chaining engine over
   a 3-rule toy knowledge base and confirm it derives the expected facts.
3. **Task C — Landscape mapping:** for 5 instructor-provided advanced-KR research-question
   prompts, identify which week of this course's map each belongs to, and name one sibling
   course (undergraduate KR&R, *Artificial Intelligence* Graduate, *Machine Learning* Graduate)
   each is explicitly NOT about.
4. **Task D — Mini-challenge:** sketch (in a markdown cell, no code required) a competency
   question for each of 3 advanced-KR formalisms this course will cover, phrased as "what should
   I be able to ask this formalism, once I've learned it?"

## Expected Output
A notebook with four clearly labeled cells/sections (A–D), each producing correct, runnable
output (A–B) or clear written answers (C–D).

## Submission
Export/submit `lab01.ipynb` via the course submission system by the end of the lab session.
