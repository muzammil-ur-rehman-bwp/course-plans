# Week 3 Summary — Automatic Differentiation in Depth

**Key takeaways:**
- Any computation can be represented as a computational graph of elementary operations; automatic
  differentiation applies the chain rule systematically over that graph, exactly (not
  approximately, unlike finite differences).
- Forward-mode AD propagates tangents and needs one sweep per input; reverse-mode AD propagates
  adjoints from a scalar output and needs just one sweep for all inputs — the regime neural
  network training lives in (many parameters, one scalar loss).
- Backpropagation is not a separate algorithm: it is reverse-mode AD specialized to a layered
  network's computational graph.

**You should now be able to:** build a minimal reverse-mode autodiff engine from scratch and
verify it reproduces hand-derived gradients from the prerequisite course.

**Next week:** initialization theory — why naive initialization fails and how Xavier/He
initialization are derived from variance-preservation arguments.
