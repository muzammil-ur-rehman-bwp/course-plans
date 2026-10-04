# Week 10 Summary — Knowledge Graphs

**Key takeaways:**
- Knowledge graphs generalize semantic networks to web/enterprise scale, serialized as
  (subject, predicate, object) RDF triples.
- TransE embeds entities and relations as vectors so that a valid triple satisfies h + r ≈ t,
  scored as f(h,r,t) = −‖h+r−t‖, and trained with a margin-based ranking loss over positive vs.
  corrupted (negative) triples.
- Link prediction — ranking candidate tails for (h, r, ?) by score — is a statistical-
  generalization reasoning task, distinct from (and without the derivation trail of) symbolic
  inference.

**You should now be able to:** represent a domain as RDF triples; state the TransE scoring
function and margin loss precisely; train a toy TransE model with NumPy and use it for link
prediction.

**Next week:** Reasoning over knowledge graphs — rule mining (support/confidence over graph
paths) and a grounded survey of neuro-symbolic reasoning combining mined rules with embedding
scores.
