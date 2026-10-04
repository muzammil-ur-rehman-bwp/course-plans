# Week 10 — Lecture Content: Knowledge Graphs

## 1. From Semantic Networks to Knowledge Graphs
The undergraduate course's semantic networks represented objects, categories, and relations as a
graph of IS-A/part-of links. A **knowledge graph** is the same basic idea at web/enterprise
scale: a large, heterogeneous graph of entities and typed relations, almost always serialized as
**RDF triples** — (subject, predicate, object), e.g. (Alice, worksFor, AcmeCorp),
(AcmeCorp, locatedIn, Boston). At this scale, a purely symbolic representation becomes hard to
search and generalize from (it cannot answer "what is probably also true" the way it can answer
"what is explicitly stated"), which motivates **embeddings**.

## 2. TransE: Translation-Based Embeddings
TransE (Bordes et al., 2013) embeds every entity e and every relation r as a vector in ℝ^d. The
core modeling intuition: a valid triple (h, r, t) should satisfy **h + r ≈ t** — the relation
vector acts as a *translation* from the head entity's embedding to the tail entity's embedding.

**Scoring function.** f(h, r, t) = −‖h + r − t‖ (L1 or L2 norm), using the *vector* embeddings of
h, r, t. A valid triple should have **small** ‖h + r − t‖, i.e. a score close to 0 (the least
negative, most plausible); an implausible triple should have large ‖h + r − t‖, i.e. a strongly
negative score.

**Training via margin-based ranking loss.** For each observed positive triple (h, r, t), generate
a **corrupted** negative triple (h′, r, t) or (h, r, t′) by replacing the head or tail with a
random entity (the entity vocabulary minus the true one, typically). Minimize:
```
L = Σ_{(h,r,t)∈S, (h',r,t')∈S'_{(h,r,t)}} max(0, γ + d(h+r,t) − d(h'+r,t'))
```
where d(·) = ‖·‖ is the same norm used in the scoring function and γ > 0 is a margin
hyperparameter. This pushes the positive triple's distance below the negative triple's distance
by at least γ; triples already separated by more than γ contribute zero loss (the hinge).

```python
import numpy as np

def init_embeddings(entities, relations, dim, rng):
    E = {e: rng.normal(0, 1, dim) for e in entities}
    R = {r: rng.normal(0, 1, dim) for r in relations}
    for d in (E, R):
        for k in d:
            d[k] /= np.linalg.norm(d[k])  # TransE conventionally keeps embeddings unit-normalized
    return E, R

def score(E, R, h, r, t):
    return -np.linalg.norm(E[h] + R[r] - E[t])  # higher (less negative) = more plausible

def corrupt(triple, entities, rng):
    h, r, t = triple
    if rng.random() < 0.5:
        h = rng.choice([e for e in entities if e != h])
    else:
        t = rng.choice([e for e in entities if e != t])
    return (h, r, t)

def train_transe(triples, entities, relations, dim=8, margin=1.0, lr=0.01, epochs=200, seed=0):
    rng = np.random.default_rng(seed)
    E, R = init_embeddings(entities, relations, dim, rng)
    for epoch in range(epochs):
        for (h, r, t) in triples:
            h2, r2, t2 = corrupt((h, r, t), entities, rng)
            pos_vec = E[h] + R[r] - E[t]
            neg_vec = E[h2] + R[r2] - E[t2]
            pos_d, neg_d = np.linalg.norm(pos_vec), np.linalg.norm(neg_vec)
            hinge = margin + pos_d - neg_d
            if hinge > 0:  # gradient step only where the hinge loss is active
                grad_pos = pos_vec / (pos_d + 1e-9)
                grad_neg = neg_vec / (neg_d + 1e-9)
                E[h] -= lr * grad_pos; E[t] += lr * grad_pos; R[r] -= lr * grad_pos
                E[h2] += lr * grad_neg; E[t2] -= lr * grad_neg; R[r2] += lr * grad_neg
                for k in (h, t, h2, t2):
                    E[k] /= np.linalg.norm(E[k]) + 1e-9  # renormalize to the unit sphere
    return E, R
```

## 3. Link Prediction
Given a partial triple (h, r, ?), score every candidate entity t′ as f(h, r, t′) and rank; the
top-ranked entities are the model's predicted missing facts — this *is* a reasoning task: filling
in plausible relational facts that were never explicitly stated.

```python
def predict_tail(E, R, h, r, candidates):
    return sorted(candidates, key=lambda t: -score(E, R, h, r, t))
```

## 4. Why This Matters as a Reasoning Task
A symbolic triple store can only answer queries over facts it was explicitly told. TransE's
vector space generalizes smoothly: entities and relations with similar roles land near each
other, so a never-stated-but-plausible triple can still score well. The cost: there is no
derivation to point to (contrast this with Week 14's proof trees) — a predicted link is a
statistical generalization, not a logical consequence, and must be read as such.

## 5. In-Class/Lab Exercise
Build a toy knowledge graph with 5–6 entities and 2 relation types (e.g., `worksFor`,
`locatedIn`), train `train_transe` on it, and use `predict_tail` to predict a held-out triple's
missing tail entity — check whether the true answer ranks near the top, and discuss what a wrong
top prediction would mean (a case where the embedding's generalization does not match the
graph's actual, unstated structure).
