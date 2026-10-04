# Week 5 Summary — Inheritance I: `extends`, Access Control, Constructor Chaining

**Key takeaways:**
- `class Derived extends Base` gives the subclass every non-private field and method the
  superclass declares; `protected` is visible to the superclass and its subclasses, but not to
  outside code.
- A subclass constructor must invoke a superclass constructor, explicitly via `super(args)` as
  its first statement, or implicitly via the superclass's no-argument constructor.
- Java allows a class to `extends` only one superclass — there is no multiple class inheritance,
  so the diamond ambiguity possible in languages that allow it cannot arise in Java this way.

**You should now be able to:** design a two-level class hierarchy, chain a subclass constructor
to a non-default superclass constructor, and explain why Java restricts `extends` to one class.

**Next week:** inheritance II — overriding vs. overloading, `@Override`, and `Object` as the
universal superclass.
