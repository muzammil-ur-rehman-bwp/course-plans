# Lab Notes 2 — Verifying the Finite-Class PAC Bound

**Concept recap:** the bound $m_H(\epsilon,\delta)$ guarantees failure probability $\leq\delta$ —
it is an upper bound on the true failure probability, not an exact value, because the union bound
used to derive it (Week 2, Section 4) is itself conservative.

**Common pitfalls:**
- Treating the bound as tight and expecting the empirical failure rate to equal $\delta$ exactly —
  it is normal, and expected, for the true rate to sit well below $\delta$.
- Simulating fewer trials than needed to estimate a small failure probability reliably (e.g., 100
  trials cannot reliably distinguish a 1% true failure rate from a 5% one) — use at least a few
  thousand trials when $\delta$ is small.
- Misapplying the realizability assumption: this bound requires some $h^\star\in H$ with
  $L_D(h^\star)=0$; if the simulation setup is not actually realizable, the bound as derived does
  not apply (the agnostic, two-sided bound from Week 2 Section 6 would be needed instead).

**Debugging tip:** if the empirical failure rate *exceeds* $\delta$, first check that `true_h_idx`
genuinely achieves zero empirical risk every trial (a realizability bug) before suspecting the
bound itself.

**Instructor tip:** push students on Task D to notice the bound's conservativeness scales with
$m$ relative to $m_H$ — this previews why VC/Rademacher bounds (Weeks 3–4), while also
conservative, are the best *distribution-free* tools we have, and why cross-validation
(Week 14) exists as a complementary, tighter, data-dependent estimate.
