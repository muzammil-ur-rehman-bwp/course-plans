# Week 2 Lecture Plan — Artificial Intelligence (Graduate)
## Topic: Advanced Search — IDA*, Bidirectional Search, SMA*

**Duration:** 2 hours lecture + 3 hour lab/seminar

### Learning Objectives (Bloom's Level)
1. Explain why plain A*'s O(b^d) memory requirement is a practical bottleneck. (*Understand*)
2. Implement IDA*, and derive its space complexity O(d) vs. A*'s O(b^d). (*Apply, Analyze*)
3. Explain the conditions under which bidirectional search gives a complexity improvement, and
   trace SMA*'s node-forgetting behavior on a small example. (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap + motivation | Why A* runs out of memory before it runs out of time |
| 0:15–0:45 | IDA* | Board-worked algorithm + complexity derivation |
| 0:45–1:15 | Bidirectional search | Diagram-driven explanation; branching-factor-squared speedup argument |
| 1:15–1:45 | SMA* | Worked trace of node-forgetting under a memory bound |
| 1:45–2:00 | Synthesis | Comparative table: A* vs. IDA* vs. bidirectional vs. SMA* |

### Materials/Equipment
- Slides: "Memory-Bounded & Bidirectional Heuristic Search"
- Whiteboard for IDA* threshold-iteration trace and SMA* node-forgetting trace
- Live-coding environment (Jupyter)

### Formative Check (in-class)
Given a search tree with branching factor b=3 and solution depth d=6, estimate A*'s worst-case
node count vs. IDA*'s worst-case memory footprint, and explain the gap.

### Link to Lab/Assessment
Lab 2: implement and benchmark IDA* (see `lab-manuals/lab-02.md`).
