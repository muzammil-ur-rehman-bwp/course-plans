# Lab Manual 14 — UML & Design Critique

**Duration:** 3 hours | **Prerequisite:** Week 14 lecture

## Objectives
Practice drawing a basic UML class diagram and critiquing a class design against SRP and OCP.

## Setup
Create a diagram file (image or ASCII, your choice) plus `Lab14.java` for the refactoring tasks.

## Procedure
1. **Task A — UML diagram:** draw a UML class diagram for a hierarchy from an earlier week (e.g.
   `Shape`/`Circle`/`Rectangle` from Week 8), showing attributes, methods, and the inheritance
   arrow.
2. **Task B — Composition in UML:** add a `Car`/`Engine` composition relationship to the same
   diagram, using a diamond connector.
3. **Task C — SRP refactor:** given a provided `Report` class mixing calculation, formatting, and
   storage in one class, refactor it into three smaller classes, each with one responsibility.
4. **Task D — OCP refactor:** given a provided `totalArea(List<Object> shapes)` method using an
   `instanceof` chain, refactor it to use polymorphism instead, requiring no edits for a
   hypothetical new shape type.

## Expected Output
A UML diagram (Tasks A–B) and a refactored project (Tasks C–D) compiling cleanly with `javac`.

## Submission
Submit your diagram file and `.java` files via the course submission system by the end of the
lab session.
