# Lab Manual 1 — C++ Basics

**Duration:** 3 hours | **Prerequisite:** Week 1 lecture

## Objectives
Practice compiling and running C++ programs, and using variables, primitive types, and basic I/O.

## Setup
1. Confirm a C++17 compiler is available: `g++ --version` (or `clang++ --version`).
2. Create a working folder `lab01/` and open it in VS Code (or your editor of choice).
3. Create `lab01.cpp`.

## Procedure
1. **Task A — Hello, World:** write and compile a program that prints `"Hello, C++!"`. Verify it
   compiles with `g++ -std=c++17 -Wall lab01.cpp -o lab01` and runs with `./lab01`.
2. **Task B — Variables:** declare an `int`, a `double`, a `char`, and a `bool`, initialize each,
   and print all four with labels.
3. **Task C — Basic I/O:** read a rectangle's `width` and `height` (as `double`) from the user
   with `std::cin`, compute the area and perimeter, and print both with labels.
4. **Task D — Mini-challenge:** read a temperature in Celsius from the user and print the
   Fahrenheit equivalent (`F = C * 9.0 / 5.0 + 32.0`).

## Expected Output
A single source file with four clearly labeled sections (A–D), each compiling cleanly with
`-Wall` and producing correct output.

## Submission
Submit `lab01.cpp` via the course submission system by the end of the lab session (or per
instructor-announced deadline).
