# Week 3 Lecture Plan — Introduction to AI
## Topic: Uninformed Search — State Spaces, BFS, DFS, UCS

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Formulate a problem as a search problem (states, actions, transition model, goal test, path cost). (*Understand*)
2. Apply BFS, DFS, and uniform-cost search to solve a state-space problem in Python. (*Apply*)
3. Analyze the completeness, optimality, and time/space complexity of BFS, DFS, and UCS. (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:25 | Search problem formulation | States, actions, transition model, goal test, path cost; tree search vs. graph search |
| 0:25–0:50 | BFS | Algorithm, `deque`-based implementation, trace on a maze example |
| 0:50–1:00 | Break | — |
| 1:00–1:25 | DFS | Algorithm, recursive/stack-based implementation, trace |
| 1:25–1:45 | Uniform-Cost Search | Priority queue by path cost; relation to Dijkstra's algorithm |
| 1:45–2:00 | Comparison | BFS vs. DFS vs. UCS: completeness, optimality, complexity table |

### Materials/Equipment
- Live-coding environment
- Starter notebook: generic `Problem` class skeleton; maze example

### Formative Check (in-class)
Exercise: trace BFS, DFS, and UCS by hand on a small weighted graph, then verify with code.

### Link to Lab/Assessment
Lab 3: Implement a generic `Problem` class and BFS/DFS/UCS solvers for a maze or graph.
