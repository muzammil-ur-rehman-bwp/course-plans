# Week 1 Summary — Classes Recap & Encapsulation

**Key takeaways:**
- `private` fields plus public getters/setters form a class's controlled interface; a setter is
  the natural place to validate a new value before storing it.
- `this` disambiguates a field from a same-named parameter, and is required when assigning a
  constructor/method parameter into a field of the same name.
- A class with only `final` fields, set once in the constructor, and no setters is immutable —
  easier to reason about and safe to share.
- This course builds directly on the prerequisite's intro-to-classes week — everything from
  constructors in depth onward is new material this semester.

**You should now be able to:** audit a class's fields for the correct access level, write
validated getters/setters, and design a small immutable class.

**Next week:** constructors in depth — overloaded constructors, `this(...)` chaining, the
`equals`/`hashCode`/`toString` contract, and why Java has no destructors.
