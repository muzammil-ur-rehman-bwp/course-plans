# Presentation: Module 1 — NTK Revisited, Mean-Field Theory, Implicit Bias & Feature Learning (Weeks 1–5)

**Format:** Slide deck outline for lecture delivery (convert to slides in institution's template).

1. **Title slide** — Advanced Artificial Neural Network (Post Graduate): Module 1, NTK Revisited,
   Mean-Field Theory, Implicit Bias & Feature Learning
2. **Course scope recap** — this course's topics vs. the graduate-ANN prerequisite and the sibling
   postgraduate courses (AI/ML/DL/KR&R, Advanced)
3. **Scoping a research proposal, previewed early** — one-slide sketch of problem
   statement/survey/approach/feasibility argument, full treatment in Week 13
4. **The NTK, recapped in one sentence** — the graduate course's infinite-width-limit result
5. **The linearization argument, derived** — the function-space ODE $\dot u_t = -\Theta_t u_t$;
   why $\Theta_t\approx\Theta_0$ as width $\to\infty$
6. **Lazy training, named precisely** — relative parameter movement
   $\|\theta_t-\theta_0\|/\|\theta_0\|\to 0$
7. **NTK's sharper critique** — no feature learning by construction; loose bounds; measurable
   kernel drift at practical widths
8. **The mean-field limit** — a distributional view of width $\to\infty$; contrast with NTK's
   frozen kernel
9. **Signal propagation, generalized** — the variance/correlation recursion recovering Xavier/He
   as fixed-point special cases; the order-to-chaos transition
10. **Implicit bias, derived** — gradient descent on separable data converges in direction to the
    max-margin solution (Soudry et al.); the exponential-tail argument
11. **Implicit bias beyond linear** — deep-linear/matrix-factorization bias toward simplicity; the
    open general nonlinear case
12. **Feature learning beyond the kernel regime** — mechanisms that break laziness; kernel drift,
    representation alignment, and the kernel-regression-vs-trained-network gap as signatures
13. **Module 1 recap** — key results checklist (NTK linearization, mean-field recursion, max-margin
    bias, feature-learning signatures)
14. **Looking ahead** — "Next: sharpness and generalization, grokking, scaling laws, and
    statistical-physics approaches to the loss landscape" teaser slide

**Speaker notes:** this module's hardest derivation for students is the NTK function-space ODE and
its connection to the mean-field limit's different scaling convention — budget real board time for
both, since Weeks 6–9's sharpness/SAM and scaling-law material leans on the same "derive, don't
just state" standard this module sets for the whole course.
