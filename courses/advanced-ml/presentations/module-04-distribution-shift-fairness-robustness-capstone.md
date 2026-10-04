# Presentation: Module 4 — Distribution Shift, Fairness, Robustness & Capstone (Weeks 10–16)

**Format:** Slide deck outline for lecture delivery (convert to slides in institution's template).

1. **Title slide** — Advanced Machine Learning (Post Graduate): Module 4, Distribution Shift,
   Fairness, Robustness & Capstone
2. **Covariate shift, formalized** — the assumption $P(y|x)$ is preserved while $P(x)$ shifts
3. **Importance weighting, derived** — the weighted-risk identity from first principles
4. **Estimating the density ratio** — the domain-classifier-odds trick
5. **Generalization under shift, and its limits** — the divergence-penalty picture; the overlap
   failure mode
6. **Formal fairness criteria** — demographic parity, equalized odds, calibration
7. **The PPV identity and its monotonicity** — the algebraic core of the impossibility result
8. **The impossibility result** — why differing base rates force a tradeoff between calibration
   and equalized odds; the practical, value-laden implication
9. **Robust statistics: motivation** — the sample mean's breakdown point and Chebyshev-only
   guarantee under heavy tails
10. **Median-of-means, derived** — per-group Chebyshev concentration plus Chernoff-boosted
    majority vote; the sub-Gaussian-type confidence interval under only finite variance
11. **Robustness under adversarial contamination** — bounded-error guarantees vs. the sample
    mean's unbounded error; the conceptual link to adversarial robustness in modern ML
12. **Research methods recap** — the precision test; the three-part survey standard; the
    feasibility-argument standard
13. **Open problems survey recap** — the three illustrative open-question shapes (statistical–
    computational gaps; fairness-relaxation tensions; robustness-scaling tradeoffs)
14. **The capstone** — problem statement, 5+ paper survey, proposed approach, feasibility
    argument/preliminary results; evaluated as a research proposal, not a completed project
15. **Module 4 / course recap** — the full five-pillar map, Week 1 through Week 16
16. **Closing** — "Capstone research proposal presentations begin now" transition slide

**Speaker notes:** Week 11's fairness impossibility result is frequently misread by students as
"fairness is impossible to achieve at all" — be explicit, more than once, that it is a statement
about *simultaneously* satisfying multiple *formal* criteria under differing base rates, not a
nihilistic claim about fairness work being pointless; this framing matters for how students carry
the result into their own capstone proposals if relevant.
