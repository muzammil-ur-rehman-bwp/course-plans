# Lab Manual 6 — Inheritance II

**Duration:** 3 hours | **Prerequisite:** Week 6 lecture

## Objectives
Practice distinguishing overriding from overloading, using `@Override` correctly, and recognizing
what every class inherits from `Object`.

## Setup
Create `Lab06.java` (and any supporting classes) in your working folder.

## Procedure
1. **Task A — Override:** given `class Animal` with `public String makeSound()`, write
   `class Dog extends Animal` that overrides `makeSound()` (same signature, `@Override`) to
   return `"Woof!"`.
2. **Task B — Overload:** add an overloaded `public String makeSound(int times)` to `Dog` that
   repeats the sound, and explain in a comment why this does *not* replace the overridden
   no-argument version.
3. **Task C — Deliberate `@Override` failure:** write a method intended as an override with a
   typo in its name or parameter list, mark it `@Override`, and record the exact compiler error
   in a comment; then fix it.
4. **Task D — `Object` methods:** without writing any `equals`/`hashCode`/`toString` yourself on
   a new, otherwise-empty class, call all three on an instance and record the default output in a
   comment; then override just `toString()`.

## Expected Output
A project with `Animal`, `Dog`, and a plain class for Task D, compiling cleanly with `javac`
(after the Task C fix), demonstrating the distinction between overriding and overloading.

## Submission
Submit your `.java` files via the course submission system by the end of the lab session.
