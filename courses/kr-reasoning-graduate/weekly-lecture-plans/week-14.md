# Week 14 Lecture Plan — Knowledge Representation and Reasoning (Graduate)
## Topic: Explainability and Reasoning

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Build a full proof-tree explanation for a derived fact in a forward-chaining rule engine.
   (*Apply*)
2. Build a "why not" explanation identifying the first failing premise for a query that does not
   derive. (*Apply*)
3. State precisely what guarantee a proof-tree explanation provides that a post-hoc ML
   explanation method does not. (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap | Undergraduate course's single-step derivation trace (Week 14 there) |
| 0:15–0:45 | Proof trees | Full recursive justification structure; worked example |
| 0:45–1:10 | "Why not" explanations | Identifying the first failing premise; worked example |
| 1:10–1:20 | Break | — |
| 1:20–1:50 | Symbolic vs. ML explainability | What a proof tree guarantees vs. what a post-hoc feature-attribution method approximates |
| 1:50–2:00 | Synthesis | Where explainability connects to the capstone's own write-up expectations |

### Materials/Equipment
- Slides: "Explanation in Symbolic Reasoning: Proof Trees and the ML Contrast"
- Live-coding environment (Jupyter)

### Formative Check (in-class)
Given a small rule base and a derived query, sketch the proof tree by hand before checking
against the engine's generated output.

### Link to Lab/Assessment
Lab 14: implement proof-tree and "why not" explanation generation over a small rule base (see
`lab-manuals/lab-14.md`).
