# Week 7 Lecture Plan — Advanced Artificial Intelligence (Post Graduate)
## Topic: Algorithmic Game Theory II — Mechanism Design

**Duration:** 2 hours lecture + 3 hour research seminar/lab

### Learning Objectives (Bloom's Level)
1. State the mechanism-design problem: choosing allocation and payment rules to align
   self-interested agents' incentives with a social objective. (*Understand*)
2. Derive the VCG payment rule and prove its dominant-strategy truthfulness. (*Apply, Analyze*)
3. Evaluate how VCG-style ideas appear in ad auctions and resource allocation. (*Evaluate*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap + motivation | From equilibrium computation (Week 6) to designing the game itself |
| 0:15–0:30 | Second-price auction | Recap (graduate course gave this conceptually); now derive truthfulness formally |
| 0:30–1:10 | VCG mechanism | General definition + full dominant-strategy truthfulness proof |
| 1:10–1:40 | Applications | Ad auctions, resource allocation; where practice departs from exact VCG and why |
| 1:40–2:00 | Synthesis | Recap: allocation rule + payment rule = mechanism; truthfulness as a design goal |

### Materials/Equipment
- Slides: "Mechanism Design & the VCG Mechanism"
- Whiteboard for the truthfulness proof
- Live-coding environment (Jupyter)

### Formative Check (in-class)
Walk through, on a 3-bidder single-item example, why a bidder's utility under VCG decomposes
into a term the mechanism maximizes using the bidder's own true value plus a term that does not
depend on the bidder's report.

### Link to Lab/Assessment
Lab 7: implement a VCG mechanism and empirically verify truthfulness (see
`lab-manuals/lab-07.md`). **Assignment 2 assigned** (Weeks 5–7).
