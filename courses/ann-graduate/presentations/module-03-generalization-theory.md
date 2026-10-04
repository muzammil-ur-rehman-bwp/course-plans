# Presentation: Module 3 — Generalization Theory (Weeks 9–11)

**Format:** Slide deck outline for lecture delivery.

1. **Title slide** — Module 3: Generalization Theory
2. **The PAC learning framework** — polynomially-many-samples, high-probability, low-error
3. **VC dimension by shattering** — definition; worked example (2D half-planes, VC dim 3)
4. **The classical VC bound** — statement, and why it becomes vacuous for $p \gg m$
5. **The deep learning puzzle** — networks with $p\gg m$ generalize well anyway
6. **Rademacher complexity** — data-dependent capacity via fitting random labels
7. **Margin-based bounds** — why a large-margin solution generalizes better than raw capacity
   suggests
8. **The random-label-fitting phenomenon** — networks fit random labels to zero error, yet
   generalize normally on real data when trained normally
9. **Capacity vs. implicit bias** — the key distinction this module has been building toward
10. **Skip connections, the optimization argument** — identity-at-initialization; Jacobian stays
    near $I$
11. **Attention, the expressivity argument** — $O(1)$-length information paths between positions
12. **Module recap** — classical capacity theory (VC) cannot explain deep learning alone;
    data-dependent measures (Rademacher, margin) and implicit bias get closer; onward to specific
    modern theories that attempt to close the gap (Module 4)

**Speaker notes:** slide 9 (capacity vs. implicit bias) is this module's hinge slide — make sure
students can state, unprompted, why "this network can memorize random labels" and "this network
overfits on real data" are different claims, using slide 8's own numbers as the evidence.
