# Lab Manual 11 — File I/O

**Duration:** 3 hours | **Prerequisite:** Week 11 lecture

## Objectives
Practice writing and reading text files, including structured data, with `ofstream`/`ifstream`.

## Setup
Create `lab11.cpp` in your working folder.

## Procedure
1. **Task A — Write a file:** write a program that writes 5 lines of `name score` pairs to
   `scores.txt` using `ofstream`, checking `is_open()` before writing.
2. **Task B — Read a file:** write a separate section that opens `scores.txt` with `ifstream`,
   checks `is_open()`, reads all `name score` pairs with `>>` in a loop, and prints them.
3. **Task C — Structured records:** define `struct Student { std::string name; int score; };`,
   read `scores.txt` into an array of `Student`, and print the average score.
4. **Task D — Mini-challenge:** extend Task C to also write the roster back out to
   `scores_sorted.txt` after sorting it by score (a simple selection sort on the struct array is
   fine, even before Week 14's formal coverage).

## Expected Output
A single source file with four clearly labeled sections (A–D); `scores.txt` must exist (either
provided or created by Task A) before Tasks B–C run, each compiling cleanly with `-Wall`.

## Submission
Submit `lab11.cpp` (and any data files it produces) via the course submission system by the end
of the lab session.
