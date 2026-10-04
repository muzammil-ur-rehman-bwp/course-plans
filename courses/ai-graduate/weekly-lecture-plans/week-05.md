# Week 5 Lecture Plan — Artificial Intelligence (Graduate)
## Topic: Rigorous CSP & Combinatorial Optimization

**Duration:** 2 hours lecture + 3 hour lab/seminar

### Learning Objectives (Bloom's Level)
1. Implement AC-3 and heuristic-ordered backtracking search for CSPs. (*Apply*)
2. Implement simulated annealing and genetic algorithms for a toy combinatorial optimization
   problem. (*Apply*)
3. Discuss, conceptually, the convergence properties of simulated annealing vs. genetic
   algorithms. (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap | CSP basics from the undergraduate course (backtracking, constraint graphs) |
| 0:15–0:50 | Arc consistency & ordering heuristics | AC-3 algorithm walkthrough; MRV/LCV heuristics |
| 0:50–1:25 | Simulated annealing | Metropolis acceptance criterion, cooling schedules, convergence discussion |
| 1:25–1:50 | Genetic algorithms | Selection/crossover/mutation; why no convergence guarantee exists |
| 1:50–2:00 | Synthesis | When to choose exact CSP search vs. metaheuristic optimization |

### Materials/Equipment
- Slides: "Rigorous CSP & Metaheuristic Optimization"
- Live-coding environment (Jupyter)

### Formative Check (in-class)
Trace AC-3 by hand on a 3-variable CSP with a simple inequality constraint graph and identify
which domains are pruned.

### Link to Lab/Assessment
Lab 5: implement AC-3 + heuristic backtracking for a CSP, and simulated annealing for a toy TSP
instance (see `lab-manuals/lab-05.md`).
