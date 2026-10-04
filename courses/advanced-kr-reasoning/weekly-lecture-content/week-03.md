# Week 3 — Lecture Content: Many-Valued and Paraconsistent Logics

## 1. Why Two-Valued Logic Is Sometimes the Wrong Tool
Classical logic forces every sentence to be exactly true or false. Two situations break this
assumption in practice: **genuine incompleteness** (a knowledge base simply does not yet know
whether φ holds — collapsing "unknown" to "false" is a modeling error, not a neutral default) and
**genuine inconsistency** (two trusted sources assert φ and ¬φ respectively — classically, this
single contradiction entails *everything* via explosion, which is useless). This week introduces
two independent responses: **three-valued logics** for the first problem, and **paraconsistent
logics** for the second.

## 2. Kleene's Strong Three-Valued Logic (K3)
K3 adds a third value **U** ("undefined"/"unknown") to {T, F}. Each connective's table is fixed by
one design principle: *the value is U exactly when the classical result cannot yet be determined,
no matter how U eventually resolves to T or F.*

| ∧ | T | F | U |      | ∨ | T | F | U |      | ¬ |   |
|---|---|---|---|      |---|---|---|---|      |---|---|
| **T** | T | F | U |   | **T** | T | T | T |   | T | F |
| **F** | F | F | F |   | **F** | T | F | U |   | F | T |
| **U** | U | F | U |   | **U** | T | U | U |   | U | U |

Reading `U ∧ F`: regardless of what U resolves to, F alone already forces the conjunction false,
so the table correctly gives F (determined). Reading `U ∧ T`: if U resolves to T the conjunction
is T, if U resolves to F the conjunction is F — not determined, so U. Kleene's implication is
usually taken as the derived form `a → b := ¬a ∨ b`, giving, in particular, `U → U = ¬U ∨ U = U ∨ U = U`.

## 3. Łukasiewicz's Three-Valued Logic (Ł3)
Ł3 agrees with K3 exactly on ∧, ∨, and ¬ (same tables above) but assigns **implication its own,
non-classical table**, standardly given numerically with T=1, F=0, U=0.5:
```
a → b  :=  min(1, 1 - a + b)
```
This is a "degree of truth preservation" reading rather than K3's "determinacy" reading. The
critical contrast case: **`U → U` in Ł3** is `min(1, 1 - 0.5 + 0.5) = min(1, 1) = 1 = T`, whereas
**K3 gives U → U = U**. The two logics are identical on every connective except this one, yet
disagree on this single, simple formula — illustrating that "U → U" is not a neutral technical
fact but a philosophical choice: Ł3 treats an implication between two equally-unknown things as
automatically true (truth is "preserved exactly as much as it is lost"), while K3 refuses to
commit, since the classical truth value of `U → U` genuinely depends on how U resolves (if U
resolves to T→F this is false; if U resolves to the same value on both sides it is true — K3
tracks that *this particular* dependency structure is not resolved, Ł3 does not track dependency
between occurrences of U at all).

```python
T, F, U = "T", "F", "U"

def k3_and(a, b):
    order = {F: 0, U: 1, T: 2}
    return min(a, b, key=lambda v: order[v])

def k3_or(a, b):
    order = {F: 0, U: 1, T: 2}
    return max(a, b, key=lambda v: order[v])

def k3_not(a):
    return {T: F, F: T, U: U}[a]

def k3_implies(a, b):
    return k3_or(k3_not(a), b)

NUM = {T: 1.0, F: 0.0, U: 0.5}
VAL = {1.0: T, 0.0: F, 0.5: U}

def luk_implies(a, b):
    r = min(1.0, 1.0 - NUM[a] + NUM[b])
    return VAL[r]

print(k3_implies(U, U))   # U
print(luk_implies(U, U))  # T
```

## 4. Paraconsistent Logics and the Belnap–Dunn Four-Valued Logic (FDE)
A logic is **paraconsistent** if it does not validate *ex contradictione sequitur quodlibet*
(from `φ` and `¬φ`, classically, infer any ψ whatsoever) — contradictions are tolerated locally
without collapsing the whole system. The Belnap–Dunn **First Degree Entailment (FDE)** logic
achieves this with four values, representing each formula's status by **independent evidence for
and against it**: a pair `(t, f) ∈ {0,1}²` where t = "there is support that it's true" and
f = "there is support that it's false." The four combinations are named **N** (0,0, neither),
**T** (1,0), **F** (0,1), and **B** (1,1, both — genuine, non-exploding contradiction, distinct
from simply "unknown"). Connectives operate pointwise on the two coordinates:
```
¬(t, f)          = (f, t)
(t1,f1) ∧ (t2,f2) = (min(t1,t2), max(f1,f2))
(t1,f1) ∨ (t2,f2) = (max(t1,t2), min(f1,f2))
```
```python
N, Tv, Fv, B = (0, 0), (1, 0), (0, 1), (1, 1)

def fde_not(x):
    t, f = x
    return (f, t)

def fde_and(x, y):
    (t1, f1), (t2, f2) = x, y
    return (min(t1, t2), max(f1, f2))

def fde_or(x, y):
    (t1, f1), (t2, f2) = x, y
    return (max(t1, t2), min(f1, f2))
```
Crucially, `fde_and(Tv, Fv)` (i.e. `P ∧ ¬P` where P has been independently asserted both true and
false by two sources, giving P itself value B) computes `fde_and(B, fde_not(B)) = fde_and(B, B) = (1,1) = B`
— still just B, a contradictory-but-contained value. Nothing in this semantics makes an unrelated
atom Q take any value other than whatever evidence actually exists for Q (typically N, if no
source has spoken on Q at all). This is explosion's absence made concrete: classically,
`{P, ¬P} ⊨ Q` for *every* Q; in FDE, Q's value is wholly undetermined by P's contradiction.

## 5. Application: Merging Disagreeing Sources
Model two sensor feeds as FDE assertions over a shared vocabulary: feed 1 asserts `door_open`
(giving it t-evidence), feed 2 asserts `¬door_open` (giving it f-evidence). Combining the evidence
(not resolving it) gives `door_open = B`. A downstream rule `door_open ∧ alarm_armed → evacuate`
can still be evaluated meaningfully under FDE's tables without the whole knowledge base collapsing
to "everything is now assertable" — exactly the property classical logic lacks here.

## 6. In-Class/Lab Exercise
Build a toy KB with one direct contradiction (`B` on one atom `P`) and at least two unrelated
atoms with ordinary, non-contradictory evidence. Using the FDE evaluator, confirm: (a) a query on
an atom unrelated to P returns its own evidence-based value, not B; (b) a classical evaluator run
on the "flattened" two-valued projection of the same KB (ignoring FDE, treating the contradiction
as triggering explosion) would instead conclude every query is derivable — write one sentence
contrasting the two outcomes.
