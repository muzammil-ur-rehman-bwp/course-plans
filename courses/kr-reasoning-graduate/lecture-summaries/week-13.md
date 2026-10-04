# Week 13 Summary — Ontology Engineering in Practice

**Key takeaways:**
- A practical ontology-development cycle (specification via competency questions,
  conceptualization, formalization, implementation, evaluation, maintenance) scopes and validates
  an ontology against concrete questions it must answer, not an abstract completeness standard.
- Ontology alignment proposes correspondences via lexical, structural, and instance-based
  similarity; every signal has clear failure modes (synonymy defeats lexical matching; homonymy
  defeats it the other way), so proposals need human audit.
- Pellet and HermiT are production tableau-based OWL DL reasoners (scaling Week 4's algorithm);
  Protégé calls them to check consistency/classification and surfaces inconsistencies for the
  engineer to fix.

**You should now be able to:** write and check competency questions; implement and audit a
lexical ontology matcher; describe what Pellet/HermiT check and how Protégé surfaces the results.

**Next week:** Explainability and reasoning — proof trees as explanations in rule-based systems,
contrasted briefly and honestly with black-box ML explainability.
