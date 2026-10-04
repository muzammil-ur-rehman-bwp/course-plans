# Week 3 Summary — Mean-Field Theory of Neural Networks

**Key takeaways:**
- The mean-field limit tracks the *distribution* of hidden-unit parameters as width → ∞ (a
  Wasserstein-gradient-flow/PDE view), in contrast to the NTK limit's fixed kernel.
- The two limits arise from different output-layer scaling conventions ($1/\sqrt m$ vs. $1/m$);
  only the mean-field scaling permits O(1) relative parameter movement and therefore genuine
  feature learning.
- The mean-field variance-propagation recursion generalizes Xavier/He initialization, recovering
  both as fixed-point special cases, and a companion correlation recursion reveals an
  order-to-chaos transition in depth.

**You should now be able to:** state why two different, both-valid infinite-width limits of the
same wide network exist; derive the variance-propagation recursion's fixed points for a given
activation; connect mean-field signal propagation back to the graduate course's initialization
formulas as a special case rather than a separate fact.

**Next week:** Implicit regularization and the implicit bias of gradient descent — the max-margin
result for linear models on separable data, derived, and a survey of the much harder nonlinear
case.
