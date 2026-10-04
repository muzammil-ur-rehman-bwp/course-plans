# Week 6 Summary — Non-Monotonic Reasoning via Answer Set Programming

**Key takeaways:**
- A normal logic program's rules use negation as failure (`not c`, "c cannot be derived"), not
  classical negation — the source of ASP's non-monotonicity.
- M is a stable model of P iff M equals the least model of the Gelfond–Lifschitz reduct P^M
  (delete rules whose `not c` is contradicted by M, then strip surviving `not` literals);
  programs can have zero, one, or several stable models.
- ASP solves combinatorial problems (e.g., graph coloring) by encoding each solution as a stable
  model; it gives non-monotonic reasoning a directly computable semantics, unlike default logic's
  extension-based account.

**You should now be able to:** compute a GL-reduct and check stability by hand and in code;
encode a small combinatorial problem as an ASP program; contrast ASP with default
logic/circumscription.

**Next week:** Belief revision and update — the AGM postulates for rational belief revision, the
revision/update distinction, and Dalal's Hamming-distance-based revision operator.
