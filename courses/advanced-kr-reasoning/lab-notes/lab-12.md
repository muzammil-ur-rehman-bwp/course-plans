# Lab Notes 12 — Ontology Diff Tool

**Concept recap:** a syntactic axiom diff and a semantic entailment diff are different things;
`ontology_diff` reports both, since a "fix" can change entailments on queries that look
unrelated to the syntactically changed axioms once interactions are accounted for.

**Common pitfalls:**
- Designing Task B's two versions so that every entailment change is on a query that was
  *obviously* going to change (e.g., directly mentioning the modified axiom) — a stronger
  exercise includes at least one query whose entailment status changes *indirectly*, through
  interaction with an axiom the student did not expect to matter, which is closer to the real
  engineering risk this week's material is about.
- Forgetting that `ontology_diff`'s query list must be fixed and identical across both calls to
  `entailment_checker` — comparing entailment on different query sets between versions makes the
  "changed_queries" result meaningless.
- In Task D, declaring a change "backward-compatible" solely because most entailments were
  preserved, without checking whether *any* entailment was lost — strict backward compatibility
  (as defined in the lecture content) requires preserving *every* old entailment; losing even one
  makes it a non-backward-compatible (if possibly still justified) change.

**Debugging tip:** before running the full diff, manually predict which of your 4–5 queries you
expect to change and why; a mismatch between your prediction and the tool's output is the
fastest way to catch either a bug in your toy ontology or a genuine, instructive surprise.

**Instructor tip:** have students swap Task B ontologies with a partner and independently
perform Task D's backward-compatibility argument on their partner's example — a partner's honest
assessment of the "surprise" entailment change (if present) is a good proxy for how a real
dependent-system engineer would react to it.
