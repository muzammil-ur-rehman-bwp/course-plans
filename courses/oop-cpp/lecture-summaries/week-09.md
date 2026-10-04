# Week 9 Summary — Polymorphism II: Abstract Classes

**Key takeaways:**
- A pure virtual function (`virtual T f() = 0;`) declares required behavior with no default
  implementation.
- A class with at least one pure virtual function is abstract and cannot be instantiated directly;
  a derived class that overrides all inherited pure virtual functions becomes concrete.
- C++ has no `interface` keyword — the convention is an abstract class with all-pure-virtual
  member functions and no data, used exactly like an interface elsewhere.
- An abstract class can still have a virtual destructor and ordinary (non-pure) virtual functions
  with sensible defaults alongside its pure virtual ones.

**You should now be able to:** design an abstract base class with pure virtual functions, derive
concrete classes from it, and drive them polymorphically through base-class pointers/references.

**Next week:** templates I — function templates, the first step into generic programming.
