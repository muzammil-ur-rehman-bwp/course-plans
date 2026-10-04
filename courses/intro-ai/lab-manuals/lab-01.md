# Lab Manual 1 — Environment Setup & PEAS Analysis

**Duration:** 3 hours | **Prerequisite:** Week 1 lecture

## Objectives
Set up the Python/Jupyter lab environment; practice writing PEAS specifications; implement a
trivial table-driven agent skeleton.

## Setup
Install Python 3.10+ and Jupyter (or use Google Colab). Create `lab01.ipynb`.

## Procedure
1. **Task A — Environment check:** confirm Python version and that you can run and save a
   Jupyter notebook; write a one-cell "hello agent" script that prints a greeting.
2. **Task B — PEAS specifications:** write PEAS descriptions (as the `PEAS` dataclass from the
   lecture content, or as a plain dict) for three task environments: (1) an automated taxi,
   (2) a vacuum-cleaning robot, (3) one environment of your choice (e.g., a spam filter, a
   warehouse robot).
3. **Task C — Table-driven agent skeleton:** implement a `TableDrivenAgent` class that stores a
   percept-history-to-action table (a Python dict keyed by a tuple of percepts) and returns the
   looked-up action, or `"NoOp"` if the percept sequence is not in the table.
4. **Task D — Discussion:** for each of your three PEAS environments, write 1–2 sentences on
   whether it is fully or partially observable (a short preview of Week 2).

## Expected Output
A notebook with Tasks A–D; three PEAS specifications and a working `TableDrivenAgent` class
demonstrated on at least 2 example percept sequences.

## Submission
Submit `lab01.ipynb` by the end of the lab session.
