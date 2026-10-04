# Week 9 Summary — Midterm + Dynamic Memory

**Key takeaways:**
- The midterm exam covered Weeks 1–8: basics through pointers/references.
- Stack memory is automatic and scope-tied; heap memory is manually managed with `new`/`delete`
  and can outlive the function that allocated it.
- Every `new` needs exactly one `delete`; every `new[]` needs exactly one `delete[]` — mismatching
  forms or forgetting to free causes leaks or undefined behavior.
- A dangling pointer (one that still holds an address that has been freed) must never be used;
  setting it to `nullptr` after `delete` makes accidental reuse fail safely.

**You should now be able to:** explain stack vs. heap; allocate and correctly free single
objects and dynamic arrays; identify a memory leak or dangling pointer in a code snippet.

**Next week:** structs — grouping related data of different types into a single user-defined
type.
