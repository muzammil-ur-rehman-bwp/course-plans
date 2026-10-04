# Lab Manual 10 — Structs

**Duration:** 3 hours | **Prerequisite:** Week 10 lecture

## Objectives
Practice defining a struct, building an array of structs, and passing structs to functions.

## Setup
Create `lab10.cpp` in your working folder.

## Procedure
1. **Task A — Define & use:** define `struct Student { std::string name; int age; double gpa; };`
   create one `Student`, fill its fields, and print them.
2. **Task B — Array of structs:** declare a 5-element array of `Student`, fill it with user
   input (or hardcoded test data), and print all records in a formatted table.
3. **Task C — Processing functions:** write `double averageGpa(const Student roster[], int
   size)` and `int countAbove(const Student roster[], int size, double threshold)`; call both on
   the array from Task B.
4. **Task D — Mini-challenge:** write `void raiseGpa(Student& s, double amount)` that modifies a
   single student's GPA by reference, and `Student findTopStudent(const Student roster[], int
   size)` that returns (by value) the record with the highest GPA.

## Expected Output
A single source file with four clearly labeled sections (A–D), each compiling cleanly with
`-Wall` and producing correctly formatted output.

## Submission
Submit `lab10.cpp` via the course submission system by the end of the lab session.
