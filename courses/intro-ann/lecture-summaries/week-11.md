# Week 11 Summary — Regularization

**Key takeaways:**
- A network with excess capacity can memorize training data, hurting generalization; regularization
  trades some training performance for better validation/test performance.
- L2 regularization adds $\lambda W$ to the gradient, producing proportional weight decay every
  step; L1 adds a constant $\lambda\,\text{sign}(W)$ pull, tending to produce exactly-zero weights.
- Dropout randomly deactivates units during training (with inverted scaling to keep expected
  magnitude consistent) and uses all units at test time.
- Early stopping halts training once validation loss stops improving, directly using the
  train/validation gap as an overfitting signal.
- Capstone proposals are due this week.

**You should now be able to:** implement L1/L2 regularization, dropout, and early stopping;
choose and justify a regularization strategy from a training/validation loss curve.

**Next week:** moving to a deep learning framework — automatic differentiation generalizes the
backpropagation already implemented by hand, and lets training scale to real datasets.
