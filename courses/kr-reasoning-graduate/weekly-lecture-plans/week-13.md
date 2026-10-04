# Week 13 Lecture Plan — Knowledge Representation and Reasoning (Graduate)
## Topic: Ontology Engineering in Practice

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Write competency questions for a toy domain ontology and audit a sketched class hierarchy
   against them. (*Apply*)
2. Implement a lexical ontology matcher and audit its proposed correspondences. (*Apply*,
   *Analyze*)
3. Describe what Pellet/HermiT check and how Protégé surfaces a detected inconsistency.
   (*Understand*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap | Week 4's tableau algorithm as what a real DL reasoner runs under the hood |
| 0:15–0:45 | Development methodology | The iterative ontology-engineering cycle, competency questions |
| 0:45–1:15 | Ontology alignment | Lexical/structural/instance-based matching, worked example |
| 1:15–1:25 | Break | — |
| 1:25–1:50 | Reasoner tooling | Pellet, HermiT, Protégé — what each does in a real workflow |
| 1:50–2:00 | Synthesis | Where Assignment 3 and the Paper Critique connect to this week |

### Materials/Equipment
- Slides: "Ontology Engineering Practice: Methodology, Alignment, Tooling"
- Live-coding environment (Jupyter)

### Formative Check (in-class)
Given two small, independently named class lists, propose 2–3 candidate alignments by eye, then
compare against the lexical matcher's output.

### Link to Lab/Assessment
Lab 13: implement a toy lexical ontology matcher and audit its proposed correspondences on two
small class lists (see `lab-manuals/lab-13.md`).
- **Assignment 3 assigned** (Weeks 9–12 content). **Paper Critique & Presentation assigned.**
