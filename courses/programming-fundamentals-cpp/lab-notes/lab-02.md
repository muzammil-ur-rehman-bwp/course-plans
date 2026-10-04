# Lab Notes 2 — Operators, Expressions, and Type Conversion

**Concept recap:** `/` truncates for two integer operands; `%` gives the remainder (integers
only); `static_cast<double>(x)` forces floating-point arithmetic; `&&`/`||`/`!` combine boolean
expressions; parentheses make precedence explicit and should be used generously.

**Common pitfalls:**
- Writing `a / b` expecting a fractional result when both are `int` — silently truncates instead
  of erroring.
- Using `=` instead of `==` inside a condition (a typo the compiler often, but not always, warns
  about with `-Wall`).
- Casting only one operand but expecting both to be treated as floating-point — remember only the
  cast operand changes type; the *other* operand being `int` is what then promotes the whole
  expression.
- Dividing by zero with `%` or `/` on integers — undefined behavior (not a clean "infinity" like
  floating-point division by zero can produce).

**Debugging tip:** when an expression's result looks wrong, print the type-relevant intermediate
values (e.g., print `a`, `b`, and `a / b` separately) to see exactly where truncation or an
unexpected conversion happens.

**Instructor tip:** have students predict `7 / 2`, `-7 / 2`, and `7 % -2` on paper before running
them — negative-operand truncation/remainder behavior surprises many beginners.
