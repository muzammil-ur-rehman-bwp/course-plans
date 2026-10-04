# Lab Manual 5 — Functions

**Duration:** 3 hours | **Prerequisite:** Week 5 lecture

## Objectives
Practice writing functions with value and reference parameters, default arguments, and
overloading.

## Setup
Create `lab05.cpp` in your working folder.

## Procedure
1. **Task A — Value parameters:** write `int square(int n)` and `double average(double a,
   double b)`; call each from `main` with test values.
2. **Task B — Reference parameters:** write `void swap(int& a, int& b)`; verify in `main` that
   calling it actually changes the caller's two variables.
3. **Task C — Overloading:** write an overloaded `int maxValue(int a, int b)` and
   `int maxValue(int a, int b, int c)`.
4. **Task D — Mini-challenge:** write `void normalizeScore(double& score, double maxPossible =
   100.0)` that rescales `score` in place to a 0–1 range using the default or a provided
   `maxPossible`; call it both with and without the second argument.

## Expected Output
A single source file with four clearly labeled sections (A–D), each compiling cleanly with
`-Wall`, with `main` printing clear before/after values proving reference parameters modified the
caller's variables.

## Submission
Submit `lab05.cpp` via the course submission system by the end of the lab session.
