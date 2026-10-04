# Week 10 Lecture Plan — Advanced Knowledge Representation and Reasoning (Post Graduate)
## Topic: Multi-Agent Belief Merging

**Duration:** 2 hours lecture + 3 hour research seminar/lab

### Learning Objectives (Bloom's Level)
1. Explain how distance-based merging generalizes Dalal revision to multiple belief sets.
   (*Understand, Apply*)
2. State the standard merging postulates and evaluate an operator against them. (*Apply,
   Analyze*)
3. Contrast multi-agent merging with single-agent AGM revision on parallel examples.
   (*Analyze, Evaluate*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap | AGM revision and Dalal revision (graduate course review) |
| 0:15–0:45 | Belief profiles and merging | Multiple peer belief sets; integrity constraints IC |
| 0:45–1:15 | Distance-based merging operators | Aggregate Hamming distance; model selection |
| 1:15–1:40 | Merging postulates | IC0–IC3 stated and checked on worked examples |
| 1:40–2:00 | Merging vs. revision | Peer belief sets vs. one privileged set plus new input |

### Materials/Equipment
- Slides: "Multi-Agent Belief Merging"
- Whiteboard for model-enumeration worked examples

### Formative Check (in-class)
For two toy agent belief sets and an integrity constraint, enumerate valuations, compute each
one's aggregate distance, and identify the merged result's models.

### Link to Lab/Assessment
Lab 10: implement a distance-based belief-merging evaluator and check merging postulates (see
`lab-manuals/lab-10.md`). **Quiz 4** (Weeks 7–9 content) this week. **Assignment 2 assigned**
this week.
