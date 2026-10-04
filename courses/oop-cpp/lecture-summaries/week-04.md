# Week 4 Summary — Operator Overloading II

**Key takeaways:**
- `operator<<`/`operator>>` must be free functions (the stream, a type you don't own, is the
  left-hand operand), not member functions of the class being printed/read.
- `friend` grants one named free function access to a class's private members — a narrow,
  deliberate exception to encapsulation, best used only when public accessors aren't otherwise
  sufficient.
- A correct `operator<<`/`operator>>` returns the stream by reference, which is what makes
  chained `<<`/`>>` calls work.

**You should now be able to:** implement a correct, chainable `operator<<`/`operator>>` pair for
a user-defined type using `friend`, and decide when `friend` is warranted versus using existing
public accessors.

**Next week:** composition — building classes out of other classes as data members ("has-a"
relationships), as the first of two ways (alongside inheritance, Week 6) to relate classes.
