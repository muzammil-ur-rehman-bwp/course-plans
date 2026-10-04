# Week 5 Summary — Composition

**Key takeaways:**
- Composition ("has-a") models one object as a genuine part of another, by holding it as a
  regular data member — distinct from "is-a" (inheritance, next week).
- Member objects are constructed in declaration order, before the enclosing constructor's body
  runs, and destroyed in the exact reverse order.
- Initialize a member object through the member initializer list, passing whatever arguments its
  own constructor needs.
- Owning a member object (composition) is different from holding a pointer/reference to an object
  owned elsewhere (aggregation) — only the former ties the member's lifetime to the owner's.

**You should now be able to:** design a class composed of one or more member objects, write its
constructor with a correct initializer list, and explain member construction/destruction order.

**Next week:** inheritance I — base/derived classes, `protected` members, and constructor
chaining, as the "is-a" counterpart to this week's "has-a".
