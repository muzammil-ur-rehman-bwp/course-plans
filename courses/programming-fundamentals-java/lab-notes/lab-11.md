# Lab Notes 11 — File I/O

**Concept recap:** `Scanner`/`BufferedReader` read files, `PrintWriter`/`FileWriter` write them;
checked exceptions (`IOException`, `FileNotFoundException`) must be caught or declared;
try-with-resources automatically closes file resources.

**Common pitfalls:**
- Forgetting to catch (or declare with `throws`) `IOException`/`FileNotFoundException` — this is
  a compile error in Java, not a runtime surprise, which is actually a safety feature worth
  pointing out.
- Opening a file for writing with `PrintWriter`/`FileWriter` and not realizing it **overwrites**
  the existing file by default, losing previously saved data.
- Forgetting to `.close()` a file resource (or not using try-with-resources) — on some platforms
  this can leave the file locked or its last writes unflushed and missing from disk.
- Reading with `Scanner`'s `.next()`/`.nextInt()` when the file's tokens don't match the expected
  type/order, causing an `InputMismatchException`; or using `.nextLine()` interchangeably with
  `.next()` without accounting for leftover newlines.

**Debugging tip:** if a written file appears empty or truncated, check that the writer was
actually closed (or used in a try-with-resources block) before the program exited.

**Instructor tip:** have students open the same file for both reading and writing in the same
run (a common capstone pattern) to directly observe why the save step must fully complete and
close *before* the reload step opens the same file.
