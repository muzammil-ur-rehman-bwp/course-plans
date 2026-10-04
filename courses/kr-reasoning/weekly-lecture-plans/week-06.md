# Week 6 Lecture Plan — Knowledge Representation and Reasoning
## Topic: Semantic Networks and Frames

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Represent taxonomic knowledge as a semantic network with IS-A/part-of links. (*Apply*)
2. Explain why strict property inheritance breaks under exceptions, motivating non-monotonic
   inheritance. (*Analyze*)
3. Build a frame-based reasoner with slots, defaults, and inheritance that correctly overrides a
   default with a more specific value. (*Apply, Create*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:25 | Semantic networks | Nodes, IS-A/part-of links, property inheritance down a hierarchy |
| 0:25–0:55 | The exceptions problem | Tweety/penguin case; why strict inheritance gives the wrong answer |
| 0:55–1:05 | Break | — |
| 1:05–1:35 | Frames | Slots, values, defaults, attached procedures (brief); frame hierarchies |
| 1:35–2:00 | Frame-based inheritance resolution | Resolving a slot by walking up the frame hierarchy, overriding defaults |

### Materials/Equipment
- Slides: semantic-network diagram, Tweety/penguin exception diagram, frame-slot-inheritance trace
- Starter notebook: semantic-network and frame class skeletons

### Formative Check (in-class)
Exercise: given a 4-level IS-A hierarchy (Bird → Penguin → EmperorPenguin) with a `can_fly`
default at the `Bird` level overridden at `Penguin`, trace by hand what `can_fly` resolves to for
an `EmperorPenguin` instance, and why.

### Link to Lab/Assessment
Lab 6: Implement a semantic network with property inheritance and a frame system with slots,
defaults, and inheritance that correctly resolves the Tweety/penguin exceptions case.
