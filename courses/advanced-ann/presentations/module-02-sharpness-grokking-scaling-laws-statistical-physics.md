# Presentation: Module 2 — Sharpness, Grokking, Scaling Laws & Statistical Physics (Weeks 6–9)

**Format:** Slide deck outline for lecture delivery (convert to slides in institution's template).

1. **Title slide** — Advanced Artificial Neural Network (Post Graduate): Module 2, Sharpness,
   Grokking, Scaling Laws & Statistical Physics
2. **Module recap** — Module 1 asked which minimum training finds and why; Module 2 asks how to
   characterize and exploit that minimum's properties, and surveys other training-dynamics puzzles
3. **Flat vs. sharp minima** — the generalization intuition; Hessian-top-eigenvalue and
   perturbation-based sharpness measures
4. **The reparameterization critique** — ReLU's positive homogeneity and why raw sharpness is
   measurement-dependent
5. **Sharpness-Aware Minimization, derived** — the min-max objective; the first-order worst-case
   perturbation; the two-step practical update
6. **The grokking phenomenon** — Power et al.'s three-phase curve; why it is theoretically puzzling
7. **Competing grokking hypotheses** — slow implicit-regularization-driven transition vs. circuit
   formation; honest synthesis of what is and is not settled
8. **Scaling laws, empirically** — the power-law form in model size, data size, and compute
   (Kaplan et al.); fitting a scaling exponent
9. **Compute-optimal scaling** — the refinement broadly attributed to Hoffmann et al.
10. **Theoretical attempts at scaling laws** — data-manifold/intrinsic-dimension arguments;
    random-feature/kernel-theoretic connections back to Module 1's NTK material
11. **Midterm recap** — Weeks 1–8 synthesis checklist
12. **Statistical physics and the loss landscape** — the replica-method idea, conceptually
13. **Spin-glass analogies** — the critical-point-index/energy correlation, at its source
14. **The honest limits of the physics analogy** — non-rigorous replica continuation; idealized-
    disorder mismatch; simplified-architecture mismatch
15. **Module 2 recap** — key results checklist (sharpness/SAM, grokking hypotheses, scaling-law
    form, replica/spin-glass limits)
16. **Looking ahead** — "Next: double descent revisited rigorously, PAC-Bayes bounds, and
    loss-landscape geometry" teaser slide

**Speaker notes:** the reparameterization critique (slide 4) is this module's most commonly
under-appreciated point — students tend to treat SAM's empirical success as settling the
flat-minima debate; make the distinction between "SAM works" and "flatness causally explains
generalization" explicit before moving to grokking.
