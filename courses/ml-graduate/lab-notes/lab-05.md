# Lab Notes 5 — Hoeffding's Inequality: Verification and Assumption Failure

**Concept recap:** Hoeffding's inequality bounds the probability a sample mean of i.i.d.,
bounded variables deviates from its true mean by more than $\epsilon$. The bound's validity rests
entirely on independence and boundedness — **misapplying Hoeffding's inequality outside its
i.i.d./boundedness assumptions** (e.g., to serially correlated data, or to unbounded data without
first truncating or using a sub-Gaussian version) is a serious and common error.

**Common pitfalls:**
- Applying the bound to a sequence with any temporal or spatial correlation (e.g., consecutive
  frames of a video, repeated measurements of the same subject) without first checking whether the
  i.i.d. assumption is remotely plausible for that data.
- Using the bound's $2\exp(-2m\epsilon^2)$ form for variables that are not bounded in $[0,1]$
  without first rescaling, or without using the general $[a,b]$-bounded form from Week 5's
  derivation.
- Treating a *rejection* of the bound in simulation (Task C/D) as evidence the inequality itself
  is "wrong" — it is not wrong; its assumptions were violated, which is a different statement.

**Debugging tip:** if Task C's correlated-sequence tail probability does *not* visibly exceed the
i.i.d. case, increase the correlation strength (lower the `flip` probability in the lecture's
code) — weak correlation can still leave the bound looking approximately valid in a small
simulation.

**Instructor tip:** use Task E to have students name a real ML scenario (e.g., bootstrap resamples
used inside a single run of cross-validation, or test points from a time series) where naively
reaching for Hoeffding's inequality would be a mistake, and discuss what each scenario would need
instead (e.g., a block bootstrap, or a martingale-based concentration inequality — mentioned only
by name, not derived).
