# Lab Manual 9 — GraphSAGE and Graph Attention Networks

**Duration:** 3 hours | **Prerequisite:** Week 9 lecture

## Objectives
Implement GraphSAGE-style mean aggregation and GAT-style attention aggregation, and compare both
against Week 8's GCN on the same small graph.

## Setup
Create `lab09.ipynb`. Start from the lecture's `GraphSAGELayer` and `GATLayer`, and reuse Lab 8's
`GCNLayer` and dataset.

## Procedure
1. **Task A — GraphSAGE layer:** implement `GraphSAGELayer`; train a two-layer GraphSAGE model on
   the same node-classification dataset as Lab 8; report accuracy.
2. **Task B — GAT layer:** implement `GATLayer`; train a two-layer GAT model on the same dataset;
   report accuracy and inspect the learned attention weights for a few nodes.
3. **Task C — Three-way comparison:** tabulate GCN (Lab 8), GraphSAGE, and GAT accuracy and
   parameter count on the same dataset/split.
4. **Task D — Attention inspection:** for one node with 3+ neighbors, print its learned GAT
   attention weights and discuss, in a markdown cell, whether they are close to uniform (like
   GCN's fixed weighting) or sharply peaked on specific neighbors.

## Expected Output
A notebook with Tasks A–D; Task C's comparison table and Task D's attention-weight printout.

## Submission
Submit `lab09.ipynb` by the end of the lab session.
