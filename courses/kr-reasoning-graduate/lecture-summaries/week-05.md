# Week 5 Summary — Automated Theorem Proving in Depth

**Key takeaways:**
- The sequent calculus derives Γ⊢Δ via introduction/elimination rules from axiom sequents φ⊢φ;
  ∀-right/∃-left require a fresh eigenvariable not occurring elsewhere in the sequent.
- The first-order tableau extends Week 4's DL tableau with ∀-instantiation (by any existing term,
  possibly repeated) and ∃-instantiation (by one fresh constant); a formula set is unsatisfiable
  iff every branch closes on a clash.
- Set-of-support and ordering resolution refinements restrict which resolution steps are allowed
  while provably preserving refutation-completeness, pruning the undergraduate course's
  unrestricted resolution search space.

**You should now be able to:** construct a short sequent-calculus derivation; build a first-order
tableau by hand with correct eigenvariable/fresh-constant discipline; implement and compare
set-of-support resolution against unrestricted resolution.

**Next week:** Non-monotonic reasoning via Answer Set Programming — the stable-model semantics,
the Gelfond–Lifschitz reduct, ASP syntax, and solving graph coloring as an ASP program, contrasted
with the undergraduate course's default logic.
