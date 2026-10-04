# Week 5 Lecture Plan — Programming for AI
## Topic: Problem Solving as Search — State Spaces, BFS/DFS

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain how to formulate a problem as a search problem (states, actions, goal test, cost). (*Understand*)
2. Apply Breadth-First Search and Depth-First Search to solve a state-space problem in Python. (*Apply*)
3. Analyze the completeness, optimality, time/space complexity of BFS vs. DFS. (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:25 | Search problem formulation | States, actions, transition model, goal test, path cost |
| 0:25–0:55 | BFS | Algorithm, `deque`-based implementation, trace on a maze example |
| 0:55–1:05 | Break | — |
| 1:05–1:35 | DFS | Algorithm, recursive/stack-based implementation, trace |
| 1:35–2:00 | Comparison | BFS vs. DFS: completeness, optimality, complexity table |

### Materials/Equipment
- Live-coding environment
- Starter notebook: generic `Problem` class skeleton; maze/8-puzzle example

### Formative Check (in-class)
Exercise: trace BFS and DFS by hand on a small graph, then verify with code.

### Link to Lab/Assessment
Lab 5: Implement a generic `Problem` class + BFS/DFS solver for a maze or 8-puzzle.
