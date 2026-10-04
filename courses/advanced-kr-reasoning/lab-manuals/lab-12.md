# Lab Manual 12 — Ontology Diff Tool

**Duration:** 3 hours | **Prerequisite:** Week 12 lecture

## Objectives
Implement a simple ontology-diff tool reporting entailment changes between two toy ontology
versions, and argue whether a change is backward-compatible.

## Setup
1. Reuse your Week 1 environment, plus your Week 6/11 `least_model`-based entailment checker.
2. Create `lab12.ipynb`.

## Procedure
1. **Task A — Implementation:** implement `ontology_diff` exactly as in the Week 12 lecture
   content.
2. **Task B — Toy ontology versions:** hand-write two small ontology versions (5–8 axioms each),
   version 2 being a deliberate "fix" of a modeling mistake in version 1, plus a fixed list of
   4–5 queries.
3. **Task C — Diff and impact analysis:** run `ontology_diff`; report the syntactic
   added/removed axioms and which queries' entailment status changed.
4. **Task D — Backward-compatibility argument:** write a short, precise paragraph arguing
   whether the version-1-to-2 change should be considered backward-compatible, citing
   specifically which entailments were preserved and which were lost, and state what a dependent
   system relying on a now-lost entailment would need to be told.

## Expected Output
A notebook with four clearly labeled cells/sections (A–D), each producing correct, runnable
output and the required written argument.

## Submission
Export/submit `lab12.ipynb` via the course submission system by the end of the lab session.
