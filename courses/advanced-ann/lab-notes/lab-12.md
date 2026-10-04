# Lab Notes 12 — Mode Connectivity: Linear vs. Nonlinear Interpolation

**Concept recap:** a high linear-interpolation loss barrier between two minima does not imply they
are disconnected in the landscape — a simple nonlinear (bend-point) path can connect them while
keeping loss low throughout.

**Common pitfalls:**
- Insufficient training of `model_A`/`model_B` before measuring the barrier — an undertrained
  model's "minimum" is not really a minimum yet, and the resulting linear-interpolation curve can
  look misleadingly flat or misleadingly sharp; confirm both models reach low, comparable training
  loss before proceeding to Tasks B–D.
- Using too few Bezier optimization steps for the bend point in Task C, leaving it close to its
  initial value (the straight-line midpoint) and producing a Bezier curve barely better than the
  linear one — the optimization needs to run long enough for the bend point to move meaningfully
  away from the midpoint.
- In Task D, using a *different* initialization for both networks by mistake (e.g., reseeding
  `make_model()` independently for each) — the whole point of the comparison is that both networks
  must start from the *exact same* initial parameters, differing only in training randomness
  (data order), to correctly test the winning-ticket-style linear-connectivity claim.

**Debugging tip:** verify `set_params` and `flatten` are true inverses of each other on a tiny test
case (flatten a model, perturb the flat vector by a known amount, set it back, and confirm the
model's parameters changed exactly as expected) before trusting `loss_at`.

**Instructor tip:** have students predict, before running Task D, whether the shared-initialization
pair will show a smaller or larger linear-interpolation barrier than the independently-initialized
pair from Task B — this is the lab's key conceptual payoff, directly testing Week 12 §3's claim.
