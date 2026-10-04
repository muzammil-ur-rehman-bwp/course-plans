# Week 11 Lecture Plan — Knowledge Representation and Reasoning
## Topic: Temporal and Spatial Reasoning

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Enumerate Allen's thirteen interval relations and compute which holds between two given
   intervals. (*Remember, Apply*)
2. Apply constraint propagation (path consistency) over a small temporal constraint network.
   (*Apply, Analyze*)
3. Describe basic topological spatial relations as the spatial analogue of Allen's algebra.
   (*Understand*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:20 | Why intervals? | Limits of point-based time; representing durations and overlaps |
| 0:20–0:55 | Allen's interval algebra | The thirteen base relations, worked examples on a timeline |
| 0:55–1:05 | Break | — |
| 1:05–1:35 | Temporal constraint propagation | Composing relations, path consistency over a small network |
| 1:35–2:00 | Spatial reasoning (conceptual) | Topological relations (disjoint, touches, overlaps, contains); RCC overview |

### Materials/Equipment
- Slides: Allen's 13-relations timeline diagram, composition-table excerpt, spatial-relations
  diagram
- Starter notebook: interval-relation function skeleton

### Formative Check (in-class)
Exercise: given two intervals `(2, 5)` and `(5, 8)`, determine which Allen relation holds; given
3 intervals and 2 known relations, propagate to determine what is implied (or ruled out) about
the third relation.

### Link to Lab/Assessment
Lab 11: Implement Allen's thirteen interval relations and a path-consistency propagator for a
small temporal constraint network.
