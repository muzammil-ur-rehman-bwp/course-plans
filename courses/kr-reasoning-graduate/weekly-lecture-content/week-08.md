# Week 8 — Lecture Content: Argumentation Frameworks; Midterm Review

## 1. Abstracting Away From Argument Content
Belief revision (Week 7) and non-monotonic reasoning more broadly need a way to reason about
*conflicting* pieces of information. Dung's **abstract argumentation framework** deliberately
throws away each argument's internal logical structure and studies acceptability purely in terms
of who attacks whom — a surprisingly powerful simplification that subsumes default logic,
logic programming's stable models (Week 6), and more, as special cases (Dung, 1995).

## 2. Dung's Argumentation Framework
An AF is a pair ⟨A, →⟩: A is a finite set of **arguments**, and → ⊆ A×A is the **attack**
relation (a→b reads "a attacks b"). For S ⊆ A:
- S is **conflict-free** if no a, b ∈ S have a→b.
- An argument a is **acceptable with respect to S** (S *defends* a) if every attacker of a is
  itself attacked by some member of S.
- S is **admissible** if S is conflict-free and every member of S is acceptable w.r.t. S (S
  defends itself).

## 3. The Grounded Extension
Define the **characteristic function** F(S) = { a ∈ A : S defends a }. The **grounded extension**
is the least fixed point of F, computed by iterating from ∅:
```
S0 = ∅
S_{i+1} = F(S_i)
```
This sequence is monotonically increasing (for AFs in general it is guaranteed to converge for
finite A) and its limit is the grounded extension — unique, and always exists.

```python
def characteristic_function(A, attacks, S):
    attackers = {a: {x for (x, y) in attacks if y == a} for a in A}
    defended = set()
    for a in A:
        if all(any((d, x) in attacks for d in S) for x in attackers[a]):
            defended.add(a)
    return defended

def grounded_extension(A, attacks):
    S = set()
    while True:
        new_S = characteristic_function(A, attacks, S)
        if new_S == S:
            return S
        S = new_S
```

**Worked example.** A = {a, b, c}, attacks = {(a,b), (b,c)} (a attacks b, b attacks c — a simple
attack chain; attacker(a)=∅, attacker(b)={a}, attacker(c)={b}). S0 = ∅.
- **F(∅):** a has no attackers, so a is (vacuously) defended → a ∈ F(∅). b's only attacker is a;
  is a attacked by some member of ∅? No → b not defended. c's only attacker is b; is b attacked
  by some member of ∅? No → c not defended. So F(∅) = {a}.
- **F({a}):** a is still defended (no attackers). b's attacker is a; is a attacked by a member of
  {a}? Nothing attacks a → b still not defended. c's attacker is b; is b attacked by a member of
  {a}? Yes — a→b holds, and a ∈ {a} → c is now defended. So F({a}) = {a, c}.
- **F({a,c}):** a still defended; b's attacker a is still unattacked by {a,c} → b still not
  defended; c's attacker b is still attacked by a ∈ {a,c} → c still defended. F({a,c}) = {a,c},
  a fixed point.

**Grounded extension = {a, c}** — the classic "a defends c by attacking c's attacker b" pattern;
b is excluded because nothing ever attacks its attacker a.

## 4. Preferred Extensions
A **preferred extension** is a (⊆-)maximal admissible set. There can be several. The grounded
extension is always a subset of every preferred extension (it is the most cautious, "no
unforced choices" answer), but the two can genuinely diverge: a simple **mutual attack** between
two arguments is the standard textbook example where the grounded extension is empty while two
distinct, non-empty preferred extensions exist.

**Worked example (mutual attack).** A = {a, b}, attacks = {(a,b), (b,a)} (a attacks b and b
attacks a; attacker(a)={b}, attacker(b)={a}). Iterating F from ∅: a is defended iff its attacker
b is attacked by a member of ∅ — false; symmetrically b is not defended either. F(∅) = ∅, already
a fixed point. **Grounded extension = ∅** — neither argument can be unconditionally accepted.
But {a} is admissible: conflict-free (trivially, one element, and a does not attack itself), and
a is acceptable w.r.t. {a} because a's only attacker, b, is attacked by a ∈ {a} (a→b holds). By
the identical argument, {b} is also admissible. Neither {a} nor {b} can be extended further
without losing conflict-freeness (adding the other creates a→b or b→a inside the set), so both
are **maximal** admissible sets. **Preferred extensions = {a} and {b}** — two legitimate,
mutually exclusive rational positions, exactly capturing that a reasoner must pick a side between
two symmetric, mutually attacking arguments, even though neither can be *unconditionally*
(groundedly) accepted.

```python
def is_admissible(A, attacks, S):
    attacked_by_S = {y for (x, y) in attacks if x in S}
    if any((x, y) in attacks for x in S for y in S):
        return False  # not conflict-free
    attackers = {a: {x for (x, y) in attacks if y == a} for a in A}
    return all(att in attacked_by_S for a in S for att in attackers[a])

def preferred_extensions(A, attacks):
    from itertools import chain, combinations
    admissible = [set(c) for r in range(len(A) + 1)
                  for c in combinations(A, r) if is_admissible(A, attacks, set(c))]
    return [s for s in admissible if not any(s < t for t in admissible)]
```

## 5. Applications
Argumentation frameworks model reasoning with explicitly conflicting claims — e.g., competing
legal arguments, or a multi-agent dialogue where agents exchange attacking arguments — where
"what should a rational reasoner accept" is exactly the grounded (most cautious) or preferred
(maximal, possibly several alternative) extension.

## 6. Midterm Review
Practice problems span: Kripke-model evaluation and modal-system identification (Week 2); LTL
trace evaluation (Week 3); the ALC tableau algorithm and DL complexity (Week 4); sequent
calculus, FOL tableau, and resolution refinements (Week 5); stable-model computation via the
GL-reduct (Week 6); AGM postulate checking and Dalal revision (Week 7); grounded/preferred
extension computation (Week 8, this week).

## 7. In-Class/Lab Exercise
Using `grounded_extension` and `preferred_extensions`, reproduce §3's chain example ({a,c}) and
§4's mutual-attack example (grounded = ∅, preferred = {a} and {b}). Then extend the mutual-attack
pair with a third argument d that attacks a only (and is attacked by nothing), and confirm the
grounded extension becomes non-empty — d is unconditionally accepted, which lets it defend b by
defeating b's attacker a, so the grounded extension becomes {d, b}.
