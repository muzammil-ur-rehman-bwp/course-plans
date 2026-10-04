# Lab Notes 4 — Operator Overloading II (Streams & `friend`)

**Concept recap:** `operator<<`/`operator>>` must be free functions because the stream is the
left-hand operand; `friend` grants one named free function access to private members; both
operators must return the stream by reference to support chaining.

**Common pitfalls:**
- Forgetting `return out;`/`return in;` at the end of the operator — breaks chaining
  (`std::cout << a << b`) with a confusing compile error about the next `<<` in the chain.
- Returning the stream *by value* instead of by reference — streams are not copyable, so this
  fails to compile, but the error message can be unclear to someone seeing it for the first time.
- In `operator>>`, writing into the target object *before* validating the input, leaving it
  partially modified even when the read ultimately fails.
- Declaring `friend` inside the class but then defining the function with a mismatched
  signature (e.g. missing `const` on the `Fraction&` parameter) — the `friend` declaration and
  the definition must match exactly.

**Debugging tip:** if chained `<<`/`>>` calls fail to compile with an error pointing at the
*next* operator in the chain, check that your own operator returns the stream by reference, not
by value and not void.

**Instructor tip:** feed `operator>>` deliberately malformed input (e.g. `"3x4"` instead of
`"3/4"`) live, and show that `setstate(std::ios::failbit)` makes `if (std::cin >> f)` correctly
detect the failure — mirroring how `std::cin >> someInt` behaves on non-numeric input.
