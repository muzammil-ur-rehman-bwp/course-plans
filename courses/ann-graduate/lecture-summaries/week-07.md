# Week 7 Summary — Optimization Landscape Theory II

**Key takeaways:**
- Adam's effective per-parameter step size is $\alpha/(\sqrt{\hat v_t}+\epsilon)$; it is not
  guaranteed to converge even on simple convex problems, since this effective rate can grow back
  up instead of shrinking monotonically — AMSGrad's running-maximum fix restores a guarantee.
- Natural gradient descent preconditions by the Fisher information matrix, giving a
  reparameterization-invariant descent direction, at the same $O(p^2)$/$O(p^3)$ cost problem as
  Newton's method.
- Learning-rate warmup stabilizes early training by not committing to large steps while moment
  estimates and normalization statistics are still poorly estimated.

**You should now be able to:** re-derive Adam's update rule, explain its known failure mode, and
explain why warmup helps especially with adaptive optimizers and normalization.

**Next week:** regularization theory — bias-variance vs. double descent, and weight
decay/dropout's theoretical justifications; midterm review.
