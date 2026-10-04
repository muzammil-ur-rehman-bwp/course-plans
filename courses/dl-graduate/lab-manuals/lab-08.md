# Lab Manual 8 — A Graph Convolutional Network From Scratch

**Duration:** 3 hours | **Prerequisite:** Week 8 lecture

## Objectives
Implement a GCN layer from scratch and stack two layers for node classification on a small
graph.

## Setup
Create `lab08.ipynb`. Start from the lecture's `GCNLayer` and `TwoLayerGCN`. Use a small
benchmark graph (e.g., a toy synthetic graph, or Cora/CiteSeer via `torch_geometric` if
available).

## Procedure
1. **Task A — Normalized adjacency:** implement the $\tilde D^{-1/2}\tilde A\tilde D^{-1/2}$
   computation as a standalone function; verify it by hand against a provided 5-node toy graph.
2. **Task B — GCN layer:** implement `GCNLayer`; confirm output shape is `(num_nodes,
   out_features)`.
3. **Task C — Two-layer GCN for node classification:** train `TwoLayerGCN` on a small
   node-classification dataset (synthetic or Cora/CiteSeer); report classification accuracy on a
   held-out node split.
4. **Task D — Ablation:** remove the self-loop addition ($\tilde A = A$ instead of $A+I$) and
   retrain; compare accuracy and discuss why omitting self-loops changes the result.

## Expected Output
A notebook with Tasks A–D; Task C's accuracy and Task D's ablation comparison reported.

## Submission
Submit `lab08.ipynb` by the end of the lab session.
