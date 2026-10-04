# Week 5 Summary — Loss Functions

**Key takeaways:**
- MSE suits regression with a linear output; binary cross-entropy suits sigmoid outputs;
  categorical cross-entropy suits softmax outputs — the loss must match the task and the output
  activation, not be chosen independently.
- Categorical cross-entropy reduces to $-\log(\hat y_{k^*})$, the negative log-probability
  assigned to the correct class, given a one-hot label.
- MSE paired with a saturating sigmoid produces a vanishing gradient exactly when the prediction
  is most confidently wrong; cross-entropy's gradient with respect to the pre-activation $z$
  simplifies to the clean, non-vanishing $\hat y - y$ — a derivable reason, not a convention, for
  preferring cross-entropy in classification.

**You should now be able to:** implement and compute MSE, binary cross-entropy, and categorical
cross-entropy; explain, with the derivative algebra, why loss/activation mismatches cause slow
learning.

**Next week:** gradient descent — using the loss's gradient with respect to the weights to
actually update them.
