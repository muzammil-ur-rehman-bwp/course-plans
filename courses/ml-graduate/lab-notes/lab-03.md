# Lab Notes 3 — Brute-Force Shattering and VC Dimension

**Concept recap:** VC dimension is *not* the number of parameters in a model. It is a combinatorial
property of the hypothesis class defined by shattering — a class can have very few parameters and
infinite VC dimension (e.g., $\mathrm{sign}(\sin(\theta x))$ has infinite VC dimension despite one
parameter $\theta$), or many parameters and modest VC dimension.

**Common pitfalls:**
- **Confusing VC dimension with parameter count** — the single most common conceptual error. For
  linear classifiers, $\mathrm{VCdim}=d+1$ happens to equal (parameter count), but this is a
  coincidence of this particular class, not a general rule.
- Concluding "not shattered" from a single failed brute-force random search over separating
  hyperplanes, when the search simply didn't try enough candidates — the lecture's
  `can_separate_with_halfplane` is a Monte Carlo proxy, not an exact LP feasibility solver; a
  negative result from it is suggestive, not a proof (use it to build intuition, then prove the
  impossibility direction by hand, as in the lecture's Radon's-theorem argument).
- Picking a degenerate point configuration (e.g., 4 collinear points) and drawing general
  conclusions from it — "general position" matters for the *lower*-bound (shattering)
  direction of a VC proof.

**Debugging tip:** for Task C, if the square's failing labeling found isn't the expected
alternating-corner pattern, increase `n_candidates` in the Monte Carlo search before concluding
your shattering-check code is wrong — the search can simply miss a valid separator by chance.

**Instructor tip:** explicitly contrast this lab's halfplane class ($\mathrm{VCdim}=3$ in
$\mathbb{R}^2$, 3 parameters: $w_1,w_2,b$) against a brief mention of a class with few parameters
but infinite VC dimension, to pre-empt the parameter-count misconception before it hardens.
