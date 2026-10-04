# Assignment 3 — File I/O, Exceptions, and Recursion (Weeks 11–12)

**Weight:** 5% of course grade (one of 4 assignments, 20% total) | **Assigned:** Week 13 | **Due:** Start of Week 15

## Instructions
Submit a single source file `Assignment03.java`, plus any sample data file(s) it reads or
produces. Code must compile cleanly with `javac` and must handle every file operation's
exceptions (no uncaught checked exceptions).

## Questions
1. **(File I/O — write, 15 pts)** Write a method `static void saveInventory(String filename,
   String[] names, int[] quantities, int size)` that writes each `name quantity` pair to the
   given file, one per line, using `PrintWriter`.
2. **(File I/O — read, 20 pts)** Write a method `static int loadInventory(String filename,
   String[] names, int[] quantities, int maxSize)` that reads records back from a file into the
   given parallel arrays (up to `maxSize` records) and returns how many were actually read.
   Catching `FileNotFoundException` and handling a missing file gracefully (printing an error,
   returning 0) is required for credit.
3. **(File I/O — round trip, 15 pts)** In `main`, build a small inventory in memory, save it
   with Question 1's method, then load it back with Question 2's method into fresh arrays, and
   print both the original and reloaded data to show they match.
4. **(Recursion, 25 pts)** Write a recursive method `static boolean isSorted(int[] values, int
   index)` that returns whether an array is sorted in non-decreasing order, with no loops. State
   its base case and recursive case in a comment.
5. **(Recursion, 25 pts)** Write a recursive method `static int gcd(int a, int b)` implementing
   Euclid's algorithm (`gcd(a, 0) = a`; `gcd(a, b) = gcd(b, a % b)` for `b != 0`). Test it on at
   least 4 pairs, including one where `a < b`.

## Submission
Upload `Assignment03.java` (and any sample data files) via the course submission system. Late
policy per syllabus (`course-plan.md` §7).
