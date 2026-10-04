# Lab Manual 15 — Debugging & Testing OOP Code

**Duration:** 3 hours | **Prerequisite:** Week 15 lecture

## Objectives
Practice tracing stack traces across a class hierarchy, recognizing the recurring OOP bug
checklist, and writing a small hand-rolled test harness.

## Setup
Create `Lab15.java` (and any supporting classes) in your working folder.

## Procedure
1. **Task A — Trace a stack trace:** given a provided three-level hierarchy (`Shape` →
   `Polygon` → `Rectangle`) with a deliberately planted `NullPointerException` in `Rectangle`'s
   `area()` override, run it, and record in a comment exactly which class/line the top stack
   frame points to.
2. **Task B — Bug checklist sweep:** given four provided code snippets (one per bug: missing
   `@Override`, wrong `equals` signature, `equals` without `hashCode`, a raw generic type),
   identify each bug by name and write the one-line fix for each.
3. **Task C — Hand-rolled test harness:** write a `ManualTests` class with a `check(String
   label, boolean condition)` helper, and at least five `check(...)` calls exercising
   constructors, `equals`/`hashCode`, and polymorphic `area()` calls across the `Shape` hierarchy.
4. **Task D — Mini-challenge:** add a `check(...)` call that would have caught Task A's bug
   *before* it shipped, and confirm it fails against the buggy version and passes after the fix.

## Expected Output
A project compiling cleanly with `javac` (after all fixes), with `ManualTests.main` printing a
final "X passed, Y failed" summary showing all tests passing.

## Submission
Submit your `.java` files via the course submission system by the end of the lab session.
