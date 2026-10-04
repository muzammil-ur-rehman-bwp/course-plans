# Lab Manual 10 — TransE Knowledge-Graph Embedding

**Duration:** 3 hours | **Prerequisite:** Week 10 lecture

## Objectives
Implement a from-scratch TransE training loop with NumPy on a toy knowledge graph and perform
link prediction.

## Setup
1. Reuse your course virtual environment; `pip install numpy` if not already available.
2. Create `lab10.ipynb`.

## Procedure
1. **Task A — Triple store:** encode a toy knowledge graph of at least 6 entities and 2 relation
   types (e.g., `worksFor`, `locatedIn`) as a list of RDF triples.
2. **Task B — TransE training:** implement `init_embeddings`, `score`, `corrupt`, and
   `train_transe` from the Week 10 lecture content. Train on the Task A triples and report the
   final average positive-triple distance vs. average negative-triple distance.
3. **Task C — Link prediction:** hold out one triple, retrain on the rest, and use
   `predict_tail` to rank all candidate tail entities for the held-out (h, r, ?) query; report
   where the true answer ranks.
4. **Task D — Mini-challenge:** vary the embedding dimension (e.g., 4 vs. 16) and the margin
   (e.g., 0.5 vs. 2.0), and report, in a short table, how the held-out triple's rank changes.

## Expected Output
A notebook with four clearly labeled cells/sections (A–D), each producing correct, runnable
output.

## Submission
Export/submit `lab10.ipynb` via the course submission system by the end of the lab session.
