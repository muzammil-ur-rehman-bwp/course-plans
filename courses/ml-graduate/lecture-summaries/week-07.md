# Week 7 Summary — Kernel Methods and RKHS Theory

**Key takeaways:**
- A positive-definite kernel is always some inner product in some (possibly infinite-dimensional)
  feature space (Mercer's theorem); the RKHS is built from the kernel via its reproducing
  property, $\langle f,k(x,\cdot)\rangle=f(x)$.
- The representer theorem proves that minimizing a regularized empirical risk over an
  infinite-dimensional RKHS always has a finite, $m$-dimensional solution
  $\hat f=\sum_i\alpha_ik(x_i,\cdot)$ — the orthogonal-decomposition-plus-Pythagoras proof shows
  any orthogonal component only adds regularization cost with zero data-fit benefit.
- This is precisely why kernel SVMs and kernel ridge regression are computationally tractable,
  and rigorously justifies the "kernel trick" used informally in the prerequisite course.

**You should now be able to:** state and prove (sketch) the representer theorem and derive
kernel ridge regression's closed-form solution from it.

**Next week:** rigorous ensemble theory — AdaBoost's training-error bound, margin theory, and
bagging's variance-reduction argument; midterm review.
