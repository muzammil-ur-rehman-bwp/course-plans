# Week 10 Lecture Plan — Advanced Artificial Intelligence (Post Graduate)
## Topic: Interpretability Research

**Duration:** 2 hours lecture + 3 hour research seminar/lab

### Learning Objectives (Bloom's Level)
1. Describe feature-attribution methods conceptually. (*Understand*)
2. Describe mechanistic interpretability as a research program distinct from post-hoc
   explanation. (*Understand, Analyze*)
3. Critically evaluate an attribution method against a sanity-check standard, and articulate the
   post-hoc/mechanistic gap. (*Evaluate*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap + motivation | From "interpretability as a safety tool" (Week 9) to what interpretability actually offers today |
| 0:15–0:45 | Feature attribution | Saliency, permutation importance, perturbation-based (LIME/SHAP-family) methods, conceptually |
| 0:45–1:20 | Mechanistic interpretability | Reverse-engineering circuits/features vs. characterizing input-output behavior |
| 1:20–1:50 | The post-hoc/understanding gap | Sanity-check findings; what a failed sanity check does and does not imply |
| 1:50–2:00 | Synthesis | Recap table: explanation type vs. what it does and does not guarantee |

### Materials/Equipment
- Slides: "Interpretability Research: Attribution vs. Mechanism"
- Live-coding environment (Jupyter)

### Formative Check (in-class)
Explain why an attribution map that looks visually plausible can still fail a parameter-
randomization sanity check, and what that failure does and does not tell us about the model.

### Link to Lab/Assessment
Lab 10: implement permutation importance and run a sanity check against it (see
`lab-manuals/lab-10.md`).
