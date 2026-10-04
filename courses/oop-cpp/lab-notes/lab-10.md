# Lab Notes 10 — Function Templates

**Concept recap:** `template <typename T>` lets a function be written once for a placeholder
type, instantiated concretely per call site; the compiler deduces `T` from argument types, which
can be ambiguous when arguments have different types; a template's body implicitly requires
whatever operations it uses to be supported by `T`.

**Common pitfalls:**
- Expecting `maxOf(3, 4.5)` to compile by "picking the wider type" automatically — deduction does
  not do implicit promotion across differing argument types; it requires both arguments to
  deduce the *same* `T`, or an explicit instantiation.
- Calling a template with a type that doesn't support an operation the template body uses (e.g.
  `maxOf` on a type with no `operator>`), and being confused by a compiler error that points
  inside the template definition rather than at the call site.
- Forgetting that a string literal's deduced type is `const char*`, not `std::string` — leading
  to surprising behavior (comparing pointers, not string contents) if not passed as an actual
  `std::string`.
- Writing `swapValues` with pass-by-value parameters instead of references — this compiles, but
  silently fails to actually swap the caller's variables.

**Debugging tip:** when a template fails to compile only for a *specific* type, isolate exactly
which line inside the template body uses an unsupported operation — the error, though it looks
intimidating, usually names the missing operator directly.

**Instructor tip:** deliberately trigger the "no matching function" error from calling `maxOf`
with mismatched argument types, live, so students learn to recognize and read it before meeting
it on their own in the mini-challenge or an assignment.
