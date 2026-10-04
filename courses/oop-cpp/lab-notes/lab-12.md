# Lab Notes 12 — Exception Handling

**Concept recap:** `throw` raises an exception that propagates to the nearest matching `catch`,
unwinding the stack along the way; `<stdexcept>` provides a standard hierarchy to throw the most
specific applicable type; a custom exception class typically derives from `std::exception`/
`std::runtime_error`; always catch by reference, most-specific clause first.

**Common pitfalls:**
- **Catching by value:** `catch (std::exception e)` instead of `catch (const std::exception& e)`
  — this copies (and, for a derived exception type, slices) the exception object, losing derived-
  class data and potentially the correct `what()` override; always catch by (`const`) reference.
- Ordering `catch` clauses general-to-specific instead of specific-to-general — a `catch (const
  std::exception&)` placed before `catch (const InsufficientFundsError&)` will catch everything
  first, making the more specific clause unreachable (and some compilers warn about this).
- Forgetting that `InsufficientFundsError` must correctly call `std::runtime_error`'s constructor
  (with a message) in its own initializer list, or `what()` will not report the message intended.
- Using `catch (...)` as the *only* handler, discarding all information about what actually went
  wrong — reserve it as a last-resort safety net after more specific clauses.

**Debugging tip:** if a `catch` clause for a custom exception type never seems to trigger even
though the exception is thrown, check both the `catch` clause's ordering relative to any more
general clauses above it, and whether it's catching by value (and thus silently slicing before
your derived-specific code would run as expected).

**Instructor tip:** demonstrate `catch`-by-value slicing directly: catch `InsufficientFundsError`
by value as `std::runtime_error`, and show that `getShortfall()` is no longer callable/compiles
only on the base type — a direct callback to Week 8's object-slicing lesson.
