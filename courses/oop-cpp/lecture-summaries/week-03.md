# Week 3 Summary — Operator Overloading I

**Key takeaways:**
- Overloading an operator as a member function lets a user-defined type use natural arithmetic/
  comparison syntax; the left-hand operand is the implicit object, the right-hand operand is the
  parameter.
- Arithmetic operators like `operator+` should return a *new* object by value and leave both
  operands unchanged; `operator+=` is the one that mutates `*this` and returns `Fraction&` to
  support chaining.
- `operator==` and `operator<` must be mutually consistent, or code that sorts/searches your type
  will behave incorrectly with no warning.
- Overload an operator only when its meaning matches what readers already expect from that
  operator (e.g. `+` should mean addition, not something unrelated).

**You should now be able to:** implement arithmetic and comparison operators as member functions
for a small value type, correctly distinguishing mutating (`+=`) from non-mutating (`+`) forms.

**Next week:** operator overloading II — why stream operators can't be member functions, and how
`friend` functions solve that.
