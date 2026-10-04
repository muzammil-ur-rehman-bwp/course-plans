# Week 11 Lecture Plan — Knowledge Representation and Reasoning (Graduate)
## Topic: Reasoning over Knowledge Graphs

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Compute support and confidence for a candidate closed-path rule over a toy knowledge graph.
   (*Apply*)
2. Combine mined-rule candidates with TransE scores to rank candidate facts. (*Apply*, *Analyze*)
3. Explain, with concrete examples, what symbolic rules and learned embeddings each contribute
   and each lack. (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap | Week 10's TransE link prediction as one way to generalize beyond stated facts |
| 0:15–0:45 | Rule mining | Closed-path rules, support/confidence, worked by hand on a toy graph |
| 0:45–1:10 | Neuro-symbolic survey | Combining mined/hand-written rules with embedding scores; a grounded, non-hype framing |
| 1:10–1:20 | Break | — |
| 1:20–1:50 | Worked combination | Ranking candidate facts using both signals; where they agree/disagree |
| 1:50–2:00 | Synthesis | Rules vs. embeddings: exact/explainable vs. noise-tolerant/unexplainable |

### Materials/Equipment
- Slides: "Rule Mining and Neuro-Symbolic Reasoning over Knowledge Graphs"
- Live-coding environment (Jupyter, NumPy)

### Formative Check (in-class)
Given a toy graph and a candidate rule template, compute support and confidence by hand before
checking with code.

### Link to Lab/Assessment
Lab 11: implement a simple closed-path rule miner and compare its output against Week 10's
TransE scores on a toy knowledge graph (see `lab-manuals/lab-11.md`).
