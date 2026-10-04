# Lab Manual 6 — Arrays (1D)

**Duration:** 3 hours | **Prerequisite:** Week 6 lecture

## Objectives
Practice declaring, initializing, iterating over, and passing 1D arrays to functions.

## Setup
Create `lab06.cpp` in your working folder.

## Procedure
1. **Task A — Fill and print:** declare a 10-element `int` array, fill it with user input, and
   print all elements.
2. **Task B — Aggregates:** write functions `int sumArray(const int[], int)`,
   `double averageArray(const int[], int)`, and `int maxArray(const int[], int)`; call all three
   on the array from Task A.
3. **Task C — Search:** write `int indexOf(const int[], int size, int target)` that returns the
   index of the first occurrence of `target`, or `-1` if not found; test with a present and an
   absent value.
4. **Task D — Mini-challenge:** write `void reverseArray(int[], int)` that reverses an array's
   elements in place (no second array), and print the array before and after.

## Expected Output
A single source file with four clearly labeled sections (A–D), each compiling cleanly with
`-Wall` and producing correct output, including the "not found" case for Task C.

## Submission
Submit `lab06.cpp` via the course submission system by the end of the lab session.
