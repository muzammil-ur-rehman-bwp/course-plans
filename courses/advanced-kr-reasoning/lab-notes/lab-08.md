# Lab Notes 8 — Embedding-Plus-Constraint Candidate Filtering

**Concept recap:** embedding-based ranking proposes candidates by learned similarity alone, with
no guarantee of logical consistency; explicit hard (or Week 7's soft) constraints filter/veto
candidates that violate known structure — rules give exactness, embeddings give generalization,
neither alone is enough.

**Common pitfalls:**
- Writing a type constraint that silently passes every candidate because `entity_types.get(h)`
  returns `None` for an untyped entity and the comparison against the type set incorrectly
  evaluates as vacuously true — explicitly test the constraint against a deliberately mistyped
  or untyped entity before trusting it on real candidates.
- Applying constraints in a way that only filters the *displayed* top-k candidates rather than
  the full candidate list, making it look like "nothing got vetoed" when in fact only already-safe
  candidates were ever inspected — apply constraints to the full ranked list, then display the
  top-k of the *survivors*, not the other way around.
- In Task D, treating a vetoed high-confidence candidate as simply "a bug in the embedding model"
  rather than connecting it to the lecture's actual point — a high TransE score reflects
  similarity in the learned vector space, not logical correctness; the mismatch is expected
  behavior for this class of model, not an implementation error.

**Debugging tip:** before filtering real TransE output, run `filter_candidates` on a small,
fully hand-constructed candidate list where you already know by inspection which candidates
should survive — confirms the filtering logic itself before real model noise is introduced.

**Instructor tip:** ask students to report the *score* of the highest-confidence vetoed candidate
specifically — a surprisingly high score makes the "confidently wrong" point far more vivid than
an abstract discussion would.
