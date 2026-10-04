# Week 6 Summary — Probabilistic Logic Programming

**Key takeaways:**
- The distribution semantics attaches independent probabilities to logic-program facts; a total
  choice induces an ordinary program whose least model determines query truth.
- `P(q) = Σ_{θ entailing q} P(θ)`, a discrete mixture over independently-weighted facts — no
  partition function needed, unlike MLNs' log-linear weighted-formula view.
- Production systems compile to BDDs for tractable inference at scale (conceptual).

**You should now be able to:** compute a query's probability by total-choice enumeration by hand
and in code; contrast the distribution semantics with MLN semantics precisely.

**Next week:** Neuro-symbolic integration I — differentiable/fuzzy relaxations of logical
operators. **Quiz 2** (Weeks 3–4) this week.
