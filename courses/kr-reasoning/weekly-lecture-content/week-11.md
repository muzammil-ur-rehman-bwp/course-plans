# Week 11 — Lecture Content: Temporal and Spatial Reasoning

## 1. Why Intervals, Not Just Points?
Representing time as a single timestamp per fact ("meeting at 3pm") cannot express durations or
how two events relate ("the meeting overlapped the fire drill"). **Allen's interval algebra**
(Allen, 1983) represents events as intervals `(start, end)` with `start < end`, and defines
exactly **thirteen** mutually exclusive, jointly exhaustive relations that can hold between any
two intervals — any two intervals stand in exactly one of these thirteen relations.

## 2. The Thirteen Relations
Given intervals `X = (x1, x2)` and `Y = (y1, y2)`:

| Relation | Condition | Inverse |
|---|---|---|
| `X before Y` | `x2 < y1` | `after` |
| `X meets Y` | `x2 == y1` | `met-by` |
| `X overlaps Y` | `x1 < y1 < x2 < y2` | `overlapped-by` |
| `X starts Y` | `x1 == y1 and x2 < y2` | `started-by` |
| `X during Y` | `y1 < x1 and x2 < y2` | `contains` |
| `X finishes Y` | `x2 == y2 and x1 > y1` | `finished-by` |
| `X equals Y` | `x1 == y1 and x2 == y2` | `equals` (self-inverse) |

(The remaining six — `after`, `met-by`, `overlapped-by`, `started-by`, `contains`,
`finished-by` — are exactly the inverses listed above, obtained by swapping the roles of `X` and
`Y`.)

```python
def allen_relation(x, y):
    x1, x2 = x
    y1, y2 = y
    if x2 < y1:
        return "before"
    if x2 == y1:
        return "meets"
    if x1 < y1 < x2 < y2:
        return "overlaps"
    if x1 == y1 and x2 < y2:
        return "starts"
    if y1 < x1 and x2 < y2:
        return "during"
    if x2 == y2 and x1 > y1:
        return "finishes"
    if x1 == y1 and x2 == y2:
        return "equals"
    if x1 > y2:
        return "after"
    if x1 == y2:
        return "met-by"
    if y1 < x1 < y2 < x2:
        return "overlapped-by"
    if x1 == y1 and x2 > y2:
        return "started-by"
    if x1 < y1 and y2 < x2:
        return "contains"
    if x2 == y2 and x1 < y1:
        return "finished-by"
    raise ValueError("no Allen relation matched -- check interval validity (start < end)")
```

### Worked Example
`X = (2, 5)`, `Y = (5, 8)`: `x2 == y1` (5 == 5), so `allen_relation(X, Y)` returns `"meets"` — `X`
ends exactly when `Y` begins, with no gap and no overlap.

## 3. Composing Relations and Path Consistency
Given relations `X R1 Y` and `Y R2 Z`, a **composition table** (a fixed lookup table over all
13×13 relation pairs — provided as reference material, not derived in this course) gives the set
of relations that could possibly hold between `X` and `Z`. **Path consistency** uses this to
propagate constraints over a small network of intervals with partially known relations, exactly
analogous to Week 8's arc consistency for CSPs: for every triple `(X, Y, Z)`, intersect the
currently-allowed relations between `X` and `Z` with what composing `X`-`Y` and `Y`-`Z` permits,
repeating until a fixed point.

```python
def propagate_path_consistency(intervals, possible_relations, composition_table):
    """possible_relations[(X, Y)] = set of Allen relations still considered possible between
    X and Y. Repeatedly tighten using composed constraints until no further change."""
    changed = True
    while changed:
        changed = False
        for x in intervals:
            for y in intervals:
                for z in intervals:
                    if x == y or y == z or x == z:
                        continue
                    composed = set()
                    for r1 in possible_relations.get((x, y), set()):
                        for r2 in possible_relations.get((y, z), set()):
                            composed |= composition_table.get((r1, r2), set())
                    current = possible_relations.get((x, z))
                    if current is not None:
                        tightened = current & composed if composed else current
                        if tightened != current:
                            possible_relations[(x, z)] = tightened
                            changed = True
    return possible_relations
```

If propagation ever tightens some `possible_relations[(x, z)]` to the empty set, the network is
**inconsistent** — no assignment of concrete intervals can satisfy every stated relation
simultaneously, exactly mirroring an AC-3 domain going empty in Week 8.

## 4. Basic Spatial Relations (Conceptual)
The spatial analogue of Allen's algebra is a set of **topological relations** between regions:
`disjoint` (no shared points), `touches` (share only a boundary), `overlaps` (share some interior
points but neither contains the other), and `contains`/`inside` (one region's interior wholly
contains the other). The **Region Connection Calculus (RCC)**, most commonly its 8-relation
fragment RCC-8, formalizes exactly this set of mutually exclusive, jointly exhaustive spatial
relations — the direct spatial parallel to Allen's 13 temporal relations. This course introduces
the idea at a conceptual level; a full RCC implementation is left for further study.

## 5. In-Class Exercise
Compute `allen_relation` by hand for `(1, 10)` and `(3, 6)`; then, given `X before Y` and
`Y before Z`, determine (from the composition idea, without consulting the full table) what must
be true of the relation between `X` and `Z`.
