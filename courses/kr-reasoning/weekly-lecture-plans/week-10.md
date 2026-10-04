# Week 10 Lecture Plan — Knowledge Representation and Reasoning
## Topic: Planning in Depth

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Extend STRIPS action schemas with variables and grounding over domain objects. (*Apply*)
2. Apply partial-order planning (open-precondition resolution, threat detection) to build a plan
   without committing to a total action order prematurely. (*Apply, Analyze*)
3. Explain how a planning graph is built (proposition/action levels, mutexes) and how GraphPlan
   uses it to extract a plan. (*Understand*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:25 | STRIPS revisited | Variabilized action schemas; grounding into concrete actions |
| 0:25–0:55 | Partial-order planning | Open preconditions, causal links, threats and their repair, worked example |
| 0:55–1:05 | Break | — |
| 1:05–1:35 | Planning graphs | Proposition/action levels, mutex relations, building the graph |
| 1:35–2:00 | GraphPlan (conceptual) | Extracting a plan by searching backward through the graph; capstone kickoff |

### Materials/Equipment
- Slides: grounding example, POP causal-link diagram, planning-graph level diagram
- Starter notebook: variabilized STRIPS schema and partial-order planner skeleton

### Formative Check (in-class)
Exercise: given a 2-schema domain with variables, ground both schemas over a 3-object domain by
hand; then, for a 2-goal planning problem, identify one threat a partial-order planner must
detect and repair.

### Link to Lab/Assessment
Lab 10: Extend STRIPS schemas with variables/grounding and implement a small partial-order
planner for a toy domain; construct the first levels of a planning graph by hand.
