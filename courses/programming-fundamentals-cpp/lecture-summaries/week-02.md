# Week 2 Summary — Operators, Expressions, and Type Conversion

**Key takeaways:**
- Arithmetic (`+ - * / %`), relational (`== != < > <= >=`), and logical (`&& || !`) operators are
  the building blocks of every expression.
- Integer division truncates (`7 / 2 == 3`); mix in a `double` or use `static_cast<double>` to get
  a fractional result.
- Use parentheses to make operator precedence explicit rather than relying on memorized rules.
- Prefer `static_cast<T>` over C-style casts for explicit type conversion.

**You should now be able to:** write expressions combining arithmetic, relational, and logical
operators; explain why `7 / 2 != 3.5`; convert between `int` and `double` explicitly and safely.

**Next week:** control flow with `if`/`else` and `switch`, which use the boolean expressions from
this week to make a program branch.
