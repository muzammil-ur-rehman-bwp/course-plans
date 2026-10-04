# Assignment 2 — Streams/`friend`, Composition, Inheritance I & II (Weeks 4–7)

**Weight:** 5% of course grade (one of 4 assignments, 20% total) | **Assigned:** Week 8 | **Due:** Start of Week 10

## Instructions
Submit a single source file `assignment02.cpp`. Code must compile cleanly with
`g++ -std=c++17 -Wall`.

## Questions
1. **(Streams & `friend`, 20 pts)** Using the `Money` class from Assignment 1 (or a fresh
   equivalent), implement `operator<<`/`operator>>` as `friend` functions, formatting as a dollar
   amount (e.g. `$12.50`), correctly chainable and correctly rejecting malformed input via
   `setstate`.
2. **(Composition, 20 pts)** Write `class Engine` (horsepower) and `class Transmission` (number
   of gears), then `class Car` composed of both, with a constructor correctly initializing both
   via the member initializer list and a `describe() const` method reporting all three classes'
   data.
3. **(Inheritance I, 20 pts)** Write `class Employee` with a `protected std::string name_` and
   `protected double baseSalary_`, and a `virtual double computeSalary() const` returning
   `baseSalary_`. Write `class Manager : public Employee` adding a `bonus_`, chaining to
   `Employee`'s constructor, and overriding `computeSalary()` to add the bonus — mark the
   override with `override`, and give `Employee` a `virtual` destructor even though this
   assignment does not yet test dynamic dispatch through a base pointer directly (Weeks 8–9 will).
4. **(Inheritance II, 20 pts)** Add a second derived class `class Executive : public Manager`
   that further overrides `computeSalary()` to add a stock-grant value on top of `Manager`'s
   result (calling `Manager::computeSalary()` explicitly inside the override, not duplicating its
   logic).
5. **(Integration, 20 pts)** In `main`, build a `std::vector<Employee*>` containing an
   `Employee`, a `Manager`, and an `Executive` (heap-allocated with `new`, matched with `delete`),
   and print each one's `computeSalary()` through the loop, confirming each uses its own override.

## Submission
Upload `assignment02.cpp` via the course submission system. Late policy per syllabus
(`course-plan.md` §7).
