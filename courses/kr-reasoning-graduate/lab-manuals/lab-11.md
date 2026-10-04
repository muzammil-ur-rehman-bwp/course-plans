# Lab Manual 11 — Rule Mining and Signal Combination

**Duration:** 3 hours | **Prerequisite:** Week 11 lecture

## Objectives
Implement a simple closed-path rule miner over a toy knowledge graph and combine its output with
Week 10's TransE scores to rank candidate facts.

## Setup
1. Reuse your course virtual environment (NumPy available from Lab 10).
2. Create `lab11.ipynb`.

## Procedure
1. **Task A — Rule miner:** implement `rule_support_confidence` from the Week 11 lecture content.
   On a toy graph with `worksFor`/`locatedIn` facts and a few confirming `basedIn` facts, compute
   the support and confidence of the `worksFor(X,Y) ∧ locatedIn(Y,Z) ⇒ basedIn(X,Z)` rule.
2. **Task B — Uncovered candidates:** identify 2–3 candidate `basedIn` facts the Task A rule does
   **not** cover (no matching body binding exists), and confirm this with your implementation.
3. **Task C — Signal combination:** reuse (or retrain) a TransE model from Lab 10 on the same
   toy graph, implement `combine_signals` from the Week 11 lecture content, and rank the Task B
   candidates alongside 1–2 rule-covered candidates.
4. **Task D — Mini-challenge:** write a short paragraph (markdown cell) identifying one case in
   your ranked output where the rule-based and embedding-based signals disagree, and explain, in
   terms of §4 of the lecture content, why that disagreement is plausible.

## Expected Output
A notebook with four clearly labeled cells/sections (A–D), each producing correct, runnable
output (A–C) or a clear written analysis (D).

## Submission
Export/submit `lab11.ipynb` via the course submission system by the end of the lab session.
