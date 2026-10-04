# Week 6 Summary — Inheritance I

**Key takeaways:**
- Inheritance models "is-a" (`Dog` is a kind of `Animal`); composition models "has-a" (Week 5) —
  choosing the wrong one is a common design mistake, revisited in Week 14.
- `class Derived : public Base` gives `Derived` every `public`/`protected` member of `Base`;
  `protected` members are visible to derived classes' own code but not to outside code.
- A derived constructor must arrange for a base constructor to run, explicitly in its initializer
  list (`Base(args)`) when `Base` has no usable default constructor.
- Construction runs base-first, then derived; destruction runs derived-first, then base — the
  exact reverse.

**You should now be able to:** design a two-level class hierarchy, write a derived constructor
that correctly chains to a non-default base constructor, and explain `protected`'s role.

**Next week:** inheritance II — overriding member functions with `override`, and a brief,
cautionary look at multiple inheritance and the diamond problem.
