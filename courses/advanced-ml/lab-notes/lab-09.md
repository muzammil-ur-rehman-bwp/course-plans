# Lab Notes 9 — Two-Stage-Least-Squares IV Estimation and Do-Calculus by Hand

**Concept recap:** IV identifies a causal effect despite unobserved confounding via
$\beta=\mathrm{Cov}(Z,Y)/\mathrm{Cov}(Z,T)$, given relevance, exclusion, and independence from the
confounder; the do-calculus's three rules are each a $d$-separation statement in a surgically
modified graph.

**Common pitfalls:**
- Fitting stage 1 of 2SLS incorrectly — stage 1 regresses the *treatment* $T$ on the instrument
  $Z$ to get fitted values $\hat T$; a common error is regressing $Y$ on $Z$ directly in stage 1,
  which produces a different (and not generally correct) quantity.
- In Task C/D, citing the wrong do-calculus rule for a step — double-check which graph
  modification ($G_{\overline X}$, $G_{\underline X}$, or a combination) each rule requires, and
  explicitly state which nodes you removed edges from before claiming a $d$-separation holds.
- In Task D, concluding "no estimator can ever estimate this effect" rather than the more precise
  claim "no estimator can identify this effect from $X,Y$'s joint observational distribution
  alone, absent further assumptions or an added instrument" — non-identifiability is a statement
  about what the observational distribution alone determines, not an absolute impossibility claim
  independent of any additional structure.

**Debugging tip:** verify your 2SLS implementation's stage-1 fit by checking that
`np.corrcoef(T, T_hat)[0,1]` is reasonably high (since $Z$ is a strong instrument in this
simulation by construction) — a near-zero correlation would indicate a stage-1 bug, not a weak
instrument.

**Instructor tip:** have students attempt Task D *before* being told it is non-identifiable —
watching exactly where their own derivation attempt stalls (no observed node blocks the back-door
path) is a more durable lesson than being told the conclusion directly.
