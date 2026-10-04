# Lab Manual 14 — Proof-Tree and "Why Not" Explanation Generator

**Duration:** 3 hours | **Prerequisite:** Week 14 lecture

## Objectives
Implement proof-tree and recursive "why not" explanation generation over a small rule base.

## Setup
1. Reuse your course virtual environment.
2. Create `lab14.ipynb`.

## Procedure
1. **Task A — Proof trees:** implement `ProofNode` and `forward_chain_with_proof` from the
   Week 14 lecture content. Reproduce the Flies(tweety) worked example and print its proof tree.
2. **Task B — Why-not (single level):** implement `why_not` and reproduce the Flies(polly)
   example, confirming it correctly identifies rule R2's missing premise CanFlap(polly).
3. **Task C — Why-not (recursive):** extend `why_not` to recurse through a missing premise's own
   rule until it reaches a genuinely missing leaf fact; reproduce the full recursive trace for
   Flies(polly) down to HasWings(polly).
4. **Task D — Mini-challenge:** build a slightly larger rule base (5–6 rules) with at least one
   fact that is missing two levels deep, and generate both the proof tree for a derivable query
   and the recursive why-not trace for a non-derivable one.

## Expected Output
A notebook with four clearly labeled cells/sections (A–D), each producing correct, runnable
output.

## Submission
Export/submit `lab14.ipynb` via the course submission system by the end of the lab session.
