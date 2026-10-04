# Presentation: Module 3 — Causal Inference in Depth (Weeks 8–9)

**Format:** Slide deck outline for lecture delivery (convert to slides in institution's template).

1. **Title slide** — Advanced Machine Learning (Post Graduate): Module 3, Causal Inference in
   Depth
2. **From conceptual to rigorous** — what the graduate course's confounding/do-notation
   introduction assumed, and what this module builds on top of it
3. **The potential-outcomes framework** — $Y_i(1),Y_i(0)$; the fundamental problem of causal
   inference
4. **Formalizing confounding** — $T\not\perp(Y(0),Y(1))$; unconfoundedness and overlap, precisely
5. **The propensity score and IPW** — definition; deriving the IPW-ATE identity via the tower
   property
6. **Midterm bridge** — Weeks 1–8 recap map (previewing the midterm's scope)
7. **When unconfoundedness fails** — motivating instrumental variables
8. **The three IV assumptions** — relevance; exclusion restriction; independence from the
   confounder
9. **The linear IV identification argument** — deriving $\beta=\mathrm{Cov}(Z,Y)/
   \mathrm{Cov}(Z,T)$; two-stage least squares
10. **The do-calculus** — building rigorously on the graduate course's conceptual
    $P(Y\mid\mathrm{do}(X))$ introduction
11. **The three do-calculus rules** — precise statements; the graph-surgery/$d$-separation
    justification for soundness; completeness
12. **Worked example: an identifiable confounded graph** — recovering the back-door adjustment
    formula from the rules
13. **Worked example: a non-identifiable graph** — where no rule sequence succeeds, and why IV is
    a genuinely different identification route
14. **Module 3 recap** — key results checklist (the IPW-ATE identity, the linear-IV formula, the
    three do-calculus rules)
15. **Looking ahead** — "Next: distribution shift, algorithmic fairness, and robust statistics"
    teaser slide

**Speaker notes:** the do-calculus module is this course's single hardest piece of material —
most students need to see the worked identifiable *and* non-identifiable examples side by side,
with the exact graph-surgery operation named at every step, before the rules stop feeling like
arbitrary syntax; do not compress Weeks 8–9's board time to make room elsewhere.
