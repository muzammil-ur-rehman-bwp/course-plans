# Lab Notes 8 — Fitting Scaling-Law Curves

**Concept recap:** fitting a power law reduces to a linear regression in log-log space; the fitted
slope is (minus) the scaling exponent.

**Common pitfalls:**
- Forgetting to take logs of *both* axes before the linear fit — fitting a line directly to
  `(sizes, losses)` rather than `(log(sizes), log(losses))` produces a meaningless result for data
  that is actually power-law distributed.
- In Task C, choosing 3 size points that are too close together (e.g., the 3 largest) rather than
  spanning the available range — a sparse-but-spread-out subset gives a much more informative
  comparison to the full-data fit than a sparse-but-clustered one.
- In Task D, varying both $N$ and $D$ simultaneously when trying to isolate $\alpha_N$ — hold $D$
  fixed at a large enough value that the $D$-term is not the bottleneck while sweeping $N$, and
  vice versa, or the two exponents will be confounded in the fit.

**Debugging tip:** if a fitted exponent comes out negative or absurdly large, print the raw
`(log_n, log_l)` pairs and visually check they look like a roughly straight line before trusting
the regression output.

**Instructor tip:** have students predict, before running Task B, whether a larger noise level
will bias the fitted exponent or simply make it noisier (less precise) — the correct answer
(simple least-squares log-log fitting is unbiased in expectation but increasingly imprecise, for
this symmetric multiplicative-noise model) is a useful, generalizable statistics point.
