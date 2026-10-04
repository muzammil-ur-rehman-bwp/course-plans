# Lab Manual 12 — Recursion

**Duration:** 3 hours | **Prerequisite:** Week 12 lecture

## Objectives
Practice writing and tracing recursive methods.

## Setup
Create `Lab12.java` in your working folder.

## Procedure
1. **Task A — Factorial:** write `static int factorial(int n)` recursively, and test it on
   `n = 0, 1, 5, 10`.
2. **Task B — Fibonacci:** write `static int fibonacci(int n)` recursively, and test it on
   `n = 0` through `10`, printing the sequence.
3. **Task C — Array sum:** write `static int sumArray(int[] values, int index)` recursively (base
   case at `index == values.length`), and test it against a loop-based sum to confirm they match.
4. **Task D — Mini-challenge:** write a recursive `static int gcd(int a, int b)` using Euclid's
   algorithm (`gcd(a, 0) = a`; `gcd(a, b) = gcd(b, a % b)`), and test it on at least 3 pairs.

## Expected Output
A single source file with four clearly labeled sections (A–D), each compiling cleanly and
producing correct output.

## Submission
Submit `Lab12.java` via the course submission system by the end of the lab session.
