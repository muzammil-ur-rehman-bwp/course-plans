# Week 10 Summary — Neural Architecture Search

**Key takeaways:**
- NAS is fully specified by a search space, a search strategy, and a performance-estimation
  strategy.
- RL-based search applies the graduate course's REINFORCE estimator with a controller as policy
  and validation performance as reward; evolutionary search uses mutation and selection with no
  gradients; differentiable NAS relaxes the discrete choice into a softmax mixture, optimized by
  ordinary gradient descent and discretized at the end.
- NAS's own search cost (historically thousands of GPU-days for RL-based search) is itself a
  central practical constraint — weight sharing and differentiable relaxation exist specifically
  to address it, not as independent improvements.
- Cheaper performance-estimation strategies trade search cost against ranking fidelity.

**You should now be able to:** decompose a NAS method into its three components; explain RL-
based, evolutionary, and differentiable search strategies; evaluate why search cost has driven
NAS research.

**Next week:** Scaling laws from a systems/engineering perspective — compute-optimal training
allocation in practice, and real engineering challenges of training at scale.
