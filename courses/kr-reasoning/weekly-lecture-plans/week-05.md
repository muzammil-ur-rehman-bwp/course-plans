# Week 5 Lecture Plan — Knowledge Representation and Reasoning
## Topic: Rule-Based Systems

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Describe the production-system architecture (working memory, rule base, recognize-act cycle).
   (*Understand*)
2. Apply forward chaining and backward chaining over a general rule-base representation.
   (*Apply*)
3. Design and build a reusable Python rule engine decoupled from any single domain. (*Create*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:25 | Production systems | Working memory, condition-action rules, the recognize-act cycle, conflict resolution (brief) |
| 0:25–0:55 | Forward chaining | Data-driven inference; match-resolve-act traced on a general rule base |
| 0:55–1:05 | Break | — |
| 1:05–1:35 | Backward chaining | Goal-directed, recursive reasoning over the same rule base; comparison with forward chaining |
| 1:35–2:00 | Engine design | Decoupling the engine from any one domain; swapping rule bases without changing engine code |

### Materials/Equipment
- Slides: production-system cycle diagram, forward vs. backward chaining trace diagrams
- Starter notebook: `Rule` class skeleton, two example toy rule bases

### Formative Check (in-class)
Exercise: given a 6-rule knowledge base, trace forward chaining to derive everything entailed by
a given fact set, then trace backward chaining to answer one specific query, and confirm both
reach the same answer.

### Link to Lab/Assessment
Lab 5: Build a general-purpose Python rule engine supporting both forward and backward chaining,
and apply it unchanged to two different toy domains.
