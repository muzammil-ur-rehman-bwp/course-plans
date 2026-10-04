# Lab Notes 11 — Computing a PAC-Bayes Bound

**Concept recap:** the PAC-Bayes bound trades off a posterior-averaged empirical-risk term against
a KL-divergence-based complexity term; minimizing over posterior variance balances the two.

**Common pitfalls:**
- Using too few Monte Carlo samples (`n_samples`) in `empirical_risk_under_posterior`, producing a
  noisy risk estimate that makes the Task B sweep's minimum hard to locate reliably; increase
  `n_samples` if the curve looks jagged rather than smooth.
- Forgetting to restore the model's original parameters after each posterior sample inside
  `empirical_risk_under_posterior` — the lecture-content code does this explicitly (`p.copy_(
  orig)` after the sampling loop); skipping it silently corrupts the trained weights for all
  subsequent computations in the notebook.
- In Task C, comparing the two models' bounds at the *same* `sigma_q2` rather than each model's
  own *minimized* bound — the point of the sharpness connection is that a flatter (SAM-trained)
  model should tolerate a *larger* minimizing `sigma_q2` while still achieving a tighter overall
  bound, not that it achieves a lower bound at a fixed, arbitrary `sigma_q2`.

**Debugging tip:** if the PAC-Bayes bound comes out larger than 1 for every `sigma_q2` tried,
first check the prior variance `sigma_p2` is not set unreasonably small relative to the actual
distance `‖w*-w0‖` — an overly tight prior will inflate the KL term regardless of posterior choice.

**Instructor tip:** have students state, before running Task C, which of the two models (SGD or
SAM-trained) they expect to achieve a tighter minimized bound, and why, using Week 6 and Week 11's
material together — this is the lab's key cross-week synthesis point.
