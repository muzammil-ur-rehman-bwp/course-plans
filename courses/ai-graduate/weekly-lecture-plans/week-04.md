# Week 4 Lecture Plan — Artificial Intelligence (Graduate)
## Topic: Game Theory II & Multi-Agent Systems

**Duration:** 2 hours lecture + 3 hour lab/seminar

### Learning Objectives (Bloom's Level)
1. Classify a multi-agent scenario as cooperative/competitive and zero-sum/general-sum.
   (*Analyze*)
2. Explain, conceptually, how mechanism design and auction theory align self-interested agents'
   incentives with a desired system-level outcome. (*Understand, Evaluate*)
3. Implement a small simulation of agents bidding in a toy auction and discuss the resulting
   equilibrium behavior. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap | Normal-form games and Nash equilibrium refresher |
| 0:15–0:45 | Cooperative vs. competitive MAS | Taxonomy, worked examples, coordination problems (brief) |
| 0:45–1:25 | Mechanism design & auctions | First-price vs. second-price (Vickrey) auctions; truthfulness argument |
| 1:25–1:50 | Case discussion | Where multi-agent systems and mechanism design show up in real AI systems (ad auctions, resource allocation) |
| 1:50–2:00 | Synthesis | Transition: how Weeks 3–4's game theory connects to Week 14's multi-agent RL |

### Materials/Equipment
- Slides: "Multi-Agent Systems & Mechanism Design"
- Live-coding environment (Jupyter) for the auction simulation

### Formative Check (in-class)
Given a toy second-price auction scenario, verify that bidding one's true value is a dominant
strategy by comparing payoffs from under-bidding and over-bidding.

### Link to Lab/Assessment
Lab 4: simulate competing bidding agents in first-price and second-price auctions
(see `lab-manuals/lab-04.md`). **Assignment 1 due this week** (see `assignments/assignment-01.md`).
