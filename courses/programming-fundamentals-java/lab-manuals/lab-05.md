# Lab Manual 5 — Methods

**Duration:** 3 hours | **Prerequisite:** Week 5 lecture

## Objectives
Practice declaring and calling methods, including overloaded methods, and observe pass-by-value
semantics for primitives vs. arrays.

## Setup
Create `Lab05.java` in your working folder.

## Procedure
1. **Task A — Basic method:** write `static double circleArea(double radius)` and call it for at
   least 3 radii, printing each labeled result.
2. **Task B — Pass-by-value demo:** write `static void tryToDouble(int n)` that doubles its
   parameter, and `static void zeroOutFirst(int[] arr)` that sets `arr[0] = 0`. Call both from
   `main`, printing the original variables before and after each call.
3. **Task C — Overloading:** write an overloaded pair of `discount` methods (one with a max-cap
   parameter, one without), and demonstrate both being called.
4. **Task D — Mini-challenge:** write `static char letterGrade(double score)` (reusing the Week 3
   grading bands) and call it on at least 5 test scores, including the boundary values.

## Expected Output
A single source file with four clearly labeled sections (A–D), each compiling cleanly and
producing correct output.

## Submission
Submit `Lab05.java` via the course submission system by the end of the lab session.
