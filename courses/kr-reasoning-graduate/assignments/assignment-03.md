# Assignment 3 — MLNs, Knowledge Graphs, KG Reasoning, Multi-Agent Epistemics (Weeks 9–12)

**Weight:** 5% of course grade (one of 3 problem sets, 15% total) | **Assigned:** Week 12 |
**Due:** Start of Week 14

## Instructions
Submit a single Jupyter notebook `assignment03.ipynb` answering all questions below. Show your
work (code + a brief written explanation) for each question.

## Questions
1. **(MLN grounding and inference, 25 pts)** For a provided 3-constant, 2-formula MLN
   (`assignment03_mln.json`), ground it by hand (list every ground atom and every formula
   grounding in a markdown cell), then compute its full probability distribution using your
   Week 9 `mln_distribution`. Identify the most probable world, and explain in 2–3 sentences why
   it is most probable given the formulas' weights.
2. **(TransE training and link prediction, 25 pts)** Train a TransE model (Week 10) on a provided
   knowledge graph (`assignment03_kg.json`, at least 8 entities, 2 relations), hold out 2 triples,
   and report each held-out triple's true-tail rank among all candidates. Discuss, in 2–3
   sentences, whether your dimension/margin choices seem to affect the ranking quality.
3. **(Rule mining and signal combination, 20 pts)** On the same knowledge graph, mine a
   closed-path rule (Week 11) and report its support/confidence. Combine the mined rule's
   confidence with your TransE model's scores (using `combine_signals`) to rank 4 candidate facts
   (2 rule-covered, 2 not), and discuss any disagreement between the two signals.
4. **(Multi-agent epistemics, 30 pts)** (a) Simulate the muddy-children puzzle (Week 12) for
   (n=5, k=3) and confirm the returned round equals 3. (b) For a provided 2-agent Kripke model
   where E_G, C_G, and D_G all differ on a given formula φ (`assignment03_epistemic.json`),
   compute each of the three and explain, in 2–3 sentences, the real-world difference in meaning
   between "everyone knows φ," "it is common knowledge that φ," and "the group distributively
   knows φ" on this specific example.

## Submission
Upload `assignment03.ipynb` via the course submission system. Late policy per syllabus
(`course-plan.md` §7).
