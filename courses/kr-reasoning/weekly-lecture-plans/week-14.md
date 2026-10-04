# Week 14 Lecture Plan — Knowledge Representation and Reasoning
## Topic: Knowledge-Based Agents in Practice

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Describe the integrated knowledge-based agent architecture (working memory, rules, a dispatch
   query interface). (*Understand*)
2. Build an integrated forward/backward-chaining reasoner over a toy knowledge base combining
   facts, rules, and a shallow frame-style taxonomy. (*Apply, Create*)
3. Add a derivation trace that justifies each derived fact, and use it to explain an answer.
   (*Apply, Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:25 | Integration overview | Recap Weeks 5 and 6; why a single agent needs both rules and taxonomic facts |
| 0:25–0:55 | A unified reasoner | One `ask(query)` method dispatching to forward or backward chaining |
| 0:55–1:05 | Break | — |
| 1:05–1:35 | Derivation traces | Recording which rule/fact justified each derived fact; why this matters for explainability |
| 1:35–2:00 | Worked queries | Running the integrated agent on a toy diagnostic knowledge base, inspecting traces |

### Materials/Equipment
- Slides: integrated-agent architecture diagram, derivation-trace example
- Starter notebook: integrated reasoner skeleton reusing Week 5/6 code

### Formative Check (in-class)
Exercise: given an integrated KB (facts, rules, and an IS-A taxonomy feeding rule premises), ask
one query by forward chaining and one by backward chaining, and produce a short derivation trace
for each by hand.

### Link to Lab/Assessment
Lab 14: Integrate the Week 5 rule engine and Week 6 frame/inheritance code into one reasoner with
a derivation trace, and query it on a toy domain.
