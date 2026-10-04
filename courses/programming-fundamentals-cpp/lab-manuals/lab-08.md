# Lab Manual 8 — Pointers & Midterm Practice

**Duration:** 3 hours | **Prerequisite:** Week 8 lecture

## Objectives
Practice pointer declaration, dereferencing, and pointer-based array traversal; work through
midterm practice problems spanning Weeks 1–7.

## Setup
Create `lab08.cpp` in your working folder.

## Procedure
1. **Task A — Pointer basics:** declare an `int`, a pointer to it, print the variable's value via
   the pointer, modify it via the pointer, and print the variable again to confirm the change.
2. **Task B — Pointer array traversal:** write `int sumViaPointer(const int* values, int size)`
   that computes an array's sum using pointer arithmetic (`*(values + i)`) instead of `[]`.
3. **Task C — Output parameters:** write `void findMinMax(const int values[], int size, int*
   minOut, int* maxOut)` that writes the array's min and max through the two pointer parameters.
4. **Task D — Midterm practice set:** complete the instructor-provided set of 5 short practice
   problems covering Weeks 1–7 (operators, control flow, loops, functions, arrays, strings).

## Expected Output
A single source file with Tasks A–C clearly labeled and compiling cleanly with `-Wall`; Task D's
practice problems submitted as directed by the instructor (may be a separate handout/sheet).

## Submission
Submit `lab08.cpp` (and Task D practice answers) via the course submission system by the end of
the lab session.
