# Lab Manual 9 — Dynamic Memory Basics

**Duration:** 3 hours (lighter session given the midterm earlier in the week) | **Prerequisite:** Week 9 lecture

## Objectives
Practice allocating and freeing single objects and dynamic arrays with `new`/`delete`.

## Setup
Create `lab09.cpp` in your working folder.

## Procedure
1. **Task A — Single allocation:** dynamically allocate a single `int`, set its value through the
   pointer, print it, then free it with `delete` and set the pointer to `nullptr`.
2. **Task B — Dynamic array:** read a size `n` from the user, dynamically allocate an array of
   `n` doubles with `new[]`, fill it with user input, print the sum and average, then free it
   with `delete[]`.
3. **Task C — Reflection:** in a comment block, explain in your own words why `delete[]` (not
   plain `delete`) must be used to free the array from Task B, and what would go wrong with a
   memory leak if `delete[]` were omitted.

## Expected Output
A single source file with Tasks A–B compiling cleanly with `-Wall` and running without leaking or
crashing; Task C's explanation as a comment.

## Submission
Submit `lab09.cpp` by the end of the lab session.
