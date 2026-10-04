# Lab Manual 11 — File I/O

**Duration:** 3 hours | **Prerequisite:** Week 11 lecture

## Objectives
Practice writing and reading text files, including structured data, with `PrintWriter`/`Scanner`.

## Setup
Create `Lab11.java` in your working folder.

## Procedure
1. **Task A — Write a file:** write a program that writes 5 lines of `name score` pairs to
   `scores.txt` using `PrintWriter`, wrapped in `try`/`catch` for `IOException`.
2. **Task B — Read a file:** open `scores.txt` with `Scanner`, wrapped in `try`/`catch` for
   `FileNotFoundException`, read all `name score` pairs with `next()`/`nextInt()` in a loop, and
   print them.
3. **Task C — Structured records:** define a simple `Student` class (`name`, `score`), read
   `scores.txt` into an `ArrayList<Student>`, and print the average score.
4. **Task D — Mini-challenge:** extend Task C to also write the roster back out to
   `scores_sorted.txt` after sorting it by score (a simple selection sort on the `Student` list is
   fine, even before Week 14's formal coverage).

## Expected Output
A single source file with four clearly labeled sections (A–D); `scores.txt` must exist (either
provided or created by Task A) before Tasks B–C run, each compiling cleanly.

## Submission
Submit `Lab11.java` (and any data files it produces) via the course submission system by the end
of the lab session.
