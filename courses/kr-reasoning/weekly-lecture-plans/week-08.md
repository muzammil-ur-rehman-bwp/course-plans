# Week 8 Lecture Plan — Knowledge Representation and Reasoning
## Topic: Constraint Satisfaction in Depth; Midterm Review

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Recall the CSP formulation (variables, domains, constraints) and the constraint-graph view.
   (*Remember*)
2. Apply the AC-3 arc-consistency algorithm to prune variable domains. (*Apply*)
3. Apply backtracking search with variable- and value-ordering heuristics (MRV, degree,
   least-constraining-value) to solve a CSP. (*Apply, Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | CSP recap | Variables, domains, constraints; the constraint graph |
| 0:15–0:45 | Arc consistency | The `revise` subroutine; AC-3's worklist algorithm; what arc consistency guarantees (and doesn't) |
| 0:45–0:55 | Break | — |
| 0:55–1:25 | Backtracking heuristics | MRV, degree heuristic, least-constraining-value; forward checking as AC-3-lite during search |
| 1:25–2:00 | Midterm review | Topic list and practice problems for Weeks 1–8 |

### Materials/Equipment
- Slides: AC-3 worklist trace, backtracking-with-heuristics trace, midterm topic map
- Starter notebook: AC-3 and backtracking-search skeletons

### Formative Check (in-class)
Exercise: run AC-3 by hand on a small 3-variable map-coloring CSP, showing which arcs get revised
and in what order; then apply MRV to choose the next variable for backtracking.

### Link to Lab/Assessment
Lab 8: Implement AC-3 and backtracking search with MRV and least-constraining-value heuristics
for a map-coloring and a small scheduling CSP.
