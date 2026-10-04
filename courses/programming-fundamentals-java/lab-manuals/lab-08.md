# Lab Manual 8 — References and Midterm Review

**Duration:** 3 hours | **Prerequisite:** Week 8 lecture

## Objectives
Practice reasoning about references and `null`, and review Weeks 1–7 for the midterm.

## Setup
Create `Lab08.java` in your working folder.

## Procedure
1. **Task A — Shared references:** assign one array variable to another (`int[] b = a;`), mutate
   an element through `b`, and print `a` to show the change is visible through both variables.
2. **Task B — `null` and `NullPointerException`:** declare an `int[]` variable initialized to
   `null`, deliberately trigger a `NullPointerException` by accessing its `.length`, observe the
   message, then fix it with a `null` check.
3. **Task C — Review problems:** solve 3 instructor-provided practice problems spanning
   operators, control flow, loops, methods, and arrays.
4. **Task D — Mini-challenge:** write a short program demonstrating that two separately
   constructed `String`s with identical content are `.equals()` but may or may not be `==`
   (reusing Week 7's lesson), and explain the distinction in a comment.

## Expected Output
A single source file with four clearly labeled sections (A–D); Task B's exception should be shown
as a comment with the fix applied in the final submitted code.

## Submission
Submit `Lab08.java` via the course submission system by the end of the lab session.
