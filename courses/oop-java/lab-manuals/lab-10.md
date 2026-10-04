# Lab Manual 10 — Generic Classes

**Duration:** 3 hours | **Prerequisite:** Week 10 lecture

## Objectives
Practice writing and instantiating a generic class, and recognizing a raw-type usage.

## Setup
Create `Lab10.java` (and any supporting classes) in your working folder.

## Procedure
1. **Task A — Generic class:** design `class Box<T>` with a `set(T content)`/`get()` pair.
2. **Task B — Two type parameters:** design `class Pair<T, U>` with a constructor and
   `getFirst()`/`getSecond()`.
3. **Task C — Bounded type:** design `class NumericBox<T extends Number>` with a method
   `double doubled()` that calls `doubleValue()` on its stored value.
4. **Task D — Mini-challenge:** instantiate `Box<T>` with two different types in the same
   `main`, then deliberately declare one raw-type `Box rawBox = new Box();`, record the compiler
   warning in a comment, and fix it to a properly parameterized `Box<String>`.

## Expected Output
A project with `Box<T>`, `Pair<T, U>`, and `NumericBox<T extends Number>`, compiling cleanly with
`javac -Xlint:unchecked` showing no warnings in the final, fixed form.

## Submission
Submit your `.java` files via the course submission system by the end of the lab session.
