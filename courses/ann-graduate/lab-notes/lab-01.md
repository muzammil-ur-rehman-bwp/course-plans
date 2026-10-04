# Lab Notes 1 — Prerequisite Refresher

**Concept recap:** this lab is a diagnostic, not new material — it confirms the forward/backward
pass and gradient-checking skills from *Introduction to Artificial Neural Networks* are solid
before this course builds autodiff theory, initialization theory, and optimization theory on top
of them.

**Common pitfalls:**
- Mismatched shapes between $\delta^{(l)}$ and $a^{(l-1)}$ in $\partial L/\partial W^{(l)} =
  \delta^{(l)}a^{(l-1)\top}$ — a transpose error that can silently run without a shape error if
  dimensions coincide.
- Forgetting the activation derivative factor ($\odot g'(z^{(l)})$) when propagating $\delta$
  backward through a hidden layer.
- Re-using a stale forward pass when perturbing a parameter for the finite-difference check —
  every perturbed evaluation needs a fresh forward pass.

**Debugging tip:** if Task C fails for exactly one layer, suspect that layer's $\delta$
backward-recursion step first, not the layer closest to the loss (which is usually simplest and
correct).

**Instructor tip:** treat Task C's pass/fail as a hard gate — do not let students proceed into
Week 2 material with an unverified backward pass; it will compound into confusing, hard-to-trace
bugs in Lab 3's autodiff engine.
