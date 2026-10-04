# Week 11 Lecture Plan — Introduction to AI
## Topic: Bayesian Networks

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain how a Bayesian network represents a joint distribution compactly using conditional independence. (*Understand*)
2. Apply inference by enumeration to compute a query probability on a small network. (*Apply*)
3. Analyze a network's structure to identify which variables are conditionally independent given evidence. (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:25 | Why Bayesian networks? | The cost of a full joint distribution; factoring it using conditional independence |
| 0:25–0:55 | Network structure | Nodes, directed edges, conditional probability tables (CPTs); the "classic alarm" style example |
| 0:55–1:05 | Break | — |
| 1:05–1:35 | Inference by enumeration | Computing P(query | evidence) by summing over hidden variables, worked by hand |
| 1:35–2:00 | Capstone kickoff | Capstone project introduced in Week 9; proposal guidelines walkthrough, example topics, Q&A |

### Materials/Equipment
- Slides: Bayesian network diagram and CPTs for a worked 3–4 node example
- `assignments/capstone-proposal-guidelines.md` handout

### Formative Check (in-class)
Exercise: given a 3-node network (e.g., Burglary → Alarm ← Earthquake) and its CPTs, compute
P(Burglary | Alarm = true) by hand via enumeration.

### Link to Lab/Assessment
Lab 11: Implement inference by enumeration on a small hand-built Bayesian network in Python.
Assignment 3 due this week; capstone proposal due this week.
