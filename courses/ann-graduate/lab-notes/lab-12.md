# Lab Notes 12 — Computing a Small-Scale Neural Tangent Kernel

**Concept recap:** the NTK is computed from the **gradient of the network's output with respect
to its parameters**, not with respect to its input — a common point of confusion given how much
of this course (Weeks 4–8) focused on input/activation gradients rather than parameter-gradient
inner products.

**Common pitfalls:**
- Computing $\nabla_x f$ (gradient with respect to input) instead of $\nabla_\theta f$ (gradient
  with respect to parameters) when building the kernel — these are entirely different objects;
  the NTK is specifically a kernel *over inputs*, built from *parameter* gradients.
- Forgetting to flatten and concatenate gradients from **every** parameter tensor (both `w1` and
  `v` in the lecture's `OneHiddenLayer`) before taking the inner product — omitting one
  parameter group silently computes a different (and wrong) kernel.
- In Task A, using too few random seeds or just one network instantiation per width and
  over-interpreting noise in the $K$-difference-across-widths number as a real trend — consider
  averaging over a few seeds if time permits, or at minimum stating that one draw at a given
  width is a single sample from substantial initialization randomness, especially at smaller
  widths.
- In Task C, re-using a kernel computed with the *trained* parameters $\theta_T$ instead of the
  *initial* parameters $\theta_0$ for the kernel-regression comparison — the NTK framework's
  claim is specifically about the kernel computed at (or near) initialization staying
  approximately fixed; using the trained kernel for this comparison breaks the intended test.

**Debugging tip:** if kernel-regression and trained-network predictions disagree wildly even at
width 2000, check first whether the kernel ridge regularizer (`1e-3` in the lecture code) is
appropriate for the specific kernel's scale — an ill-conditioned $K$ may need more regularization.

**Instructor tip:** make Task C's width-dependence explicit and discussed out loud — the point is
not "the trained network and kernel regression always agree," but that agreement **improves with
width**, which is the testable, falsifiable content of the NTK claim at finite width.
