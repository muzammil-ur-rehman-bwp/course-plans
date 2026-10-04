# Week 8 Lecture Plan — Deep Learning (Graduate)
## Topic: Graph Neural Networks I; Midterm Review

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Represent graph-structured data (adjacency, node/edge features) for learning. (*Understand*)
2. Explain the message-passing (aggregate-and-update) framework. (*Understand*)
3. Derive and implement the Graph Convolutional Network (GCN) layer equation. (*Apply, Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Motivation | Why CNNs/Transformers do not directly apply to irregular graph structure |
| 0:15–0:35 | Representing graphs | Adjacency matrix/list, node feature matrix, edge features |
| 0:35–1:00 | Message passing | Aggregate-and-update framing; one round traced by hand on a toy graph |
| 1:00–1:10 | Break | — |
| 1:10–1:40 | The GCN layer | Spectral motivation (brief); the normalized-adjacency spatial form; degree normalization |
| 1:40–2:00 | Midterm review | Weeks 1–8 topic map and sample question types |

### Materials/Equipment
- Live-coding environment, PyTorch, `torch_geometric` (or from-scratch adjacency ops)
- Slide diagram: a small toy graph with one message-passing round annotated

### Formative Check (in-class)
Given a 4-node toy graph's adjacency matrix, students compute the normalized adjacency
$\tilde D^{-1/2}\tilde A\tilde D^{-1/2}$ by hand for one GCN layer's forward pass.

### Link to Lab/Assessment
Lab 8: Implementing a GCN layer from scratch and a two-layer GCN for node classification on a
small graph (see `lab-manuals/lab-08.md`).
