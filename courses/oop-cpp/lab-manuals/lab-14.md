# Lab Manual 14 — UML Diagramming & SOLID Refactoring

**Duration:** 3 hours | **Prerequisite:** Week 14 lecture

## Objectives
Practice reading/drawing UML class diagrams and refactoring a class against SRP and OCP.

## Setup
Create `lab14.cpp` for the code tasks, and a `lab14-diagram` file (image, PDF, or plain-text ASCII
diagram as shown in lecture) for the diagramming task.

## Procedure
1. **Task A — Draw a diagram:** sketch a UML class diagram for the `Shape`/`Circle`/`Rectangle`
   hierarchy from Week 9, plus a `ShapeCollection` class composed of a `std::vector<Shape*>` —
   include correct attribute/method notation and the correct arrow types (inheritance vs.
   composition).
2. **Task B — SRP refactor:** given a provided class `ReportGenerator` that reads data, computes
   an average, and prints a report all in one class, split it into `DataReader`,
   `StatisticsCalculator`, and `ReportPrinter`, each with a single responsibility.
3. **Task C — OCP refactor:** given a provided function `double areaOf(const std::string&
   shapeType, double dim1, double dim2)` using an `if`/`else if` chain on a type string, refactor
   the computation to use the `Shape` hierarchy from Task A instead (polymorphism, no type
   string).
4. **Task D — Mini-challenge:** add a new shape type (e.g. `Triangle`) to your Task C solution and
   confirm, in a comment, that no existing code needed to change — only a new class was added.

## Expected Output
`lab14-diagram` showing a correct UML diagram; `lab14.cpp` with Tasks B–D compiling cleanly with
`-Wall` and producing correct results.

## Submission
Submit both `lab14.cpp` and `lab14-diagram` via the course submission system by the end of the
lab session.
