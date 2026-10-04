# Lab Notes 13 — Lexical Ontology Matcher

**Concept recap:** lexical/token-based similarity is one of three alignment signals (alongside
structural and instance-based); it catches shared-wording correspondences but misses synonymy
and can false-positive on homonymy.

**Common pitfalls:**
- **Tokenization missing CamelCase boundaries**: `tokenize`'s regex-based split must correctly
  separate `ResearchPaper` into `{research, paper}` — a matcher that fails to split CamelCase
  will treat multi-word class names as a single opaque token, silently tanking every similarity
  score for such classes.
- **Treating the matcher's threshold as ground truth**: `propose_alignments`'s `threshold`
  parameter is a tuning knob, not a correctness guarantee — Task B explicitly expects some
  proposed correspondences at or above threshold to still be audited as incorrect or ambiguous.
- **Accepting every high-scoring proposal uncritically**: a perfect lexical match (e.g., both
  ontologies using `Account`) is not automatically a correct correspondence if the two classes
  mean different things in context — Task C's reverse case (homonymy) is worth constructing even
  though the manual only explicitly asks for the synonymy case.
- In Task D, writing competency questions too vague to check against a hierarchy (e.g., "what
  can we know about students?") — a good competency question is specific enough that "yes, this
  hierarchy can answer it" or "no, it cannot" is a clear yes/no.

**Debugging tip:** print each candidate pair's token sets (not just the final Jaccard score)
when auditing Task A's output — seeing exactly which tokens matched or failed to match makes it
obvious why a correspondence was proposed or missed.

**Instructor tip:** have students bring in two real, small class-naming lists from unrelated
sources (e.g., two different public datasets' schema field names) for Task C — manufactured
synonymy examples are easy to solve "by eye" in advance, but real independently-authored
vocabularies produce much more convincing misses.
