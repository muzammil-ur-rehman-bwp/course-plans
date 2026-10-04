# Week 10 Lecture Plan — Knowledge Representation and Reasoning (Graduate)
## Topic: Knowledge Graphs

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Represent a small domain as a set of RDF triples. (*Apply*)
2. State the TransE scoring function and margin-based training objective precisely.
   (*Understand*)
3. Train a toy TransE embedding and use it for link prediction. (*Apply*, *Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap | Semantic networks (undergraduate course) as the ancestor of knowledge graphs |
| 0:15–0:40 | RDF triples | Subject-predicate-object representation; a toy knowledge graph |
| 0:40–1:10 | TransE | The h+r≈t intuition, the scoring function, the margin-based ranking loss, negative-triple corruption |
| 1:10–1:20 | Break | — |
| 1:20–1:50 | Link prediction | Ranking candidate entities; worked example on the toy graph |
| 1:50–2:00 | Synthesis | What embeddings add that a pure symbolic triple store cannot do alone |

### Materials/Equipment
- Slides: "Knowledge Graphs and TransE Embeddings"
- Live-coding environment (Jupyter, NumPy)

### Formative Check (in-class)
Given 4 toy triples and a candidate corrupted triple, compute both triples' TransE scores by
hand (with simple 2-D toy vectors) and determine which the margin loss would penalize.

### Link to Lab/Assessment
Lab 10: implement a from-scratch TransE training loop with NumPy on a toy knowledge graph and
perform link prediction (see `lab-manuals/lab-10.md`).
- **Quiz 4 next week** (Weeks 7–8 content).
