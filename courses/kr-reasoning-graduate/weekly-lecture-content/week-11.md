# Week 11 — Lecture Content: Reasoning over Knowledge Graphs

## 1. Two Ways to Generalize Beyond Stated Facts
Week 10's TransE predicts missing links statistically, with no explanation. This week adds a
complementary, *symbolic* way to generalize — **rule mining** — and then asks how the two signals
can be combined without overselling either one.

## 2. Rule Mining: Support and Confidence
A **closed-path rule** has the form `body ⇒ head`, where body is a conjunction of relation atoms
chained through shared variables, e.g.
```
worksFor(X,Y) ∧ locatedIn(Y,Z) ⇒ basedIn(X,Z)
```
Mirroring the idea behind real systems such as AMIE (Galárraga et al., 2013), a candidate rule is
scored over the graph by:
- **Support**: the number of variable bindings for which both the body *and* the head hold in
  the graph (the rule's correct, confirmed firings).
- **Confidence**: support divided by the number of bindings for which the body holds at all
  (correct firings ÷ all firings) — how often the rule's conclusion is actually right when its
  premise applies.

```python
def rule_support_confidence(triples, body_pattern, head_pattern):
    """triples: set of (s, p, o). *_pattern: list of (var_slot, relation, var_slot) with
    variable names as strings. Returns (support, confidence, body_count)."""
    by_relation = {}
    for s, p, o in triples:
        by_relation.setdefault(p, []).append((s, o))

    def match_body(pattern, bindings_so_far):
        if not pattern:
            yield bindings_so_far
            return
        (s_var, rel, o_var), rest = pattern[0], pattern[1:]
        for s, o in by_relation.get(rel, []):
            new_bindings = dict(bindings_so_far)
            ok = True
            for var, val in ((s_var, s), (o_var, o)):
                if var in new_bindings and new_bindings[var] != val:
                    ok = False
                    break
                new_bindings[var] = val
            if ok:
                yield from match_body(rest, new_bindings)

    body_count, support = 0, 0
    for bindings in match_body(body_pattern, {}):
        body_count += 1
        h_s_var, h_rel, h_o_var = head_pattern
        head_triple = (bindings[h_s_var], h_rel, bindings[h_o_var])
        if head_triple in triples:
            support += 1
    confidence = support / body_count if body_count else 0.0
    return support, confidence, body_count
```

## 3. A Grounded Survey of Neuro-Symbolic Reasoning over Knowledge Graphs
"Neuro-symbolic" covers many designs; this course treats it at a survey level with one concrete,
implementable pattern: use mined (or hand-written) rules to propose high-confidence candidate
facts, and use TransE scores (Week 10) to **rank or filter** candidates a purely symbolic miner
leaves ambiguous (e.g., several rules fire with middling confidence, or no rule covers a
candidate at all but embeddings still rank it plausible). Being explicit about what each side
contributes avoids overselling either:
- **Rules** give exact, traceable coverage exactly where they apply (a fired rule *is* its own
  explanation) but are brittle to graph noise/incompleteness and give no signal where no rule
  matches.
- **Embeddings** generalize smoothly and tolerate noise, but give no explanation for a prediction
  and can be confidently wrong (a high score is not a guarantee).

```python
def combine_signals(candidate_facts, rule_confidences, E, R, score_fn):
    """candidate_facts: list of (h, r, t). rule_confidences: dict mapping a candidate fact to
    the confidence of whichever mined rule produced it (0 if none). score_fn: TransE score
    function from Week 10. Returns candidates ranked by a simple weighted combination."""
    ranked = []
    for (h, r, t) in candidate_facts:
        rule_signal = rule_confidences.get((h, r, t), 0.0)
        embed_signal = score_fn(E, R, h, r, t)
        ranked.append(((h, r, t), rule_signal, embed_signal))
    return sorted(ranked, key=lambda x: (-x[1], -x[2]))  # prioritize rule confidence, then embedding score
```

## 4. Where the Two Signals Agree or Disagree
- **Agreement**: a candidate fact supported by a high-confidence mined rule also scores well
  under TransE — the strongest evidence a system can have, since two independent mechanisms
  concur.
- **Rule-only**: a rule fires with high confidence but the candidate's entities are rare/noisy in
  training, so TransE's embedding for them is poorly learned and scores it low — here the
  symbolic signal should usually be trusted more, since it is derived directly from confirmed
  graph structure.
- **Embedding-only**: no mined rule covers the candidate, but TransE ranks it highly because the
  entities/relations resemble other well-supported patterns — this is exactly the generalization
  rules cannot offer, but it comes with no explanation and should be flagged as lower-confidence
  in any system that reports results to a user.

## 5. In-Class/Lab Exercise
On a toy graph with `worksFor` and `locatedIn` facts (and a few `basedIn` facts confirming the
rule from §2), compute the rule's support and confidence with `rule_support_confidence`. Then
take 2–3 candidate `basedIn` facts the rule does *not* cover, score them with Week 10's trained
TransE model, and discuss which candidates `combine_signals` would rank highest and why.
