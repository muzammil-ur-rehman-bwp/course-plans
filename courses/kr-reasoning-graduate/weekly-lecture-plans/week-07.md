# Week 7 Lecture Plan — Knowledge Representation and Reasoning (Graduate)
## Topic: Belief Revision and Update

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. State the AGM postulates (K*1)–(K*6) for rational belief revision. (*Understand*)
2. Check whether a candidate belief-change operator satisfies the AGM postulates on a worked
   example. (*Apply*)
3. Distinguish revision from update and explain why they answer different questions. (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap | Why classical logical consequence alone cannot model giving up a belief |
| 0:15–0:50 | The AGM postulates | (K*1)–(K*6) stated and motivated, one at a time, with a counterexample for each if violated |
| 0:50–1:10 | Dalal revision | Semantic (model-based) construction via Hamming distance |
| 1:10–1:20 | Break | — |
| 1:20–1:50 | Revision vs. update | Katsuno–Mendelzon update postulates contrasted; worked side-by-side example |
| 1:50–2:00 | Synthesis | Where belief revision connects to Week 8's argumentation (both reason about conflicting information) |

### Materials/Equipment
- Slides: "AGM Belief Revision and the Revision/Update Distinction"
- Whiteboard for the postulate-by-postulate derivation
- Live-coding environment (Jupyter)

### Formative Check (in-class)
Given a small belief set K (as a set of models) and a candidate revision outcome, check each AGM
postulate (K*1)–(K*6) against it and identify any violation.

### Link to Lab/Assessment
Lab 7: implement Dalal's Hamming-distance revision operator and verify the core AGM postulates
hold on a toy example (see `lab-manuals/lab-07.md`).
- **Quiz 2 this week** (Week 4 content).
