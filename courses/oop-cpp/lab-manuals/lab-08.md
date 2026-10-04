# Lab Manual 8 — Virtual Functions & Object Slicing

**Duration:** 3 hours | **Prerequisite:** Week 8 lecture

## Objectives
Practice `virtual` functions, dynamic dispatch, virtual destructors, and recognizing/avoiding
object slicing.

## Setup
Create `lab08.cpp` in your working folder.

## Procedure
1. **Task A — Without `virtual`:** write `class Animal` with a non-`virtual` `speak() const` and
   `class Dog : public Animal` overriding it; call `speak()` through an `Animal*` pointing at a
   `Dog` and observe (in a comment) that `Animal::speak()` runs, not `Dog::speak()`.
2. **Task B — With `virtual`:** add `virtual` to `Animal::speak()` and `override` to
   `Dog::speak()`; repeat the same call through `Animal*` and confirm `Dog::speak()` now runs.
3. **Task C — Virtual destructor:** give `Dog` an owning raw pointer member (e.g. `int* tag_`)
   allocated in its constructor and freed in its destructor; `delete` a `Dog` through an
   `Animal*` first with a *non*-`virtual` `~Animal()` (observe the leak/incorrect behavior in a
   comment, do not actually ship this version), then fix it by making `~Animal()` `virtual`.
4. **Task D — Slicing:** write `void describe(Animal a)` (by value) and `void describeRef(const
   Animal& a)` (by reference); call both with a `Dog` object and explain, in a comment, why only
   the reference version correctly dispatches to `Dog::speak()`.

## Expected Output
A single source file with four clearly labeled sections (A–D), compiling cleanly with `-Wall`;
Task C's leaking version should be clearly marked as a deliberately-broken demonstration, not the
final working code.

## Submission
Submit `lab08.cpp` via the course submission system by the end of the lab session.
