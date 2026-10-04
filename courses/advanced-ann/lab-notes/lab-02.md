# Lab Notes 2 — NTK: Lazy Training and Kernel Drift Across Width

**Concept recap:** lazy training is the condition $\|\theta_t-\theta_0\|/\|\theta_0\|\to 0$ as
width grows; it is the mechanism behind the NTK limit's kernel staying approximately fixed.

**Common pitfalls:**
- Forgetting `model.zero_grad()` between per-sample gradient computations inside `ntk_matrix`,
  which silently accumulates gradients across samples and produces a wrong (inflated) kernel.
- Using too few training steps at the largest widths in Task B — larger widths can need more
  steps to reach comparably low training loss with the same learning rate; compare final training
  loss across widths before trusting the kernel-drift comparison between them.
- In Task D, confusing "larger learning rate moves parameters more in absolute terms" with
  "larger learning rate breaks the lazy regime" — report *relative* movement
  ($\|\theta_t-\theta_0\|/\|\theta_0\|$), not raw movement, since that is the quantity Week 2's
  theory is actually about.

**Debugging tip:** before trusting a `width=5000` run, sanity-check `ntk_matrix` on `width=20`
against the Week 2 lecture content's own printed output pattern — both should show a smaller
kernel-drift value at the larger width.

**Instructor tip:** have students predict, before running Task D, whether a 10x larger learning
rate will increase or decrease relative parameter movement at fixed width, then check their
prediction against the result — reinforcing that the lazy/feature-learning boundary is a property
of training dynamics as a whole, not of architecture alone.
