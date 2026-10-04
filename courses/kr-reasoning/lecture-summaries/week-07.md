# Week 7 Summary — Description Logics and Ontologies

**Key takeaways:**
- Description logics are decidable fragments of FOL purpose-built for taxonomic knowledge,
  trading expressiveness for guaranteed-terminating reasoning.
- Basic DL constructors (`⊓, ¬, ∃R.C, ∀R.C`) each translate directly to a one-free-variable FOL
  formula; subsumption (`C ⊑ D`) asks whether every instance of `C` is necessarily an instance of
  `D`.
- OWL and RDF are the Semantic Web's standardized realization of DL-style ontologies; real DL
  tooling (Protégé, Pellet) exists in the field but is not required for this course's Python-based
  toy subsumption checker.

**You should now be able to:** read and write basic DL concept/role expressions; translate them
to FOL; implement and interpret a toy, model-based subsumption checker; explain how OWL/RDF relate
to DL and the Semantic Web vision.

**Next week:** constraint satisfaction in depth — AC-3 and heuristic backtracking search, plus
midterm review.
