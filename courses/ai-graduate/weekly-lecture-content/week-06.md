# Week 6 — Lecture Content: Automated Reasoning — SAT and SMT

## 1. The Boolean Satisfiability Problem (SAT)
Given a propositional formula in **conjunctive normal form (CNF)** — a conjunction of clauses,
each clause a disjunction of literals (a variable or its negation) — SAT asks whether there is an
assignment of truth values to variables that makes the whole formula true. Example CNF formula:
(¬A ∨ B) ∧ (A ∨ ¬B ∨ C) ∧ (¬C).

## 2. The DPLL Algorithm
DPLL (Davis–Putnam–Logemann–Loveland) is a sound and complete backtracking search algorithm for
SAT, built around two simplification rules applied before branching:

- **Unit propagation.** If a clause has exactly one unassigned literal and all its other literals
  are false under the current partial assignment (a "unit clause"), that literal must be set to
  true (or its variable's negation set to false) for the formula to be satisfiable — assign it,
  simplify the formula, and repeat.
- **Pure-literal elimination.** If a variable appears with only one polarity (always positive or
  always negative) across all remaining clauses, assign it to satisfy that polarity — this can
  never hurt satisfiability.

**Algorithm:**
```
DPLL(clauses, assignment):
    clauses, assignment = unit_propagate(clauses, assignment)
    if clauses contains an empty clause: return UNSAT
    if clauses is empty: return SAT, assignment
    clauses, assignment = pure_literal_eliminate(clauses, assignment)
    if clauses is empty: return SAT, assignment
    var = choose_unassigned_variable(clauses)
    for value in [True, False]:
        result = DPLL(simplify(clauses, var, value), assignment + {var: value})
        if result is SAT: return result
    return UNSAT
```

```python
def dpll(clauses, assignment=None):
    """clauses: list of frozensets of literals (positive int = var, negative int = ¬var).
    assignment: dict var -> bool. Returns (True, assignment) or (False, None)."""
    assignment = dict(assignment) if assignment else {}
    clauses = list(clauses)

    # Unit propagation
    changed = True
    while changed:
        changed = False
        for clause in clauses:
            unassigned = [lit for lit in clause
                          if abs(lit) not in assignment]
            if not unassigned:
                satisfied = any((lit > 0) == assignment.get(abs(lit), False)
                                 for lit in clause if abs(lit) in assignment)
                if not satisfied:
                    return False, None  # empty/falsified clause -> conflict
                continue
            satisfied = any((lit > 0) == assignment.get(abs(lit)) for lit in clause
                             if abs(lit) in assignment)
            if not satisfied and len(unassigned) == 1:
                lit = unassigned[0]
                assignment[abs(lit)] = lit > 0
                changed = True

    simplified = _simplify(clauses, assignment)
    if simplified is None:
        return False, None
    if not simplified:
        return True, assignment

    var = abs(next(iter(simplified[0])))
    for value in (True, False):
        trial = dict(assignment)
        trial[var] = value
        sat, result = dpll(clauses, trial)
        if sat:
            return True, result
    return False, None

def _simplify(clauses, assignment):
    """Remove satisfied clauses; drop falsified literals. Return None on an empty (falsified)
    clause, else the remaining list of clauses (each a frozenset of unassigned literals)."""
    remaining = []
    for clause in clauses:
        if any((lit > 0) == assignment.get(abs(lit)) for lit in clause if abs(lit) in assignment):
            continue  # clause already satisfied
        unassigned = frozenset(lit for lit in clause if abs(lit) not in assignment)
        if not unassigned:
            return None  # all literals falsified -> conflict
        remaining.append(unassigned)
    return remaining
```

**Soundness and completeness.** DPLL is sound (if it reports SAT, the returned assignment truly
satisfies every clause — unit propagation and branching only ever make assignments consistent
with at least one way of satisfying the remaining clauses) and complete (if no satisfying
assignment exists, every branch eventually derives an empty/falsified clause and the algorithm
correctly reports UNSAT, because it exhaustively considers both truth values for every variable
along every branch not pruned by propagation).

## 3. Clause Learning (Conceptual)
Plain DPLL backtracks "blindly" on conflict — it tries the other branch at the most recent
decision point but does not remember *why* the conflict happened. Modern **CDCL (Conflict-Driven
Clause Learning)** solvers analyze the conflict (via an implication graph) to derive a new
**learned clause** that records the combination of assignments that caused the conflict, adds it
to the clause database, and backtracks non-chronologically to the point where that learned clause
becomes a unit clause — triggering immediate propagation and pruning large parts of the search
space that would otherwise be revisited. This is the single most important idea behind why modern
industrial SAT solvers (e.g., those underlying `python-sat`/PySAT) can solve formulas with
millions of variables, far beyond the reach of plain DPLL. This course treats clause learning at
this conceptual level; a from-scratch CDCL implementation is not required.

## 4. Satisfiability Modulo Theories (SMT), Conceptually
SMT generalizes SAT: instead of only propositional variables, formulas may contain atoms from a
background theory — e.g., linear arithmetic over integers/reals (x + 2y ≤ 5), arrays, or
uninterpreted functions. An SMT solver combines a SAT solver (handling the Boolean structure, via
DPLL/CDCL) with theory-specific decision procedures (deciding satisfiability of a conjunction of
theory atoms) in a loop: the SAT solver proposes a satisfying assignment to the "abstracted"
Boolean skeleton, the theory solver checks whether that assignment's atoms are jointly consistent
in the theory, and if not, the theory solver returns a conflicting subset that is added as a new
clause (closing the loop much like clause learning). SMT solvers (e.g., Z3, CVC5) are the engine
behind most modern program/hardware verification tools and scheduling/optimization systems that
need to express numeric or structural constraints SAT alone cannot represent naturally.

## 5. In-Class/Lab Exercise
Trace `dpll` by hand on the formula (¬A ∨ B) ∧ (A ∨ ¬B ∨ C) ∧ (¬C) — identify the unit clause
that fires first, and determine satisfiability; then run the Python implementation to confirm.
