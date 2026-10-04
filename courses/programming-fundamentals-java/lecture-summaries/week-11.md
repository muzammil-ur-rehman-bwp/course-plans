# Week 11 Summary — File I/O and Exceptions

**Key takeaways:**
- `Scanner` can read from a `File` exactly as it reads from `System.in`; `BufferedReader` reads
  line-by-line with `readLine()`, returning `null` at end of file.
- `PrintWriter`/`FileWriter` write text files; by default this overwrites an existing file unless
  append mode is requested.
- Checked exceptions (`IOException`, `FileNotFoundException`) must be caught or declared;
  `try`/`catch`/`finally` and try-with-resources manage this and ensure files are closed properly.
- Persisting data is a save-then-reload pattern: write one record per line, then parse it back
  the same way on the next run.

**You should now be able to:** read and write text files in Java; handle missing-file and other
I/O errors gracefully instead of crashing; implement a basic save/reload round trip.

**This week:** Quiz 5 (classes, `ArrayList` & file I/O) — see `quizzes/quiz-05.md`. Capstone
proposal due — see `assignments/capstone-proposal-guidelines.md`.

**Next week:** recursion — solving problems in terms of themselves.
