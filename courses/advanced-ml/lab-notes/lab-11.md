# Lab Notes 11 — Fairness Metrics and the Impossibility Result

**Concept recap:** the PPV identity $\mathrm{PPV}=pTPR/(pTPR+(1-p)FPR)$ is strictly monotonic in
base rate $p$ for a non-degenerate classifier, so equal PPV under shared TPR/FPR forces equal base
rates — meaning equalized odds and calibration/predictive parity generally cannot coexist when
base rates differ.

**Common pitfalls:**
- In Task C, solving the `ppv_formula` equation for TPR incorrectly — rearrange
  $\mathrm{PPV}=p\cdot\mathrm{TPR}/(p\cdot\mathrm{TPR}+(1-p)\cdot\mathrm{FPR})$ carefully for
  $\mathrm{TPR}$ given fixed $p,\mathrm{FPR},\mathrm{PPV}$; a sign error here produces a TPR
  outside $[0,1]$, which is a signal to recheck the algebra, not a signal that the result is
  simply unusual.
- Confusing "the classifier is miscalibrated" with "the classifier is bad" — a classifier with
  equal TPR/FPR across groups (Task B) is not making an error in any individual prediction; it is
  the *aggregate* PPV that differs due to differing base rates, a subtlety worth stating
  precisely in any write-up.
- In Task D, holding TPR/FPR fixed but accidentally varying them slightly between runs due to a
  copy-paste error — use a single shared `tpr, fpr` definition across all base-rate-gap
  conditions to isolate the effect of the base-rate gap alone.

**Debugging tip:** before trusting Task C's TPR solution, plug it back into `ppv_formula` for
Group B's base rate and confirm it reproduces Group A's PPV to within numerical tolerance.

**Instructor tip:** ask students, after Task C, whether the TPR/FPR gap they found counts as
"the classifier being unfair to Group B" or "the classifier being unfair to Group A" — there is
no single correct answer, and surfacing that ambiguity is the direct point of Week 11 §5's
practical-implication discussion.
