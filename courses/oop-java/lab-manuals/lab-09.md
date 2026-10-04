# Lab Manual 9 — Interfaces

**Duration:** 3 hours | **Prerequisite:** Week 9 lecture

## Objectives
Practice declaring and implementing interfaces, implementing multiple interfaces in one class,
and using a default method.

## Setup
Create `Lab09.java` (and any supporting classes) in your working folder.

## Procedure
1. **Task A — Declare an interface:** design `interface Drivable` with `void accelerate()` and
   `void brake()`.
2. **Task B — Implement it in unrelated classes:** implement `Drivable` in two classes that do
   not extend each other or any common custom superclass, e.g. `class Car implements Drivable`
   and `class Bicycle implements Drivable`.
3. **Task C — Multiple interfaces:** add a second interface `interface Lockable` with `void
   lock()`/`void unlock()`, and have `Car` implement both `Drivable` and `Lockable`.
4. **Task D — Default method:** add a `default` method to `Drivable`, e.g. `default void
   honk() { System.out.println("Beep!"); }`, and call it on a `Car` instance without writing any
   `honk()` code in `Car` itself.

## Expected Output
A project with `Drivable`, `Lockable`, `Car`, and `Bicycle`, compiling cleanly with `javac`,
demonstrating a method `static void testDrive(Drivable d)` called polymorphically on both `Car`
and `Bicycle`.

## Submission
Submit your `.java` files via the course submission system by the end of the lab session.
