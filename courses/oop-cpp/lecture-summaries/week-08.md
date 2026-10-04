# Week 8 Summary — Polymorphism I

**Key takeaways:**
- Without `virtual`, a call through a base-class pointer/reference resolves at compile time from
  the pointer's static type, not the object's actual type — rarely what you want.
- `virtual` enables dynamic dispatch: the call resolves at runtime via the object's vtable to its
  actual (dynamic) type's override.
- A base class with any virtual function, or ever deleted polymorphically, **must** have a
  `virtual` destructor — otherwise deleting through a base pointer skips the derived destructor
  (leaks, undefined behavior).
- Object slicing: passing/assigning a derived object **by value** as its base type discards the
  derived part and defeats polymorphism, even though it compiles. Polymorphism requires pointers
  or references, never by-value objects.

**You should now be able to:** add `virtual` and a virtual destructor correctly to a polymorphic
base class, and identify/avoid object slicing in by-value code.

**Next week:** Midterm Exam (Weeks 1–8), then polymorphism II — pure virtual functions and
abstract base classes.
