# Week 11 Summary — File I/O

**Key takeaways:**
- `std::ofstream` writes to a file, `std::ifstream` reads from one; always check `is_open()`
  before using either.
- `>>` reads whitespace-separated tokens (like `std::cin`); `getline` reads a whole line,
  including spaces.
- The `while (stream >> var)` idiom reads until extraction fails (including end-of-file).
- Combining file I/O with structs and arrays lets a program persist structured records between
  runs — the foundation of the capstone project.

**You should now be able to:** write and read text files with `ofstream`/`ifstream`; choose
`getline` vs. `>>` appropriately; load file data into an array of structs.

**Next week:** recursion — functions that call themselves to solve a problem in terms of smaller
instances of itself.
