# Week 8 Summary — Pointers & References; Midterm Review

**Key takeaways:**
- A pointer stores a memory address; `&` takes an address, `*` dereferences a pointer to access
  the value it points to. Always initialize pointers (`nullptr` if no valid address yet).
- An array decays to a pointer to its first element; pointer arithmetic and `[]` indexing both
  work on it.
- References must be bound at declaration and can never be null or reseated; pointers can be
  null and reseated — use references when a valid value is guaranteed, pointers when "no value"
  is meaningful.
- The midterm (Week 9) covers Weeks 1–7: basics, operators, control flow, loops, functions,
  arrays, strings, and pointers/references.

**You should now be able to:** declare and use pointers safely; explain pointers vs. references;
solve representative problems from Weeks 1–7 under exam conditions.

**Next week:** the midterm exam, followed by dynamic memory allocation (`new`/`delete`).
