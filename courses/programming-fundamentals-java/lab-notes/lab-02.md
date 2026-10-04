# Lab Notes 2 — Operators and Expressions

**Concept recap:** arithmetic/relational/logical operators work as expected, but integer division
truncates (`7 / 2 == 3`); an explicit cast (`(double) a / b`) must be applied *before* the
division to get a fractional result; `int` overflows silently rather than erroring.

**Common pitfalls:**
- Writing `(double) (a / b)` instead of `(double) a / b` — the cast applied after integer
  division has already happened does nothing useful; the truncation is permanent by then.
- Confusing `=` (assignment) with `==` (equality) inside a condition — Java's compiler rejects
  `if (x = 5)` for a `boolean` condition unless `x` is itself `boolean` (unlike some languages,
  this is usually a compile error in Java, which is a safety net worth pointing out).
- Expecting `Integer.MAX_VALUE + 1` to throw an error — it silently wraps to
  `Integer.MIN_VALUE` instead.
- Mixing up operator precedence (e.g., assuming `+` happens before `*`); when unsure, add
  parentheses rather than guessing.

**Debugging tip:** when a computed result looks "close but wrong," check for an integer-division
truncation first — it's the single most common source of subtly wrong numeric output in early
Java programs.

**Instructor tip:** have students predict `a / b` and `(double) a / b` on paper *before* running
the code — the mismatch between their prediction and the output is a memorable teaching moment.
