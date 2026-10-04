# Lab Manual 11 — Black-Box Justification Finder

**Duration:** 3 hours | **Prerequisite:** Week 11 lecture

## Objectives
Implement a black-box minimal-justification finder and use it to surface multiple distinct
justifications for a single toy entailment.

## Setup
1. Reuse your Week 1 environment, plus your Week 6 `least_model` function (reused as the
   entailment-checking sub-procedure).
2. Create `lab11.ipynb`.

## Procedure
1. **Task A — Implementation:** implement `entails` and `all_justifications` exactly as in the
   Week 11 lecture content, using `least_model` as the `entailment_checker`.
2. **Task B — Toy ontology:** hand-craft a toy "ontology" (5–7 propositional-Horn-style axioms,
   as a stand-in for DL axioms) with a query entailed by two genuinely disjoint minimal subsets.
3. **Task C — Justification finding:** run `all_justifications` and confirm it reports exactly
   the two expected justifications and no non-minimal supersets.
4. **Task D — Minimality verification:** for each found justification, remove one axiom at a time
   and confirm the entailment breaks every time (verifying minimality by hand, not just trusting
   the algorithm).

## Expected Output
A notebook with four clearly labeled cells/sections (A–D), each producing correct, runnable
output and the Task D minimality verification explicitly shown.

## Submission
Export/submit `lab11.ipynb` via the course submission system by the end of the lab session.
