# Lab Manual 5 — Induction Heads and the Implicit-Gradient-Descent Analogy

**Duration:** 3 hours | **Prerequisite:** Week 5 lecture

## Objectives
Trace an induction-head circuit on toy sequences; reproduce the linear-attention/linear-regression
toy argument for implicit gradient descent at the level of its stated assumptions; write a
structured, evidence-graded critique distinguishing the two.

## Setup
1. Reuse your Week 1 virtual environment.
2. Create `lab05.ipynb` (code) and `lab05-critique.md` (written critique).

## Procedure
1. **Task A — Induction-head trace:** implement `toy_induction_trace` exactly as in the Week 5
   lecture content; run it on at least 3 synthetic repeated-pattern sequences of varying length
   and vocabulary size, and confirm it correctly identifies the induction target at each repeated
   occurrence.
2. **Task B — Attention-pattern visualization:** build a minimal 2-layer toy attention model (or
   use provided weights) exhibiting a previous-token head and an induction head, and visualize
   the attention pattern at the induction head for one of Task A's sequences, confirming it
   attends to the position the trace predicts.
3. **Task C — Evidence-grading checklist:** for each of (i) the induction-heads claim and (ii)
   the implicit-gradient-descent analogy, answer in writing: what exactly was measured or proven?
   Under what assumptions/scale/setting? What is the gap to the general claim informally made?
   What evidence would close that gap?
4. **Task D — Structured critique:** write a 150–200 word structured critique of the implicit-
   gradient-descent analogy using the Task C checklist, in `lab05-critique.md`.

## Expected Output
A notebook (A–B) and a written critique document (C–D).

## Submission
Submit `lab05.ipynb` and `lab05-critique.md` via the course submission system by the end of the
lab session.
