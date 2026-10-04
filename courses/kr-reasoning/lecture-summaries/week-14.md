# Week 14 Summary — Knowledge-Based Agents in Practice

**Key takeaways:**
- A realistic knowledge-based agent integrates a rule base and a frame/taxonomy hierarchy,
  expanding taxonomic facts into plain facts the rule engine can use as premises.
- A single `ask(query, strategy)` method can dispatch to forward or backward chaining over the
  same unified fact/rule representation.
- A derivation trace recording each derived fact's justification (a given fact, or which rule
  fired) directly supports explainability — one of the three standard axes for evaluating KR
  systems, alongside correctness and scalability.

**You should now be able to:** build an integrated reasoner combining rules and a frame
hierarchy; dispatch queries to forward or backward chaining; attach and read a derivation trace
that explains a query's answer.

**Next week:** current trends — knowledge graphs, neuro-symbolic AI, and evaluating KR systems.
