# Week 11 Summary — Deep Reinforcement Learning II: Policy Gradient and Actor-Critic Methods

**Key takeaways:**
- Policy gradient methods directly parameterize and optimize $\pi_\theta(a\mid s)$, with no value
  function required.
- REINFORCE's gradient estimator, $\mathbb{E}[\sum_t \nabla_\theta\log\pi_\theta(a_t\mid
  s_t)\,G_t]$, follows from differentiating $J(\theta)$ and applying the log-derivative trick.
- Subtracting any action-independent baseline leaves the estimator unbiased while typically
  reducing its variance; a learned critic $V_\phi(s)$ gives the lowest-variance baseline,
  yielding the actor-critic advantage $A(s,a)=G_t-V(s)$.

**You should now be able to:** derive the REINFORCE estimator step by step and implement it with
a baseline, and explain the actor-critic variance-reduction argument.

**Next week:** large-scale training practices — parallelism, mixed precision, gradient
checkpointing, and LoRA.
