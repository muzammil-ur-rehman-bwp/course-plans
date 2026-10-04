# Lab Manual 5 — Inheritance I

**Duration:** 3 hours | **Prerequisite:** Week 5 lecture

## Objectives
Practice `extends`, access control across inheritance, and constructor chaining with
`super(...)`.

## Setup
Create `Lab05.java` (and any supporting classes) in your working folder.

## Procedure
1. **Task A — Base class:** design `class Vehicle` with a `protected String licensePlate` field
   and a constructor accepting it.
2. **Task B — Subclass:** design `class Car extends Vehicle` with an additional `private int
   numDoors` field, whose constructor chains to `Vehicle`'s via `super(licensePlate)`.
3. **Task C — Access check:** add a `private String vin` field to `Vehicle` and, in a comment
   inside `Car`, explain why `Car` cannot access `vin` directly, contrasting it with
   `licensePlate`.
4. **Task D — Mini-challenge:** design a second subclass `class Motorcycle extends Vehicle` and
   write a method `static void printPlate(Vehicle v)` that works correctly for both `Car` and
   `Motorcycle` objects, demonstrating that a subclass is usable wherever its superclass is
   expected.

## Expected Output
A project with `Vehicle`, `Car`, and `Motorcycle` classes compiling cleanly with `javac`,
demonstrating correct constructor chaining and access control.

## Submission
Submit your `.java` files via the course submission system by the end of the lab session.
