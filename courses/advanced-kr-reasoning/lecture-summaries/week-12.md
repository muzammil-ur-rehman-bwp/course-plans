# Week 12 Summary — Ontology Evolution and Versioning

**Key takeaways:**
- Syntactic diffing of ontology versions can both over- and under-report real semantic change
  (logically equivalent restatements, interacting new axioms); meaningful diffing needs
  entailment-preservation checks, not just set comparison.
- Impact analysis identifies entailments gained/lost between versions; backward-compatibility
  policy decides whether entailment loss is acceptable and how dependent systems are told.
- Modularity bounds, but does not eliminate, cross-module interaction effects — a partial
  mitigation, not a solved problem.

**You should now be able to:** perform a manual impact analysis across two ontology versions;
implement a simple ontology-diff tool; argue for/against backward-compatibility of a change.

**Next week:** Research methods for advanced KR research — reading/critiquing frontier papers;
problem-statement precision. **Quiz 6** (Weeks 11–12) this week.
