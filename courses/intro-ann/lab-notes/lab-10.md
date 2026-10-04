# Lab Notes 10 — Optimizers From Scratch

**Concept recap:** momentum uses a velocity term; RMSProp divides by a running RMS of squared
gradients; Adam does both, with bias correction that matters most in the first several updates.

**Common pitfalls:**
- Reusing the *same* learning rate across all four optimizers in Task C — plain SGD and Adam
  typically need very different learning rate scales (e.g., 0.1–0.5 for SGD on a small problem vs.
  0.001–0.01 for Adam); an "unfair" comparison using one shared rate often makes Adam look worse
  than it is, or SGD look like it has diverged when it has merely overshot.
- Forgetting to initialize $m$, $v$, and the momentum velocity $v$ to zero arrays matching each
  parameter's shape, or forgetting to persist them *between* calls to the update function rather
  than re-initializing every step (which silently turns momentum/Adam back into plain SGD).
- Off-by-one in the bias-correction exponent — $t$ must be the 1-indexed update count (starting
  at $t=1$, not $t=0$), since $\beta_1^0 = 1$ would make the correction divisor $1-1=0$ and divide
  by zero on the very first step.
- Not resetting the optimizer's state (velocity/moment estimates) when starting a fresh training
  run with freshly re-initialized weights — stale state from a previous run silently changes the
  first several updates' behavior.

**Debugging tip:** if Task D's "no bias correction" Adam variant behaves identically to full
Adam, check that $t$ is actually being incremented and used — a stuck $t=1$ (or a $t$ that resets
every call) will mask the bias-correction effect you are supposed to observe.

**Instructor tip:** use Task C's fair-tuning requirement to reinforce that optimizer comparisons
in published papers always involve per-optimizer learning rate tuning — a shared, untuned
learning rate is not a meaningful comparison.
