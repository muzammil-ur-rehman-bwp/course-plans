# Lab Manual 2 — Constructors & the `Object` Contract

**Duration:** 3 hours | **Prerequisite:** Week 2 lecture

## Objectives
Practice overloaded constructors with `this(...)` chaining, and a correct, `@Override`-annotated
`equals`/`hashCode`/`toString` trio.

## Setup
Create `Lab02.java` (and any supporting classes) in your working folder.

## Procedure
1. **Task A — Overloaded constructors:** design `class Point` with fields `x`, `y`; a two-argument
   constructor; a one-argument constructor `Point(double value)` that chains to it via
   `this(value, value)`; and a no-argument constructor that chains to `this(0, 0)`.
2. **Task B — `toString()`:** override `toString()` on `Point` to print `"(x, y)"`.
3. **Task C — `equals`/`hashCode`:** override `equals(Object obj)` (with `@Override`, parameter
   type exactly `Object`) and a matching `hashCode()` using `java.util.Objects.hash(...)`.
4. **Task D — Deliberate bug, then fix:** first write `equals(Point other)` (an overload, no
   `@Override`) and show in a comment that `somePoint.equals((Object) otherPoint)` does not use it;
   then fix it to properly override `equals(Object)`.

## Expected Output
A `Point` class compiling cleanly with `javac`, with a `main` method demonstrating all four
constructors, correct `toString()` output, and `equals`/`hashCode()` consistent with each other
for two points with identical coordinates.

## Submission
Submit your `.java` files via the course submission system by the end of the lab session.
