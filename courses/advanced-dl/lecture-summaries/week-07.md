# Week 7 Summary — PPO for RLHF and Direct Preference Optimization

**Key takeaways:**
- PPO optimizes the KL-regularized RLHF objective by applying the graduate course's actor-critic
  machinery to a KL-regularized reward, using a clipped surrogate objective to bound per-step
  policy movement; it requires four interacting networks and is engineering-heavy.
- The KL-regularized objective's optimal policy has closed form
  $\pi^* \propto \pi_{\mathrm{ref}}\exp(r/\beta)$; inverting it gives the reward implied by any
  policy.
- Substituting the implied reward into the Bradley-Terry loss cancels the per-prompt partition
  function exactly, yielding the DPO loss — a single supervised-style loss on policy
  log-probabilities, with no reward model and no RL optimizer.
- DPO is an exact reparameterization of the same objective under the Bradley-Terry assumption,
  not an approximation; it trades flexibility (non-pairwise rewards, on-policy adaptation) for
  training-pipeline simplicity.

**You should now be able to:** state PPO's clipped objective in the RLHF setting; derive the
closed-form optimal policy; derive the DPO loss end to end, including the $Z(x)$ cancellation;
implement and train with the DPO loss.

**Next week:** Efficient inference I — quantization (post-training and quantization-aware) and
knowledge distillation; midterm review.
