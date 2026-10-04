# Lab Manual 4 — Unification and First-Order Resolution

**Duration:** 3 hours | **Prerequisite:** Week 4 lecture

## Objectives
Implement the unification algorithm (with occurs check) and a FOL resolution step from scratch.

## Setup
Create `lab04.ipynb`. Represent terms as nested tuples: a variable as a lowercase string, a
constant as a capitalized string, and a compound term as `(function_symbol, (arg1, arg2, ...))`.

## Procedure
1. **Task A — Term helpers:** implement `is_variable(t)`, `is_compound(t)`, `function_symbol(t)`,
   and `args(t)` for the tuple-based term representation.
2. **Task B — Unification:** implement `unify(x, y, subst=None)`, `unify_var`, and
   `occurs_check` from the lecture content; test on at least 5 pairs, including one that should
   fail only because of the occurs check (e.g., unifying `x` with `f(x)`), and one that should
   fail for a mismatched function symbol or arity.
3. **Task C — FOL resolution:** implement `standardize_apart` (rename variables with a unique
   suffix) and `fol_resolve(clause1, clause2)`; test it on a clause pair requiring a non-trivial
   unification (not just matching constants).
4. **Task D — Skolemization by hand:** in a markdown cell, Skolemize 2 given `∃`-containing
   sentences by hand, showing the Skolem function/constant introduced for each.

## Expected Output
A notebook with Tasks A–D; a correct unification function passing all 5+ test pairs (including
both failure cases), a working FOL resolution step, and 2 correctly Skolemized sentences.

## Submission
Submit `lab04.ipynb` by the end of the lab session.
