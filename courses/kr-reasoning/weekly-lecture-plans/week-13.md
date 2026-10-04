# Week 13 Lecture Plan — Knowledge Representation and Reasoning
## Topic: Reasoning with Uncertainty Beyond Bayes

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain, at a survey level, how Markov logic networks combine first-order logic with
   probability via weighted formulas. (*Understand*)
2. Define fuzzy sets and membership functions, and apply standard fuzzy set operations.
   (*Understand, Apply*)
3. Evaluate a small fuzzy rule base on a concrete input. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Limits of pure logic and pure BNs | Motivating a need for formalisms combining or generalizing both |
| 0:15–0:45 | Markov logic networks | Weighted FOL formulas; worlds, satisfaction counts, relative preference (conceptual) |
| 0:45–0:55 | Break | — |
| 0:55–1:25 | Fuzzy sets & membership | Membership functions, fuzzy AND/OR/NOT |
| 1:25–2:00 | Fuzzy rule evaluation | A worked fuzzy-rule example (toy temperature/fan-speed controller) |

### Materials/Equipment
- Slides: MLN weighted-formula example, triangular/trapezoidal membership function diagrams
- Starter notebook: fuzzy membership function skeleton

### Formative Check (in-class)
Exercise: for a triangular membership function and a given input value, compute the degree of
membership by hand; then combine two fuzzy truth values with fuzzy AND and fuzzy OR.

### Link to Lab/Assessment
Lab 13: Implement fuzzy membership functions and fuzzy set operations, and evaluate a small fuzzy
rule base on a toy controller example.
