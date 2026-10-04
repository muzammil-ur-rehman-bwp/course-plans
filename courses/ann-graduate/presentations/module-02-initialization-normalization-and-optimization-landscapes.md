# Presentation: Module 2 — Initialization, Normalization & Optimization Landscapes (Weeks 4–8)

**Format:** Slide deck outline for lecture delivery.

1. **Title slide** — Module 2: Initialization, Normalization & Optimization Landscapes
2. **Why initialization matters** — symmetry breaking; geometric variance decay/explosion
3. **Deriving Xavier/Glorot** — the forward/backward variance-preservation compromise
4. **Deriving He initialization** — correcting for ReLU's variance-halving
5. **Batch Normalization, forward** — per-batch statistics; train vs. eval mode
6. **Batch Normalization, backward** — chain rule through $\mu_B$ and $\sigma_B^2$
7. **Why does BatchNorm work?** — internal covariate shift vs. loss-landscape smoothing
8. **Layer Normalization** — per-example, batch-independent; when it's preferred
9. **The loss landscape's geometry** — saddle-point dominance in high dimensions
10. **Newton's method & Gauss-Newton** — quadratic convergence, saddle-attraction pathology, and
    the $O(p^2)$/$O(p^3)$ cost wall
11. **Adam re-derived, and its failure case** — the oscillating-gradient non-convergence
    construction; AMSGrad's fix
12. **Natural gradient descent** — Fisher-information preconditioning, conceptually
13. **Learning-rate warmup theory** — why ramping up early stabilizes adaptive optimizers
14. **Bias-variance vs. double descent** — the classical U vs. the empirical double-descent curve
15. **Weight decay as MAP; dropout as Bayesian averaging** — two regularizers, two Bayesian
    readings
16. **Module recap** — initialization and normalization get signal flowing; optimization-landscape
    theory explains how (and how imperfectly) gradient descent navigates the result; regularization
    theory complicates the classical capacity story — onward to generalization theory (Module 3)

**Speaker notes:** slide 7 (why does BatchNorm work) should be delivered as a genuine debate, not
a settled fact followed by a single "right answer" — present both explanations, then the evidence
that favors loss-landscape smoothing, so students see how to weigh competing theoretical claims
against evidence, a skill reused constantly in Module 4.
