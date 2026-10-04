# Lab Notes 5 — ASPIC+ Argument-Construction-and-Attack Calculator

**Concept recap:** arguments are trees built from strict/defeasible rules over premises; rebut
targets a sub-argument's *conclusion*; undercut targets a sub-argument's top (defeasible) *rule's
applicability*; ASPIC+ hands its resulting attack relation to Dung's semantics unchanged.

**Common pitfalls:**
- Allowing a rebut or undercut against a **strict**-rule-concluded sub-argument — by definition,
  neither attack type is legitimate against a strict top rule; a bug that lets `attacks` fire
  against a strict conclusion will silently produce an invalid ASPIC+ attack relation.
  `build_arguments`'s depth bound can also let combinatorial explosion mask this bug by burying
  one bad attack among many correct ones — isolate and test Task B/C's two specific arguments
  directly before trusting the full enumeration.
- Confusing "the conclusion that denies a rule's applicability" with "the opposite conclusion" —
  Task C's `not_appl(...)` naming convention is deliberately distinct from the plain
  `not_flies`-style negated-conclusion convention used for rebuttal; mixing the two conventions up
  is the single most common Task C bug.
- Forgetting that `max_depth` in `build_arguments` must be large enough to reach the conclusion
  under test — too shallow a depth silently produces an incomplete argument set with no error.

**Debugging tip:** print every constructed argument's conclusion and top-rule name before
computing attacks; visually confirm both the flies- and not_flies-arguments actually appear in
the enumerated set before checking the attack relation between them.

**Instructor tip:** have students re-derive, by hand on paper, which specific sub-argument an
attack targets before trusting the code's output — the rebut/undercut distinction is exactly the
kind of thing that is easy to get right in code by accident (matching a label) without the
student actually understanding which element is being targeted and why.
