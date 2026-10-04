# Lab Manual 4 — SROIQ Fragment Satisfiability Checker

**Duration:** 3 hours | **Prerequisite:** Week 4 lecture

## Objectives
Implement a from-scratch satisfiability checker for an ALC-plus fragment (role hierarchies,
unqualified number restrictions) and trace its new completion rules.

## Setup
1. Reuse your Week 1 environment.
2. Create `lab04.ipynb`.

## Procedure
1. **Task A — Implementation:** implement `Node`, `role_closure`, `propagate_universals`,
   `check_at_least`, and `has_clash` exactly as in the Week 4 lecture content.
2. **Task B — Role-hierarchy propagation:** build the Week 4 worked example (`≥2 hasChild.Happy`
   under `hasChild ⊑ hasRelative` with `∀hasRelative.Known` on the parent) and confirm both
   fresh children end up labeled `Known` in addition to `Happy`.
3. **Task C — Clash detection:** construct a node where the ≥-rule's fresh successors are forced
   into a clash (e.g., a successor required to be both `Happy` and `¬Happy` by a combination of
   restrictions) and confirm `has_clash` correctly detects it.
4. **Task D — Mini-challenge:** extend the fragment with a second role hierarchy level (e.g.,
   `hasChild ⊑ hasRelative ⊑ hasConnection`) and confirm `role_closure` and
   `propagate_universals` correctly propagate a `∀hasConnection.C` restriction two levels down to
   `hasChild`-successors.

## Expected Output
A notebook with four clearly labeled cells/sections (A–D), each producing correct, runnable
output and a printed trace of which rule fired at each step.

## Submission
Export/submit `lab04.ipynb` via the course submission system by the end of the lab session.
