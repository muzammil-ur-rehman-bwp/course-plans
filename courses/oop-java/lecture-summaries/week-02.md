# Week 2 Summary — Constructors in Depth; `equals`/`hashCode`/`toString`; No Destructors

**Key takeaways:**
- Overloaded constructors can chain with `this(...)`, which must be the first statement in the
  constructor body; the compiler supplies a default no-argument constructor only if no
  constructor is written at all.
- Overriding `equals(Object)` (not `equals(MyClass)`, a common overload bug) and `hashCode()`
  together, consistently, is required for correct behavior in hash-based collections.
- `@Override` is the safeguard that catches a mismatched `equals` signature (or any other
  intended-override typo) at compile time instead of silently compiling a new overload.
- Java has no destructors — the garbage collector reclaims any unreachable object automatically,
  removing the need for a C++-style Rule of Three entirely.

**You should now be able to:** write chained overloaded constructors, write a correct
`equals`/`hashCode`/`toString` trio, and explain why Java needs no destructor.

**Next week:** composition — objects as fields of other classes, and "has-a" vs. the "is-a"
relationship inheritance introduces later.
