# Week 3 — Lecture Content: Temporal Logic

## 1. Why Temporal Logic
Point-based facts ("p is true") say nothing about *when*, or about how truth evolves. Temporal
logic reasons about a system's behavior over a sequence of states — exactly what is needed to
state planning goals ("eventually reach a goal state"), safety constraints ("never enter a bad
state"), and verification properties ("every request is eventually answered").

## 2. Linear Temporal Logic (LTL)
LTL formulas are evaluated over an infinite (in practice, a long/looping, or for teaching
purposes a finite) **trace** π = s₀, s₁, s₂, … of states, each state a propositional valuation.
Write π,i ⊨ φ for "φ holds at position i of trace π." Propositional connectives behave as usual;
the temporal operators are:

- **Next**: π,i ⊨ Xφ  iff  π,i+1 ⊨ φ.
- **Always (globally)**: π,i ⊨ Gφ  iff  π,j ⊨ φ for every j ≥ i.
- **Eventually**: π,i ⊨ Fφ  iff  π,j ⊨ φ for some j ≥ i.
- **Until**: π,i ⊨ φUψ  iff  there is some j ≥ i with π,j ⊨ ψ, and π,k ⊨ φ for every k with
  i ≤ k < j.

Fφ and Gφ are duals (Fφ ≡ ¬G¬φ), and both are special cases of U: Fφ ≡ trueUφ.

**Finite-trace convention used in this course's labs.** A genuine LTL trace is infinite; for a
finite trace of length n (indices 0..n−1) used in labs and verification tools, Gφ and Fφ are
evaluated only over the available suffix (indices i..n−1) — this is a standard, explicitly stated
simplification (sometimes called *bounded* or *finite-trace* LTL), not the textbook infinite-trace
semantics, and the lecture content below flags every place this matters.

```python
def ltl_eval(trace, i, formula):
    """trace: list of dicts {atom: bool}. formula: nested tuples, e.g. ('G', ('atom','p'))."""
    n = len(trace)
    if i >= n:
        return None  # out of bounds; caller should not query past the trace
    op = formula[0]
    if op == 'atom':
        return trace[i].get(formula[1], False)
    if op == 'not':
        return not ltl_eval(trace, i, formula[1])
    if op == 'and':
        return ltl_eval(trace, i, formula[1]) and ltl_eval(trace, i, formula[2])
    if op == 'or':
        return ltl_eval(trace, i, formula[1]) or ltl_eval(trace, i, formula[2])
    if op == 'X':
        return i + 1 < n and ltl_eval(trace, i + 1, formula[1])
    if op == 'G':
        return all(ltl_eval(trace, j, formula[1]) for j in range(i, n))
    if op == 'F':
        return any(ltl_eval(trace, j, formula[1]) for j in range(i, n))
    if op == 'U':
        phi, psi = formula[1], formula[2]
        for j in range(i, n):
            if ltl_eval(trace, j, psi):
                return all(ltl_eval(trace, k, phi) for k in range(i, j))
        return False
    raise ValueError(f"unknown operator {op}")
```

## 3. Computation Tree Logic (CTL), Conceptually
LTL formulas describe properties of a *single* trace. Once a system can branch (nondeterministic
choices, or multiple possible continuations from a state — the same Kripke-style structure as
Week 2, but now read as a transition system rather than an epistemic model), we may want to say
"on *every* possible future, eventually φ" versus "on *some* possible future, eventually φ." CTL
adds path quantifiers **A** ("for all paths from here") and **E** ("for some path from here"),
each paired with a next-state temporal operator, e.g.:

- **AGφ**: on every path, φ holds at every step (a safety property that holds no matter what the
  system does).
- **EFφ**: on some path, φ eventually holds (φ is reachable).

**Why AGφ ≠ Gφ and EFφ ≠ Fφ once branching exists.** Gφ/Fφ are properties of one fixed trace;
AGφ/EFφ quantify over the whole branching tree of possible traces from a state. A system can
satisfy EFφ (some path reaches φ) while the *particular* trace you happen to observe never does
— EFφ is a property of the branching structure, not of any single observed run, so it cannot be
checked by running `ltl_eval` on one trace; it requires exploring the branching transition system.

## 4. Applications
- **Planning.** A goal "eventually at a state satisfying G" is Fgoal; a safety constraint "never
  enter a failure state" is G¬fail. This connects directly to the undergraduate course's STRIPS
  goal test, now expressed in a logic that can also state ongoing/safety requirements STRIPS's
  single goal test cannot.
- **Verification.** "Every request is eventually followed by a response" is
  G(request → F response) — a standard liveness property checked by model checkers over a
  system's full branching transition system (hence stated in CTL or in LTL-with-fairness, not
  evaluated on one trace alone).

## 5. In-Class/Lab Exercise
Build a 5-step trace where `request` is true at steps 0 and 2, and `response` is true at steps 1
and 3. Evaluate `G(request → F response)` at step 0 using `ltl_eval` (treating `→` as
`not(request) or response`, nested appropriately) — it should hold. Then remove the response at
step 3 and re-evaluate — it should now fail, since the request at step 2 is never answered within
the trace.
