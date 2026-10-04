# Week 7 Summary — 2D Arrays and Strings

**Key takeaways:**
- 2D arrays are indexed `[row][column]` and stored row-major in memory; nested loops are the
  standard tool for processing them.
- A C-string is a `char[]` terminated by `'\0'`, manipulated with `<cstring>` functions like
  `strlen`/`strcmp`; it requires careful manual size management.
- `std::string` manages its own size, supports `+` concatenation, `==` content comparison,
  `substr`, and `find`, and is strongly preferred in modern C++.

**You should now be able to:** declare and process a 2D array with nested loops; distinguish
C-strings from `std::string`; perform common `std::string` operations.

**Next week:** pointers and references — how C++ exposes and names memory addresses directly, and
midterm review.
