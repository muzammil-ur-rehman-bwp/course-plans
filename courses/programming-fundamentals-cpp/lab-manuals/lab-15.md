# Lab Manual 15 — Debugging Exercise + Capstone Work Session

**Duration:** 3 hours | **Prerequisite:** Week 15 lecture

## Objectives
Practice using compiler warnings and a debugger to isolate and fix bugs, apply const-correctness,
and make progress on the capstone project.

## Setup
1. Download/copy the instructor-provided buggy program `buggy.cpp` into your working folder.
2. Ensure you can run a debugger (`gdb` or your IDE's integrated debugger).

## Procedure
1. **Task A — Compiler warnings:** compile `buggy.cpp` with `g++ -std=c++17 -Wall -Wextra` and
   list every warning produced, with the line number and a one-sentence explanation of what each
   one means.
2. **Task B — Fix with a debugger:** use breakpoints and stepping to confirm the runtime
   behavior of at least one bug that warnings alone did not fully explain; fix all identified
   bugs in `buggy.cpp`.
3. **Task C — Const-correctness pass:** add `const` to every parameter/reference in the fixed
   program that is never modified, and confirm it still compiles cleanly.
4. **Task D — Capstone work session:** use remaining lab time to work on your capstone project
   (proposal should already be approved from Week 11); check in with the instructor/TA on current
   progress and any blockers.

## Expected Output
A corrected, const-correct `buggy.cpp` that compiles cleanly with `-Wall -Wextra` and runs
correctly; a short written list of the bugs found and fixed (Task A); visible capstone progress
to show the instructor/TA (Task D).

## Submission
Submit the corrected `buggy.cpp` and the bug list via the course submission system by the end of
the lab session. This is the final lab of the semester — Week 16 is capstone presentations.
