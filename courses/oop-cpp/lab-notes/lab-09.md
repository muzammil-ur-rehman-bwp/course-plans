# Lab Notes 9 — Abstract Classes

**Concept recap:** a pure virtual function (`= 0`) has no body and makes its class abstract (not
instantiable); a derived class becomes concrete only once it overrides every inherited pure
virtual function; C++ has no `interface` keyword — an all-pure-virtual abstract class fills that
role by convention.

**Common pitfalls:**
- Forgetting to override *every* pure virtual function in a derived class — the derived class
  remains abstract too, and `new Circle(...)` fails to compile with a clear "cannot instantiate
  abstract class" error (worth reading carefully rather than guessing at a fix).
- Forgetting `virtual ~Shape() = default;` — the lesson from Week 8 still applies: this hierarchy
  is used polymorphically (through `Shape*` in Task C), so it still needs a virtual destructor.
- Leaking the `new`-allocated `Circle`/`Rectangle` in Task C by forgetting the matching
  `delete` — a reminder of exactly the problem Week 15's smart pointers solve.
- Trying to give `area()` a body *and* `= 0` at the same time — a pure virtual function has no
  body at its declaration (a separate, rarely-used syntax exists for providing one anyway, but is
  out of scope here).

**Debugging tip:** a "cannot declare variable to be of abstract type" error names the exact class
and, usually, which pure virtual function(s) remain unoverridden — read the full error message,
not just the first line.

**Instructor tip:** this is a lighter lab session given the midterm exam earlier in the week;
focus class time on Tasks A–C and treat Task D as optional/take-home if time is short.
