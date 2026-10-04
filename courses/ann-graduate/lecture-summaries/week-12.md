# Week 12 Summary — The Neural Tangent Kernel

**Key takeaways:**
- The NTK $\Theta(x,x';\theta) = \nabla_\theta f(x;\theta)^\top \nabla_\theta f(x';\theta)$
  measures how correlated the network's response to a parameter nudge is across two inputs.
- Jacot, Gabriel, and Hongler's result: in the infinite-width limit, the NTK converges to a fixed
  kernel at initialization and stays approximately constant during gradient-descent training, so
  training behaves like kernel regression against that fixed kernel.
- This explains easy trainability of wide networks via convex-like dynamics in function space, but
  explicitly involves no feature learning — a real limitation as an account of generalization.

**You should now be able to:** state the NTK's central claim, compute a small-scale empirical NTK,
and explain precisely what the theory does and does not establish.

**Next week:** the Lottery Ticket Hypothesis and pruning.
