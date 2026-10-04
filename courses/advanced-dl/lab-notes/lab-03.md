# Lab Notes 3 — Classifier-Free Guidance and Flow Matching

**Concept recap:** classifier-free guidance combines a conditional and unconditional score from
one jointly-trained network; flow matching regresses a velocity field onto a linear
interpolation path's closed-form velocity, avoiding simulation and Jacobian computation entirely.

**Common pitfalls:**
- Setting the condition-dropout rate too low (near 0%) — the unconditional score then never gets
  enough training signal, and guidance at $w>1$ produces degenerate or NaN-prone results since
  $s_\theta(x,t,\varnothing)$ is poorly estimated.
- Confusing the guidance weight $w$'s role — $w=1$ is plain conditional sampling, not "no
  guidance"; $w=0$ is the unconditional score, not a lower-guidance version of the conditional
  one. Re-derive the formula on paper if the direction of the effect looks backwards in your plot.
- In Task C, forgetting that flow matching's `t` ranges over $[0,1]$ with $t=0$ at pure noise and
  $t=1$ at data — the opposite convention from some diffusion-time indexing — and sampling the
  ODE in the wrong direction as a result.
- Comparing flow-matching and reverse-SDE sample quality using different numbers of training
  steps for the two underlying networks, confounding "which sampler needs fewer integration
  steps" with "which network was trained longer."

**Debugging tip:** before trusting Task B's guided samples, check that at $w=1$ (plain
conditional), your guided-sampling code reduces to using only the conditional score and reproduces
ordinary conditional generation.

**Instructor tip:** have students sketch, before running Task B, what they expect the sample
cloud to look like at $w=7$ vs. $w=1$ — most will correctly predict "tighter around the mean,"
which is a good opportunity to immediately connect that prediction to the derivation's
$p(x\mid c)p(c\mid x)^{w-1}$ sharpening argument rather than treating it as a purely empirical
observation.
