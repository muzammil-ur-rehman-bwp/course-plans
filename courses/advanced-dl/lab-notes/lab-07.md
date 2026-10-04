# Lab Notes 7 — Direct Preference Optimization

**Concept recap:** DPO's loss is the Bradley-Terry loss with the reward replaced by its
closed-form-optimal-policy inversion; the per-prompt partition function $Z(x)$ cancels exactly in
the preference-probability difference.

**Common pitfalls:**
- Forgetting that `dpo_loss` needs *sequence* (or, here, per-item) log-probabilities under both
  the policy and the frozen reference model — accidentally backpropagating gradients into the
  reference model's parameters (it must stay frozen; use `torch.no_grad()` or `.detach()` when
  computing its log-probabilities).
- Setting $\beta$ too small or too large without connecting the choice back to its role in the
  original KL-regularized objective — $\beta$ is not a free hyperparameter chosen purely by
  trial and error here; relate your chosen value back to Week 6's discussion of what $\beta$
  trades off.
- In Task D, skipping the algebraic step where $\log Z(x)$ appears identically in both the
  $y_w$ and $y_l$ reward expressions — this is the step graders will look for explicitly; writing
  "the $Z(x)$ terms cancel" without showing why is insufficient.
- Comparing Task C's correlation to Lab 6's using different random seeds/datasets, making the
  comparison meaningless — reuse the exact same Lab 6 preference pairs for a fair comparison.

**Debugging tip:** as a sanity check, verify that at initialization (before training), DPO's loss
equals $-\log\sigma(0) = \log 2$ for every pair, since $\pi_\theta = \pi_{\mathrm{ref}}$ at that
point and the log-ratio difference is exactly zero.

**Instructor tip:** ask students, before running Task C, to predict whether DPO's fitted ranking
correlation will be noticeably better, worse, or about the same as Lab 6's explicit reward
model's — under the Bradley-Terry assumption (true in this synthetic setup), they should predict
"about the same," since DPO is an exact reparameterization of the same underlying objective, not
an approximation.
