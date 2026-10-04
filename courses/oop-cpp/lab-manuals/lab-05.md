# Lab Manual 5 — Composition

**Duration:** 3 hours | **Prerequisite:** Week 5 lecture

## Objectives
Practice designing classes composed of other classes as data members, with correct construction/
destruction order.

## Setup
Create `lab05.cpp` in your working folder.

## Procedure
1. **Task A — A composed member:** write `class Engine` with a private `horsepower_` and a
   `start() const` method; write `class Car` composed of an `Engine` and a `std::string model_`,
   with a constructor that initializes both via the member initializer list.
2. **Task B — Order tracing:** add a print statement in `Engine`'s constructor/destructor and in
   `Car`'s constructor/destructor (body, not initializer list); construct and destroy a `Car` and
   observe/record the printed order in a comment.
3. **Task C — Multiple composed members:** add a second member class `WheelSet` (count of
   wheels) to `Car`, and a `describe() const` method reporting model, horsepower, and wheel count.
4. **Task D — Mini-challenge:** write `class Garage` composed of a `std::vector<Car>`, with an
   `addCar(const Car&)` method and a `describeAll() const` method that calls `describe()` on
   every car.

## Expected Output
A single source file with four clearly labeled sections (A–D), compiling cleanly with `-Wall`,
with Task B's comment correctly matching the actual printed construction/destruction order.

## Submission
Submit `lab05.cpp` via the course submission system by the end of the lab session.
