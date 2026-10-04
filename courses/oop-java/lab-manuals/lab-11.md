# Lab Manual 11 — Generic Methods

**Duration:** 3 hours | **Prerequisite:** Week 11 lecture

## Objectives
Practice writing generic methods with their own type parameters, and reading a wildcard-bounded
signature.

## Setup
Create `Lab11.java` (and any supporting classes) in your working folder.

## Procedure
1. **Task A — Generic method:** write `static <T> void printAll(List<T> items)` that prints
   every element.
2. **Task B — Bounded generic method:** write `static <T extends Comparable<T>> T max(List<T>
   items)` that returns the largest element.
3. **Task C — Wildcard reading:** given a provided method signature using
   `List<? extends Number>`, write in a comment what kinds of `List` arguments it accepts and why
   `List<String>` would not be one of them.
4. **Task D — Mini-challenge:** write `static <T> boolean containsDuplicate(List<T> items)` using
   each item's `equals()` for comparison, and test it with a `List<String>` and a `List<Integer>`.

## Expected Output
A project with the four methods above, compiling cleanly with `javac`, tested in `main` with at
least two different concrete types per generic method.

## Submission
Submit your `.java` files via the course submission system by the end of the lab session.
