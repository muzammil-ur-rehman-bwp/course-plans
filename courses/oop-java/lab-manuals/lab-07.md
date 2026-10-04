# Lab Manual 7 — Polymorphism I

**Duration:** 3 hours | **Prerequisite:** Week 7 lecture

## Objectives
Practice dynamic dispatch, upcasting/downcasting, and safe use of `instanceof`.

## Setup
Create `Lab07.java` (and any supporting classes) in your working folder.

## Procedure
1. **Task A — Dynamic dispatch:** using `Animal`/`Dog`/`Cat` (from Week 5/6), store a `Dog` in an
   `Animal`-typed variable and call `makeSound()`; record which version runs and why.
2. **Task B — Upcasting is automatic:** write a method `static void describe(Animal a)` and call
   it with a `Dog` argument with no explicit cast.
3. **Task C — Safe downcasting:** given an `Animal[]` holding a mix of `Dog` and `Cat` objects,
   write a loop that uses `instanceof` (either form) to safely call a `Dog`-only method
   (`fetch()`) only on the elements that actually are `Dog`s.
4. **Task D — Mini-challenge:** deliberately write an unguarded downcast that throws
   `ClassCastException` at runtime, record the exact exception message, then fix it with an
   `instanceof` guard.

## Expected Output
A project demonstrating correct dynamic dispatch, safe downcasting, and a recorded, then fixed,
`ClassCastException`, compiling cleanly with `javac` in its final form.

## Submission
Submit your `.java` files via the course submission system by the end of the lab session.
