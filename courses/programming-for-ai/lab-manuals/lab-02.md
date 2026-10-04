# Lab Manual 2 — Data Structures & OOP

**Duration:** 3 hours | **Prerequisite:** Week 2 lecture

## Objectives
Practice choosing/using Python's built-in data structures and writing a simple class.

## Setup
Continue in the same Jupyter environment from Lab 1; create `lab02.ipynb`.

## Procedure
1. **Task A — Data structure choice:** given a small dataset of student records (list of dicts),
   write code to (a) get unique course names (set), (b) build a dict mapping `student_id → name`.
2. **Task B — Comprehensions:** rewrite three provided `for`-loop snippets as list/dict
   comprehensions.
3. **Task C — OOP:** implement the `GraphNode`/`Graph` classes from the lecture; add a method
   `Graph.neighbors(name)` returning the neighbor names of a given node.
4. **Task D — Mini-challenge:** extend `Graph` with a method `has_edge(a, b)`.

## Expected Output
A notebook with Tasks A–D completed; the `Graph` class from Task C/D must be reusable as-is in
Lab 5 (BFS/DFS) — keep the method signatures from the lecture.

## Submission
Submit `lab02.ipynb` by the end of the lab session.
