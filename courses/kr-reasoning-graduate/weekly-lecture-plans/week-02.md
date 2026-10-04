# Week 2 Lecture Plan — Knowledge Representation and Reasoning (Graduate)
## Topic: Modal Logic

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. State the Kripke-model satisfaction clauses for □ and ◇. (*Understand*)
2. Evaluate a modal formula against a given finite Kripke model by hand and in code. (*Apply*)
3. Determine which modal system (K, T, S4, S5) a given accessibility relation validates, from its
   properties (reflexivity, transitivity, symmetry). (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap | Why propositional/FOL logic alone cannot express "necessarily" or "an agent knows" |
| 0:15–0:45 | Kripke semantics | Frames, models, the □/◇ satisfaction clauses, worked evaluation on a 4-world model |
| 0:45–1:15 | Correspondence theory | Reflexivity → T, transitivity → 4, S4, equivalence relation → S5; board-worked derivations of why each axiom follows from the property |
| 1:15–1:25 | Break | — |
| 1:25–1:50 | Epistemic reading | □ as K_i (agent i knows); why S5 is the standard logic of knowledge |
| 1:50–2:00 | Synthesis | Where modal logic resurfaces this semester (temporal logic Week 3, epistemic multi-agent logic Week 12) |

### Materials/Equipment
- Slides: "Kripke Semantics and the Modal Systems"
- Whiteboard for correspondence-theory derivations
- Live-coding environment (Jupyter)

### Formative Check (in-class)
Given a 4-world Kripke model and an accessibility relation, evaluate □p, ◇q, and □◇p by hand at a
named world, then state whether the relation is reflexive/transitive/symmetric.

### Link to Lab/Assessment
Lab 2: implement a Kripke-model satisfaction checker and verify S4/S5 axioms hold under the
corresponding accessibility-relation properties (see `lab-manuals/lab-02.md`).
