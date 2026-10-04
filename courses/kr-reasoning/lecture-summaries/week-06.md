# Week 6 Summary — Semantic Networks and Frames

**Key takeaways:**
- Semantic networks represent categories/objects as nodes and relations (IS-A, part-of) as
  edges; strict inheritance walks the IS-A chain for properties, but gives wrong answers under
  exceptions (the Tweety/penguin case).
- Frames generalize this with slots that distinguish a specific value from a default value;
  resolving a slot checks a frame's own value, then its own default, then recurses to its parent
  — which correctly lets a more specific frame override an inherited default.
- This exception-handling behavior is a first, structural taste of non-monotonic reasoning,
  formalized properly in Week 9.

**You should now be able to:** implement a semantic network with property inheritance; implement
a frame system with slots, defaults, and correct default-overriding inheritance; explain why
strict inheritance fails on the Tweety/penguin case and how frames fix it.

**Next week:** description logics and ontologies — DL syntax, the DL/FOL relationship, and
OWL/RDF.
