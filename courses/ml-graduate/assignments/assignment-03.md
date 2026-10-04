# Assignment 3 — Bayesian ML, Gaussian Processes, Dimensionality-Reduction Theory (Weeks 9–12)

**Weight:** 5% of course grade (one of 3 problem sets, 15% total) | **Assigned:** Week 12 | **Due:** Start of Week 13

## Instructions
Submit `assignment03.ipynb` (and/or PDF for proof-only questions) with full derivations and
working code for all code questions.

## Questions
1. **(Bayesian linear regression, 20 pts)** Re-derive the Bayesian linear regression posterior
   mean and covariance by completing the square in the log-posterior, showing every algebraic
   step (do not simply cite the final formula). Then, for a provided small dataset and
   $\sigma^2=0.5,\tau^2=2$, compute the posterior mean/covariance by hand and verify in code.
2. **(Ridge-as-MAP, 10 pts)** Prove that the posterior mode of a Gaussian posterior equals its
   mean, and use this fact to show the MAP estimate from Question 1 exactly equals the
   corresponding ridge-regression solution; state the implied $\lambda$.
3. **(Gaussian Processes, 25 pts)** Derive the GP regression predictive mean and covariance from
   the joint Gaussian of training and test outputs, using the Gaussian conditioning identity
   (show the identity's derivation, not just its statement). Then implement GP regression from
   scratch (Cholesky-based) on a provided dataset and report the predictive mean/std at three
   specified test points.
4. **(GP hyperparameters, 10 pts)** For the same dataset, fit GP regression at three different RBF
   length scales; report and discuss the resulting (negative log) marginal likelihood for each,
   and state which length scale the marginal likelihood favors.
5. **(PCA optimality, 20 pts)** Prove, via the Lagrangian argument, that the variance-maximizing
   unit direction is the top eigenvector of the covariance matrix, and extend the argument (using
   the Courant–Fischer characterization) to show the top-$k$ eigenvectors jointly maximize
   retained variance among rank-$k$ projections. Then implement PCA from scratch and verify
   against `sklearn.decomposition.PCA` on a provided dataset.
6. **(Kernel PCA, 15 pts)** Derive the centered-kernel-matrix formula used in kernel PCA, and
   implement kernel PCA from scratch for an RBF kernel on a provided nonlinear dataset,
   confirming it separates classes that linear PCA cannot.

## Submission
Upload `assignment03.ipynb` (and/or PDF) via the course submission system. Late policy per
syllabus (`course-plan.md` §7).
