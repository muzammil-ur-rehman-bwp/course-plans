# Week 14 — Lecture Content: Explainability and Reasoning

## 1. From a Single Trace Step to a Full Justification
The undergraduate course's integrated agent (its Week 14) recorded which single rule/fact
justified each derived fact — a one-level trace. This week builds the **full recursive proof
tree**: every internal node is a rule application, its children are the facts/sub-conclusions
that satisfied that rule's premises, down to leaf facts stated outright in the knowledge base.

## 2. Proof Trees
```python
class ProofNode:
    def __init__(self, fact, rule=None, children=None):
        self.fact = fact
        self.rule = rule            # None for a leaf (asserted) fact
        self.children = children or []

    def render(self, indent=0):
        pad = "  " * indent
        if self.rule is None:
            print(f"{pad}{self.fact}  [given fact]")
        else:
            print(f"{pad}{self.fact}  [by rule: {self.rule}]")
            for c in self.children:
                c.render(indent + 1)

def forward_chain_with_proof(facts, rules):
    """facts: set of fact strings. rules: list of (name, premises list, conclusion).
    Returns (derived_facts, proof_of: dict fact -> ProofNode)."""
    derived = set(facts)
    proof_of = {f: ProofNode(f) for f in facts}
    changed = True
    while changed:
        changed = False
        for name, premises, conclusion in rules:
            if conclusion not in derived and all(p in derived for p in premises):
                derived.add(conclusion)
                proof_of[conclusion] = ProofNode(
                    conclusion, rule=name, children=[proof_of[p] for p in premises])
                changed = True
    return derived, proof_of
```

**Worked example.** Facts: {Bird(tweety), HasWings(tweety)}. Rules: R1: HasWings(x) →
CanFlap(x); R2: Bird(x) ∧ CanFlap(x) → Flies(x). Deriving Flies(tweety) builds the tree:
```
Flies(tweety)  [by rule: R2]
  Bird(tweety)  [given fact]
  CanFlap(tweety)  [by rule: R1]
    HasWings(tweety)  [given fact]
```
This is an **exact, complete, human-checkable record**: every step is a specific rule applied to
specific, already-justified premises — nothing in the explanation is approximate or missing.

## 3. "Why Not" Explanations
When a query fails to derive, the useful explanation is not "no" but *which premise, of which
rule, blocked it*:

```python
def why_not(query, facts, rules):
    """Returns the first rule that could have derived `query` and the specific premise(s)
    missing from `facts` that blocked it, or None if no rule has `query` as its conclusion."""
    for name, premises, conclusion in rules:
        if conclusion == query:
            missing = [p for p in premises if p not in facts]
            if missing:
                return name, missing
            # if nothing is missing, the query should already be derivable — not actually blocked
    return None
```

**Worked example.** Query Flies(polly), facts = {Bird(polly)} (no HasWings(polly)). `why_not`
reports: rule R2 needs CanFlap(polly), which is not in facts; tracing further, CanFlap(polly)
itself depends on rule R1's premise HasWings(polly), which is the actual missing leaf fact — a
full why-not trace should recurse through missing premises the same way the proof tree recurses
through satisfied ones, reporting the leaf-level missing fact(s), not just the first-level gap.

## 4. Symbolic Explainability vs. Black-Box ML Explainability
A proof tree is not an *approximation* of why the system concluded Flies(tweety) — it **is** the
derivation, literally the computation that produced the answer, so it is guaranteed faithful by
construction. Contrast this with explaining a black-box ML model's prediction: a post-hoc
method (e.g., perturbing input features and observing the output's sensitivity) only
**approximates** the model's local decision boundary around one input — it is not guaranteed to
match what the model actually computed internally, and can be misleading precisely where the
model's true decision surface is non-smooth. This is a deliberately brief, grounded contrast —
it states the one structural fact that matters (derivation-as-explanation vs.
approximation-of-a-black-box) without attempting to teach any specific ML explainability method,
which belongs to the machine-learning-family courses.

| | Symbolic (proof tree) | Post-hoc ML explanation |
|---|---|---|
| What it reports | The actual inference steps taken | An approximation of local model behavior |
| Faithfulness guarantee | Exact, by construction | Not guaranteed; can mismatch true behavior |
| Cost | Requires an explicit rule-based/logical model | Works on any model, including ones with no explicit reasoning structure |

## 5. In-Class/Lab Exercise
Extend `why_not` to recurse through a missing premise's own rule (if it has one) until it
reaches a genuinely missing leaf fact, reproducing the full Flies(polly) trace from §3 by hand
and in code. Then write a 4–6 sentence paragraph contrasting this proof-tree explanation with
what a feature-perturbation-based ML explanation would and would not guarantee on an analogous
black-box classifier making the same "flies/doesn't fly" decision.
