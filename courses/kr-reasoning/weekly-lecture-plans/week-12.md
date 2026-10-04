# Week 12 Lecture Plan — Knowledge Representation and Reasoning
## Topic: Probabilistic Reasoning and Bayesian Networks in Depth

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Recall Bayesian network structure (DAG, CPTs) and the conditional-independence assumptions it
   encodes. (*Remember*)
2. Apply variable elimination — factor construction, multiplication, and summing out — to answer
   a probabilistic query exactly. (*Apply, Analyze*)
3. Compare variable elimination's cost against plain enumeration on a concrete network.
   (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:20 | BN recap | DAG structure, CPTs, conditional independence (fast recap) |
| 0:20–0:55 | Factors | Representing a factor; factor multiplication; summing out a variable |
| 0:55–1:05 | Break | — |
| 1:05–1:35 | Variable elimination | The algorithm end to end, worked on a small network |
| 1:35–2:00 | Elimination ordering & cost | Why ordering matters (brief); comparing against enumeration |

### Materials/Equipment
- Slides: factor-multiplication diagram, variable-elimination trace on the Burglary/Earthquake/
  Alarm network
- Starter notebook: `Factor` class skeleton

### Formative Check (in-class)
Exercise: given a 3-node network's CPTs, construct the initial factors, multiply two of them by
hand, and sum out one hidden variable to get an intermediate factor.

### Link to Lab/Assessment
Lab 12: Implement a `Factor` class and a `variable_elimination` query function; compare its work
against plain enumeration on a small and a slightly larger network.
