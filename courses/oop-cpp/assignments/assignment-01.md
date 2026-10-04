# Assignment 1 — Encapsulation, Constructors/Destructors, Operator Overloading I (Weeks 1–3)

**Weight:** 5% of course grade (one of 4 assignments, 20% total) | **Assigned:** Week 4 | **Due:** Start of Week 6

## Instructions
Submit a single source file `assignment01.cpp`. Code must compile cleanly with
`g++ -std=c++17 -Wall` and must not leak memory (no raw owning pointers are required for this
assignment — the Rule of Three questions below use explicit, deliberate examples instead).

## Questions
1. **(Encapsulation, 15 pts)** Write `class BankAccount` with a private `double balance_`; a
   constructor that rejects a negative starting balance (clamp to `0`); public `deposit(double)`,
   `withdraw(double)` (returning `bool` for success/failure), and a `const` `getBalance()`.
2. **(Constructors, 20 pts)** Write `class Student` with private `std::string name_`, `int
   id_`, and a `std::vector<double> grades_`. Provide a default constructor, a parameterized
   constructor (name and id only, empty grades), and a copy constructor that performs a correct
   deep copy (note: `std::vector`'s own copy constructor already deep-copies — your `Student`
   copy constructor must still use a correct member initializer list).
3. **(Destructors & Rule of Three, 20 pts)** Write `class RawIntArray` that owns a dynamically
   allocated `int[]` via `new`, with a correct destructor, a correct deep-copying copy
   constructor, and a brief comment explaining what would go wrong (and why) if the copy
   constructor were omitted.
4. **(Operator overloading, 20 pts)** Write `class Money` (storing cents as an `int` internally,
   to avoid floating-point rounding issues) with overloaded `operator+`, `operator-` (returning a
   new `Money`, non-mutating), `operator+=` (mutating), and `operator==`/`operator<`
   (consistent with each other).
5. **(Integration, 25 pts)** In `main`, create at least 3 `BankAccount` objects and 2 `Student`
   objects demonstrating valid and invalid inputs; create 2 `RawIntArray` objects and copy one
   into a new object, modifying the copy to prove it's independent; create several `Money` values
   and print the results of each overloaded operator.

## Submission
Upload `assignment01.cpp` via the course submission system. Late policy per syllabus
(`course-plan.md` §7).
