# Week 10 Summary — Optimizers

**Key takeaways:**
- Plain SGD struggles on loss surfaces that are steep in one direction and shallow in another;
  momentum, RMSProp, and Adam all use information beyond the current gradient to compensate.
- Momentum accumulates a velocity $v \leftarrow \alpha v - \eta\nabla L$, damping oscillation and
  speeding progress in consistent directions.
- RMSProp divides each parameter's learning rate by the square root of a running average of its
  squared gradients, giving adaptive, per-parameter step sizes.
- Adam combines both ideas with bias-corrected moment estimates ($\hat m_t, \hat v_t$ correcting
  for $m_0=v_0=0$) and is the most widely used default optimizer in practice.

**You should now be able to:** derive and implement momentum, RMSProp, and Adam updates from
scratch, including Adam's bias correction; compare optimizer convergence behavior empirically.

**Next week:** regularization — controlling overfitting through L1/L2 penalties, dropout, and
early stopping.
