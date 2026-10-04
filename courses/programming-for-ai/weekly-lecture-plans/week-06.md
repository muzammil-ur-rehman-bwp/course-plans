# Week 6 Lecture Plan — Programming for AI
## Topic: Informed Search — Greedy Best-First & A*

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain what a heuristic function is and what admissibility/consistency mean. (*Understand*)
2. Apply Greedy Best-First Search and A* using a priority queue (`heapq`). (*Apply*)
3. Analyze how heuristic quality affects the number of nodes expanded. (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:25 | Heuristics | Definition, admissibility, consistency, examples (Manhattan/Euclidean distance) |
| 0:25–0:55 | Greedy Best-First Search | Algorithm + live-coded implementation |
| 0:55–1:05 | Break | — |
| 1:05–1:40 | A* search | Algorithm (f = g + h), `heapq`-based implementation, trace on grid example |
| 1:40–2:00 | Comparing strategies | Nodes expanded: BFS vs. Greedy vs. A* on the same grid |

### Materials/Equipment
- Live-coding environment
- Starter notebook: grid-world with obstacles

### Formative Check (in-class)
Exercise: design a heuristic for a given grid problem and argue whether it is admissible.

### Link to Lab/Assessment
Lab 6: Implement A* on a grid/puzzle with a custom heuristic. **Assignment 2** assigned this
week (search algorithms), due start of Week 8.
