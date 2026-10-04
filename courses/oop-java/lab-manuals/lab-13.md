# Lab Manual 13 — Collections Framework

**Duration:** 3 hours | **Prerequisite:** Week 13 lecture

## Objectives
Practice `ArrayList`/`HashMap`, enhanced `for` iteration, and sorting a `List` of custom objects
with a lambda-based `Comparator`.

## Setup
Create `Lab13.java` (and any supporting classes) in your working folder.

## Procedure
1. **Task A — `ArrayList`:** build a `List<Student>` of at least five `Student` objects (`name`,
   `gpa` fields), add/remove a couple of entries, and print its size.
2. **Task B — `HashMap`:** build a `Map<String, Student>` keyed by each student's name, and look
   up at least two students by key.
3. **Task C — Enhanced `for`:** iterate the `List<Student>` with an enhanced `for` loop printing
   each `toString()`, and iterate the `Map`'s `entrySet()` printing each key and value.
4. **Task D — Sorting with `Comparator`:** sort the `List<Student>` by GPA descending using a
   lambda, then re-sort it by GPA descending with name ascending as a tie-break using
   `Comparator.comparing(...).thenComparing(...)`.

## Expected Output
A project compiling cleanly with `javac`, printing the `List`/`Map` contents at each step and the
two different sorted orders from Task D.

## Submission
Submit your `.java` files via the course submission system by the end of the lab session.
