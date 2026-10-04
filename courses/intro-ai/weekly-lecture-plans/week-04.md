# Week 4 Lecture Plan — Introduction to AI
## Topic: Informed Search — Heuristics, A*, Local Search

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain admissibility and consistency of a heuristic function. (*Understand*)
2. Apply A* search to solve a state-space problem given a heuristic. (*Apply*)
3. Analyze why A* with an admissible heuristic is optimal, and compare hill climbing/simulated annealing as local-search alternatives. (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:20 | Heuristics | What a heuristic is; admissibility; consistency; examples (Manhattan distance, straight-line distance) |
| 0:20–0:45 | Greedy Best-First Search | Algorithm and its weakness (ignores path cost so far) |
| 0:45–1:10 | A\* search | `f = g + h`; why admissibility guarantees optimality (brief argument, not a full proof) |
| 1:10–1:20 | Break | — |
| 1:20–1:40 | Hill climbing | Algorithm; local optima, plateaus, ridges as failure modes |
| 1:40–2:00 | Simulated annealing | Temperature schedule; accepting worse moves to escape local optima |

### Materials/Equipment
- Slides: `f = g + h` diagram, hill-climbing landscape diagram
- Starter notebook: grid-world A* skeleton; a 1-D optimization landscape for local search

### Formative Check (in-class)
Exercise: given a small grid, compute `f(n) = g(n) + h(n)` for each frontier node by hand and
decide which node A* expands next.

### Link to Lab/Assessment
Lab 4: Implement A* with a Manhattan-distance heuristic on a grid; implement hill climbing and
simulated annealing for a toy optimization problem.
