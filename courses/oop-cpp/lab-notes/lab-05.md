# Lab Notes 5 — Composition

**Concept recap:** a composed member object is constructed in declaration order before the
enclosing constructor's body runs, and destroyed in reverse order; initialize a composed member
through the member initializer list, passing whatever arguments its constructor needs.

**Common pitfalls:**
- Trying to initialize a composed member in the constructor body instead of the initializer
  list, when the member has no default constructor — this fails to compile (the member would
  need to be default-constructed first, which isn't possible).
- Assuming a composed member is constructed in the order written in the initializer list rather
  than the order declared in the class — these can silently differ and only the declaration order
  actually governs construction.
- Confusing composition (owning a member object) with simply storing a pointer to an object
  owned elsewhere — only the former ties the member's lifetime to the owner's constructor/
  destructor.
- Forgetting that `Garage`'s `std::vector<Car>` will copy-construct `Car` objects on `push_back`
  unless move semantics apply — make sure `Car`'s (compiler-generated or custom) copy behavior is
  actually correct for this to work safely.

**Debugging tip:** when construction/destruction order isn't what you expect, print the class's
member declaration order in a comment next to the class definition and compare it directly
against the trace output from Task B.

**Instructor tip:** deliberately declare members in `Car` in a different order than the
initializer list lists them, compile with warnings enabled, and show the `-Wreorder` warning GCC/
Clang gives — a good, concrete demonstration that declaration order, not initializer-list order,
governs construction.
