# Lab Manual 14 — Sorting & Searching Algorithms

**Duration:** 3 hours | **Prerequisite:** Week 14 lecture

## Objectives
Practice implementing bubble sort, selection sort, linear search, and binary search on arrays.

## Setup
Create `lab14.cpp` in your working folder.

## Procedure
1. **Task A — Bubble sort:** implement `void bubbleSort(int values[], int size)`; test on an
   array of at least 8 unsorted integers, printing before and after.
2. **Task B — Selection sort:** implement `void selectionSort(int values[], int size)`; test on
   a different unsorted array of at least 8 integers.
3. **Task C — Linear search:** implement `int linearSearch(const int values[], int size, int
   target)`; test with a present and an absent target on an unsorted array.
4. **Task D — Binary search:** implement `int binarySearch(const int values[], int size, int
   target)`; sort an array first (reuse Task A or B), then test with a present and an absent
   target, and count how many comparisons each search makes to illustrate the difference.

## Expected Output
A single source file with four clearly labeled sections (A–D), each compiling cleanly with
`-Wall`, printing the array before/after sorting and the result (found index or "not found") for
each search test.

## Submission
Submit `lab14.cpp` via the course submission system by the end of the lab session.
