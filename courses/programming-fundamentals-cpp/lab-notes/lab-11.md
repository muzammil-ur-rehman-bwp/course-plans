# Lab Notes 11 — File I/O

**Concept recap:** `ofstream` writes, `ifstream` reads; always check `is_open()` before using a
file stream; `>>` reads whitespace-separated tokens, `getline` reads a whole line; the
`while (stream >> var)` idiom reads until extraction fails (including end-of-file).

**Common pitfalls:**
- Not checking whether the file opened successfully — if it didn't (wrong path, missing file,
  permissions), all subsequent reads/writes silently fail or produce garbage without this check.
- Forgetting the file must exist (or be creatable) relative to where the *program* runs from, not
  where the source file is edited — a common "file not found" surprise.
- Mixing `>>` and `getline` on the same stream without accounting for the leftover newline
  character `>>` leaves behind, which `getline` then reads as an empty line.
- Overflowing a fixed-size array of structs if the file has more records than the array's
  capacity — always bound the read loop by the array's size, not just end-of-file.

**Debugging tip:** open the data file in a text editor alongside running the program — comparing
exactly what's on disk to what the program printed after reading it quickly reveals a parsing
mismatch (e.g., extra spaces, wrong column order).

**Instructor tip:** deliberately point the program at a non-existent file once, with the
`is_open()` check removed, to show the silent-failure consequences before restoring the check.
