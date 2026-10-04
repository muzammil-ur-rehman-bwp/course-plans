# Week 11 Summary — Reasoning over Knowledge Graphs

**Key takeaways:**
- Closed-path rule mining scores a candidate rule by support (confirmed firings) and confidence
  (confirmed firings ÷ all firings), mirroring the idea behind systems such as AMIE.
- A grounded neuro-symbolic pattern uses mined/hand-written rules for exact, explainable coverage
  and TransE scores to rank or filter candidates rules leave ambiguous or do not cover at all.
- Rules and embeddings have complementary strengths and weaknesses: rules are exact and
  explainable but brittle to noise; embeddings generalize smoothly but give no explanation and
  can be confidently wrong.

**You should now be able to:** compute support/confidence for a candidate rule over a toy
knowledge graph; combine mined-rule and embedding signals to rank candidate facts; articulate
what each signal contributes and lacks.

**Next week:** Multi-agent epistemic reasoning — common knowledge, distributed knowledge, and
the muddy-children puzzle, contrasted with the game-theoretic multi-agent-systems treatment in
*Artificial Intelligence*, Graduate.
