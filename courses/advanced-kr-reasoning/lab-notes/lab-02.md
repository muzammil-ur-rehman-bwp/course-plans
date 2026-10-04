# Lab Notes 2 — Simply-Typed Lambda Calculus Interpreter

**Concept recap:** types are built from base types and the `→` constructor; abstraction
`λx:σ. M` builds a function of type `σ→τ`; application checks the function's domain type matches
the argument's type; β-reduction substitutes the argument for the bound variable in the body.

**Common pitfalls:**
- Forgetting variable **shadowing** in `substitute` — if an inner `Abs` rebinds the same variable
  name being substituted, substitution must stop at that point (the reference implementation's
  `if term.var == var: return term` guard handles this; omitting it silently captures the wrong
  variable).
- Confusing `type_check`'s direction — the function's type's `dom` must equal the *argument's*
  type, not the other way around; a common bug swaps `fun_type.dom` and `arg_type` in the
  equality check.
- In Task D, applying arguments in the wrong order — `introduce`'s curried argument order in the
  lecture content is (object, indirect object, subject), so the *first* application must be with
  `carol` (the object), not `alice`.

**Debugging tip:** print the type of every intermediate term as you build up a Task D-style
derivation; an ill-typed intermediate step is far easier to catch immediately than after the full
term is assembled.

**Instructor tip:** have students manually trace one β-reduction step on paper before running the
code, matching the Week 2 formative check — the automated `beta_reduce` can mask an
off-by-one substitution-scoping error that a hand trace exposes immediately.
