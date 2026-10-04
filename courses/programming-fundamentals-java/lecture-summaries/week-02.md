# Week 2 Summary — Operators, Expressions, and Type Conversion

**Key takeaways:**
- Arithmetic, relational, logical, and assignment operators behave as expected, but integer
  division truncates — `7 / 2` is `3`, not `3.5`.
- Implicit widening (`int` to `double`) is always safe; narrowing (`double` to `int`) always
  requires an explicit cast, and always truncates rather than rounds.
- `int` arithmetic silently overflows (wraps around) rather than throwing an error; `double`
  arithmetic has inherent rounding imprecision — never compare two `double`s with `==`.
- Java has exactly eight primitive types; everything else (`String`, arrays, classes) is a
  reference type — this distinction will matter a great deal from Week 5 onward.

**You should now be able to:** write expressions with correct operator precedence; use explicit
casts to avoid the integer-division trap; explain why `int` overflow and `double` imprecision
happen.

**Next week:** control flow — making decisions with `if`/`else` and `switch`.
