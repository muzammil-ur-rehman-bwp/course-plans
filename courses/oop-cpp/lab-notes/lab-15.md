# Lab Notes 15 — Smart Pointers & Debugging

**Concept recap:** `std::unique_ptr<T>` gives exclusive, move-only ownership with automatic
cleanup; `std::shared_ptr<T>` allows multiple owners via reference counting, freeing the object
when the count reaches zero; default to `unique_ptr`, use `shared_ptr` only when ownership is
genuinely shared; a non-owning raw pointer/reference is still correct for observing an object you
don't own.

**Common pitfalls:**
- Mixing a raw `new` with a smart pointer's managed deletion (e.g. also calling `delete` manually
  on a pointer a `unique_ptr` already owns) — causes a double free; let the smart pointer own the
  memory exclusively, with no manual `delete` anywhere.
- Using `shared_ptr` by default "to be safe," when `unique_ptr` (or even a plain reference) would
  be correct and cheaper — reserve `shared_ptr`'s reference-counting overhead for genuinely shared
  ownership.
- Creating a `shared_ptr` from a raw pointer more than once for the same object (e.g.
  `std::shared_ptr<Texture> a(rawPtr); std::shared_ptr<Texture> b(rawPtr);`) instead of copying an
  existing `shared_ptr` — this creates two independent reference counts, each eventually freeing
  the same object, a double free; always share by **copying** a `shared_ptr`, never by
  re-wrapping the same raw pointer.
- Writing test functions that only check the "happy path," never an edge case (empty container,
  invalid input) or a polymorphic call through a base pointer — exactly the cases most likely to
  hide a bug from Weeks 2, 8, or 12.

**Debugging tip:** if a `shared_ptr`'s `use_count()` is lower than expected, check for an
accidental copy made by value instead of by reference somewhere, temporarily inflating and then
deflating the count as the temporary copy is destroyed.

**Instructor tip:** show the double-free crash from constructing two independent `shared_ptr`s
around the same raw pointer, if a sanitizer/debugger is available — contrasted directly with the
correct, safe behavior of copying one `shared_ptr` into another.
