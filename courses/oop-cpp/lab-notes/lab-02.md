# Lab Notes 2 — Constructors & Destructors

**Concept recap:** member initializer lists construct members directly and are required for
`const`/reference members; a destructor runs automatically at end of lifetime; the Rule of Three
says a custom destructor, copy constructor, or copy-assignment operator usually implies needing
all three, for a class that owns a resource.

**Common pitfalls:**
- Using assignment in the constructor body instead of the initializer list — works for simple
  types, but is a habit that fails outright for `const`/reference members later.
- Writing `IntBuffer`'s destructor but forgetting the copy constructor — the compiler-generated
  one will shallow-copy the pointer, and both objects' destructors will `delete[]` the *same*
  memory (a double free) when they go out of scope.
- Using `delete` instead of `delete[]` on an array allocated with `new[]`, or vice versa —
  mismatching `new`/`delete[]` forms is undefined behavior even when it happens not to crash.
- Forgetting that the copy constructor must allocate its **own** array (`new int[size]`) and copy
  each element, not just copy the pointer value.

**Debugging tip:** if a program crashes or behaves inconsistently only sometimes, especially after
copying an object, suspect a shallow copy of an owning pointer first — add print statements in
the constructor/destructor to trace exactly how many times each is called and on which address.

**Instructor tip:** deliberately comment out the copy constructor in a working `IntBuffer` and
run under a memory checker if available, to show students the double-free/heap-corruption error
directly rather than only describing it.
