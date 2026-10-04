# Lab Manual 13 — Simpson's Paradox and Confounding

**Duration:** 3 hours | **Prerequisite:** Week 13 lecture

## Objectives
Reproduce Simpson's paradox numerically and construct an original confounding example.

## Setup
Create `lab13.ipynb`. pandas, NumPy, Matplotlib.

## Procedure
1. **Task A — Reproduce the lecture example:** implement the treatment/severity table and
   stratified-vs-aggregate analysis from the lecture content; confirm the reversal.
2. **Task B — Visualize:** create a bar chart comparing stratified and aggregate recovery rates
   side by side.
3. **Task C — Original example:** construct a second, original Simpson's-paradox-style dataset
   (synthetic is fine) with a different confounder and context (e.g., admissions rates by
   department, or a different clinical scenario); reproduce the reversal in your own data.
4. **Task D — Causal-role classification:** for both Task A and Task C's confounder, draw (as a
   simple text/markdown diagram or using a plotting library) the 3-node causal DAG and label the
   confounder explicitly.
5. **Task E — Reflection:** explain, in 3–4 sentences, what adjustment (if any) would be
   *incorrect* if the stratifying variable in your Task C example were instead a mediator rather
   than a confounder.

## Expected Output
A notebook with Tasks A–E, including both datasets' stratified/aggregate comparison charts.

## Submission
Submit `lab13.ipynb` by the end of the lab session.
