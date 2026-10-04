# Presentation: Module 3 — Double Descent, Generalization Bounds & Loss-Landscape Geometry (Weeks 10–12)

**Format:** Slide deck outline for lecture delivery (convert to slides in institution's template).

1. **Title slide** — Advanced Artificial Neural Network (Post Graduate): Module 3, Double Descent,
   Generalization Bounds & Loss-Landscape Geometry
2. **Module recap** — Modules 1–2 characterized training dynamics and found minima; Module 3 asks
   how well those minima generalize, and what the landscape around them looks like geometrically
3. **Double descent, beyond the graduate course** — the interpolation threshold as the organizing
   concept
4. **The three axes** — model-size, sample-size, and epoch-wise double descent (Nakkiran et al.)
5. **Theoretical explanations** — effective-capacity accounts; random-matrix-theoretic
   connections back to Module 1's NTK/kernel-regime material
6. **Location vs. magnitude** — what current theory explains well and what remains open
7. **Why classical bounds go vacuous** — VC/Rademacher's worst-case, class-uniform structure at
   deep-network parameter counts
8. **PAC-Bayes, structurally** — posterior vs. prior; the KL-divergence term; the confidence term
9. **PAC-Bayes and sharpness** — the Gaussian-perturbation posterior construction; why flat minima
   tighten the bound, connecting directly back to Module 2
10. **Mode connectivity** — the linear-interpolation barrier; nonlinear (bend-point) low-loss paths
11. **What mode connectivity does and does not imply** — connected manifold vs. identical function
    vs. landscape flatness
12. **The Lottery Ticket Hypothesis, recapped** — one-paragraph pointer to the graduate course
13. **LTH revisited: linear mode connectivity** — the sharper, geometric refinement of "this
    initialization matters"
14. **LTH revisited: current critiques** — scale/learning-rate sensitivity; what pruning evidence
    actually establishes
15. **Module 3 recap** — key results checklist (double-descent axes, PAC-Bayes structure, mode
    connectivity, LTH refinements)
16. **Looking ahead** — "Next: research methods, the open-problems survey, and the capstone
    proposal" teaser slide

**Speaker notes:** slide 9 (PAC-Bayes and sharpness) is this module's key cross-week synthesis
point and the one students most often miss on exams — spend real time making the KL-divergence
formula's connection to posterior variance and training-risk tolerance concrete with the Week 11
lab's numerical example before moving to mode connectivity.
