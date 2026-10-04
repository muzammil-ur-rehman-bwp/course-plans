# Week 6 Lecture Plan — Advanced Knowledge Representation and Reasoning (Post Graduate)
## Topic: Probabilistic Logic Programming

**Duration:** 2 hours lecture + 3 hour research seminar/lab

### Learning Objectives (Bloom's Level)
1. State the distribution semantics (total choices, induced programs, query probability).
   (*Understand, Apply*)
2. Compute a query's probability by hand via total-choice enumeration. (*Apply*)
3. Implement a brute-force distribution-semantics evaluator and contrast it with MLNs'
   log-linear view. (*Apply, Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap | MLN log-linear distribution (graduate course review) |
| 0:15–0:45 | Probabilistic facts & total choices | `p :: fact`, independence, induced programs |
| 0:45–1:15 | The distribution semantics | P(q) as a sum over entailing total choices |
| 1:15–1:40 | Contrast with MLNs | Independent-fact mixture vs. weighted-formula log-linear model |
| 1:40–2:00 | Inference at scale | Brute-force enumeration vs. BDD compilation (conceptual) |

### Materials/Equipment
- Slides: "Probabilistic Logic Programming: The Distribution Semantics"
- Whiteboard for total-choice enumeration
- Jupyter for the evaluator

### Formative Check (in-class)
For a 2-fact, 1-rule toy program, enumerate all total choices, compute each one's probability,
and sum those entailing the query.

### Link to Lab/Assessment
Lab 6: implement the brute-force distribution-semantics evaluator (see `lab-manuals/lab-06.md`).
**Quiz 2** (Weeks 3–4 content) this week.
