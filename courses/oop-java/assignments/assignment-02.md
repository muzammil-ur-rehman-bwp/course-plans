# Assignment 2 — Static Members, Inheritance I & II (Weeks 4–7)

**Weight:** 5% of course grade (one of 4 assignments, 20% total) | **Assigned:** Week 8 | **Due:** Start of Week 10

## Instructions
Submit a single project containing all classes described below. Code must compile cleanly with
`javac` with no warnings.

## Questions
1. **(Static members, 15 pts)** Write `class Employee` with a `private static int
   employeeCount`, incremented in every constructor, and a `public static int
   getEmployeeCount()`. Separately, write a static utility class `PayrollUtils` (private
   constructor) with a static method `double applyTaxRate(double gross, double rate)`.
2. **(Inheritance I, 20 pts)** Write `abstract class Vehicle` (not yet abstract methods — just a
   `protected String licensePlate` and a constructor), and `class Car extends Vehicle` whose
   constructor chains via `super(licensePlate)`, adding a `private int numDoors` field.
3. **(Access control, 15 pts)** Add a `private String vin` field to `Vehicle`. Write a method in
   `Car` that attempts to access `vin` directly and record, in a comment, the exact compiler
   error; then remove the attempt and access it correctly via a `protected`/`public` accessor you
   add to `Vehicle`.
4. **(Inheritance II, 25 pts)** Write `class Animal` with `public String makeSound()`, and two
   subclasses `Dog`/`Cat` that correctly override it with `@Override`. Add an overloaded
   `makeSound(int times)` to `Dog` and explain, in a comment, why this does not replace the
   overridden version.
5. **(Integration, 25 pts)** In `main`, create several `Employee` and `Car` objects demonstrating
   the static counter and constructor chaining; create an `Animal[]` holding a mix of `Dog` and
   `Cat`, and loop over it calling `makeSound()` polymorphically on each.

## Submission
Upload your project via the course submission system. Late policy per syllabus
(`course-plan.md` §7).
