# Lab Notes 15 — Debugging Exercise + Capstone Work Session

**Concept recap:** `-Wall -Wextra` surfaces likely bugs at compile time; a debugger lets you pause
execution and inspect real program state (breakpoints, stepping, watched variables); `const` on a
read-only parameter is a compiler-enforced promise, not decoration.

**Common pitfalls:**
- Suppressing or ignoring a warning instead of understanding and fixing its root cause — the
  warning usually points at a real, if subtle, bug (uninitialized variable, signed/unsigned
  comparison, etc.).
- Trying to fix a bug purely by reading code when the debugger would show the actual values in
  seconds — for a bug that resists several minutes of reading, switch to the debugger instead of
  continuing to stare at the source.
- Adding `const` everywhere mechanically without checking it still compiles — a parameter that
  genuinely needs to be modified will correctly fail to compile once marked `const`, which is the
  signal to remove it from that one case, not evidence the pass was wrong.
- Treating this lab's capstone work time as optional — many memory-management and file-I/O bugs
  in capstone projects surface only once students have more "real" code than the smaller lab
  exercises, making this check-in valuable.

**Debugging tip:** when using `gdb` (or an IDE debugger), set a breakpoint at the *first* line
where something looks wrong, not at `main` — stepping through correct code wastes time that
should go toward the actual bug.

**Instructor tip:** use the Task D check-in to specifically ask each student/pair whether their
capstone's `new`/`delete` pairs and file-open checks are already in place — these are the most
common sources of late-semester capstone bugs.
