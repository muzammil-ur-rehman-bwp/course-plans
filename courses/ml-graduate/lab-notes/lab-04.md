# Lab Notes 4 — Estimating Rademacher Complexity

**Concept recap:** Rademacher complexity measures how well a hypothesis class can correlate with
*pure noise* on the actual sample; for a norm-bounded linear class it has a clean, dimension-free
closed form, in contrast to VC dimension's explicit dependence on $d$ for the same class.

**Common pitfalls:**
- Forgetting that the Monte Carlo estimator's variance itself shrinks only slowly with the number
  of $\sigma$ draws — using too few draws (e.g., 10) gives a noisy estimate that can mislead Task
  B/C's trend-fitting; use at least a few hundred draws.
- Using a different norm bound $B$ in the estimator than in the closed-form comparison bound,
  making the two lines in the Task C plot incomparable.
- Over-generalizing the dimension-independence result (Task D) to *all* hypothesis classes — it
  is a property of this particular norm-bounded linear class's Rademacher complexity, not a
  universal fact; a class with intrinsically higher capacity can still have dimension-dependent
  Rademacher complexity.

**Debugging tip:** if Task B's log-log plot doesn't look linear (roughly slope $-1/2$), check that
`X` is being redrawn fresh for each $m$ (not reusing a fixed small sample padded with zeros) and
that the RBF/linear sup-formula from the lecture content is implemented exactly as derived (a sign
or normalization error here is the most common bug).

**Instructor tip:** have students compute, by hand, the VC-dimension-based bound's $d$-dependence
for the same class size, and put both numbers side by side — this is the clearest way to make
Week 4's "Rademacher complexity can beat VC dimension" claim concrete rather than abstract.
