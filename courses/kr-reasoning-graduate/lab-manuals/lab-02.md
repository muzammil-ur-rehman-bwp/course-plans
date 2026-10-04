# Lab Manual 2 — Kripke Model Satisfaction Checker

**Duration:** 3 hours | **Prerequisite:** Week 2 lecture

## Objectives
Implement a Kripke-model satisfaction checker and verify the correspondence between
accessibility-relation properties and the modal systems K/T/S4/S5.

## Setup
1. Reuse your course virtual environment.
2. Create `lab02.ipynb`.

## Procedure
1. **Task A — Satisfaction checker:** implement `satisfies` from the Week 2 lecture content.
   Build the 3-world model from the lecture's worked example and confirm
   `satisfies(model, 'w1', ('box', ('atom','p')))` matches the hand-derived result.
2. **Task B — Correspondence check (T):** build a model whose accessibility relation is NOT
   reflexive at some world w, construct a formula instance where □φ→φ fails at w, and confirm it
   with your checker; then make R reflexive at w and confirm □φ→φ now holds there.
3. **Task C — Correspondence check (S4, S5):** build a model with a transitive-but-not-symmetric
   R and confirm □φ→□□φ (axiom 4) holds but ◇φ→□◇φ (axiom 5) can fail; then make R an
   equivalence relation and confirm axiom 5 now holds everywhere.
4. **Task D — Mini-challenge:** build a small 3-agent epistemic model (one accessibility relation
   per agent, each an equivalence relation) and evaluate a formula of the form K_a(K_b(p)) at a
   chosen world.

## Expected Output
A notebook with four clearly labeled cells/sections (A–D), each producing correct, runnable
output.

## Submission
Export/submit `lab02.ipynb` via the course submission system by the end of the lab session.
