# Lab Manual 4 — Static vs. Instance Members

**Duration:** 3 hours | **Prerequisite:** Week 4 lecture

## Objectives
Practice distinguishing static from instance members, static initialization, and writing a
static utility class.

## Setup
Create `Lab04.java` (and any supporting classes) in your working folder.

## Procedure
1. **Task A — Instance counter:** add a `private static int instanceCount` field to a `Student`
   class, increment it in every constructor, and add `public static int getInstanceCount()`.
2. **Task B — Static initialization:** add a `private static final Map<String, String> DEFAULTS`
   field to a `Config` class, populated inside a `static { ... }` block.
3. **Task C — Utility class:** design `final class Grades` with a `private` constructor and a
   `public static String letterGrade(double score)` method returning `"A"`/`"B"`/`"C"`/`"D"`/
   `"F"`.
4. **Task D — Mini-challenge:** write a comment explaining why `Grades.letterGrade(...)` cannot
   directly access a `Student` instance's fields, and why that is by design.

## Expected Output
A project with `Student`, `Config`, and `Grades` classes compiling cleanly with `javac`,
demonstrating the static counter incrementing correctly across several constructed objects.

## Submission
Submit your `.java` files via the course submission system by the end of the lab session.
