# Lab Manual 15 — Debugging and Testing

**Duration:** 3 hours | **Prerequisite:** Week 15 lecture

## Objectives
Practice reading stack traces, using an IDE debugger, and writing hand-built test cases.

## Setup
Open the instructor-provided buggy program (or `Lab15.java` if building one from scratch).

## Procedure
1. **Task A — Read the stack trace:** run the buggy program, capture the stack trace it prints,
   and write down (in a comment) your hypothesis for the bug's location and cause before opening
   the debugger.
2. **Task B — Debug it:** set a breakpoint near the suspected line in your IDE, step through, and
   confirm (or revise) your hypothesis; fix the bug.
3. **Task C — Defensive programming:** add input validation (throwing `IllegalArgumentException`
   with a clear message) to at least one method in the fixed program that previously assumed
   valid input.
4. **Task D — Hand-built tests:** write a small `main`-based test harness (as in the lecture's
   `ManualTests` pattern) that checks at least 4 expected outputs of a method from this or an
   earlier lab, printing `PASS`/`FAIL` for each.

## Expected Output
A single source file (or the fixed provided file) with the bug fixed, validation added, and a
test harness printing clear `PASS`/`FAIL` results.

## Submission
Submit the fixed/annotated source file via the course submission system by the end of the lab
session.
