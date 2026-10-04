# Week 12 Summary — Exception Handling

**Key takeaways:**
- `throw` raises an exception that propagates up the call stack to the nearest matching `catch`,
  unwinding the stack (running local destructors) along the way — a form of RAII cleanup.
- `<stdexcept>` provides a standard hierarchy (`std::exception`, `std::runtime_error`,
  `std::invalid_argument`, `std::out_of_range`, etc.); throw the most specific type that fits.
- A custom exception class typically derives from `std::exception`/`std::runtime_error`, adding
  its own data and overriding `what()` as needed.
- Catch by reference, never by value (value catching slices derived exceptions, Week 8's bug
  resurfacing here); order `catch` clauses most-specific first; `catch (...)` is a last resort.

**You should now be able to:** validate input by throwing appropriate standard or custom
exceptions, and write ordered, reference-based `catch` clauses that handle them correctly.

**Next week:** introduction to the STL — `std::vector`, `std::map`, and iterators, bridging
directly from this semester's templates.
