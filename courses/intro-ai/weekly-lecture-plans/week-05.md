# Week 5 Lecture Plan — Introduction to AI
## Topic: Adversarial Search — Minimax, Alpha-Beta Pruning, Game Playing

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain how a two-player zero-sum game is formulated as a search problem. (*Understand*)
2. Apply the minimax algorithm to choose a move in a small game tree. (*Apply*)
3. Analyze how alpha-beta pruning reduces the number of nodes explored without changing the result. (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:20 | Games as search | Game tree, terminal states, utility values, the MAX/MIN perspective |
| 0:20–0:50 | Minimax | Algorithm, recursive implementation, trace on a small game tree |
| 0:50–1:00 | Break | — |
| 1:00–1:35 | Alpha-beta pruning | Alpha/beta bounds; pruning rule; trace showing branches cut vs. plain minimax |
| 1:35–2:00 | Tic-Tac-Toe case study | Full game tree size discussion; evaluation functions for non-terminal states in larger games (brief) |

### Materials/Equipment
- Slides: game-tree diagrams, alpha-beta pruning trace
- Starter notebook: Tic-Tac-Toe board representation and move generator

### Formative Check (in-class)
Exercise: trace minimax and then alpha-beta pruning on the same 3-ply game tree; count nodes
visited in each case.

### Link to Lab/Assessment
Lab 5: Implement minimax and alpha-beta pruning for Tic-Tac-Toe; compare nodes explored.
