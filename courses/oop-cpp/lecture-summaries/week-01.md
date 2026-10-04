# Week 1 Summary — Classes Recap & Encapsulation

**Key takeaways:**
- `public` members form a class's interface; `private` members are implementation detail hidden
  from outside code; `protected` is a third level — hidden from outsiders but visible to derived
  classes (useful starting Week 6).
- Getters read private data; setters write it and are the natural place to validate a new value
  before storing it.
- A `const` member function promises not to modify the object, which the compiler then enforces —
  including requiring `const` methods to call other `const` methods through a `const` reference.
- This course builds directly on the prerequisite's "intro to classes" week — everything from
  constructors in depth onward is new material this semester.

**You should now be able to:** audit a class's data members for the correct access level, write
validated getters/setters, and mark every non-mutating method `const`.

**Next week:** constructors in depth — default, parameterized, and copy constructors, member
initializer lists, destructors, and a first look at the Rule of Three.
