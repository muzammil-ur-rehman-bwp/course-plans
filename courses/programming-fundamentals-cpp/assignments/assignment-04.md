# Assignment 4 — Intro Classes, Sorting & Searching (Weeks 13–14)

**Weight:** 5% of course grade (one of 4 assignments, 20% total) | **Assigned:** Week 14 | **Due:** Start of Week 16

## Instructions
Submit a single source file `assignment04.cpp`. Code must compile cleanly with
`g++ -std=c++17 -Wall`.

## Questions
1. **(Classes, 20 pts)** Design and implement `class Temperature` storing a value internally in
   Celsius (private). Provide a constructor taking a Celsius value, and public methods
   `toFahrenheit() const` and `toCelsius() const`. Demonstrate it with at least 3 test values.
2. **(Classes, 20 pts)** Design and implement `class Stack` backed by a fixed-size private
   `int` array (capacity 20) and a private `int topIndex`. Provide `push(int)`, `pop()` (removing
   and returning the top value; handle the empty-stack case), `isEmpty() const`, and `isFull()
   const`. Demonstrate pushing, popping, and both edge cases (popping empty, pushing full).
3. **(Sorting, 20 pts)** Implement `void selectionSort(int values[], int size)` and use it to
   sort an array of at least 10 integers read from the user; print before and after.
4. **(Searching, 20 pts)** Implement `int binarySearch(const int values[], int size, int
   target)`. Using the sorted array from Question 3, search for 3 values (one guaranteed present,
   two likely absent) and print the result of each.
5. **(Integration, 20 pts)** Combine Questions 3–4: write a function `bool exists(int values[],
   int size, int target)` that sorts the array (reusing Question 3) if it is not already sorted,
   then searches it (reusing Question 4), returning whether `target` was found. Justify in a
   comment why sorting first is worth it if many searches will be performed on the same data.

## Submission
Upload `assignment04.cpp` via the course submission system. Late policy per syllabus
(`course-plan.md` §7).
