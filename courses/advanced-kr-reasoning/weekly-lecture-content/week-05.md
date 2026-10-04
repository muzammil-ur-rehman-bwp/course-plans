# Week 5 — Lecture Content: Structured Argumentation — ASPIC+

## 1. From Abstract to Structured Arguments
The graduate course's Dung framework AF = ⟨A, →⟩ treats each argument in A as an **opaque node**
— acceptability is computed purely from the attack relation →, with no account of *why* one
argument attacks another or what an argument is actually made of. **ASPIC+** gives arguments
internal structure, built from three ingredients:
- A **knowledge base** K of **premises**, partitioned into ordinary premises (assailable) and
  axiom premises (never attackable).
- **Strict inference rules**, `φ1,...,φn → φ` — classically valid: if the premises hold, φ
  follows beyond question.
- **Defeasible inference rules**, `φ1,...,φn ⇒ φ` — hold only presumptively ("normally, if
  φ1,...,φn then φ"), and are exactly the ingredient that can be attacked.

An **argument** is built recursively: every premise is a trivial argument for itself; given
arguments A1,...,An for φ1,...,φn and a rule (strict or defeasible) `φ1,...,φn → φ` or
`φ1,...,φn ⇒ φ`, applying it yields a new, larger argument for φ, whose immediate **sub-arguments**
are A1,...,An. The rule applied last is the argument's **top rule**.

```python
from dataclasses import dataclass, field

@dataclass
class Rule:
    name: str
    premises: tuple
    conclusion: str
    strict: bool  # False = defeasible

@dataclass
class Argument:
    conclusion: str
    top_rule: Rule | None       # None for a bare premise
    sub_arguments: tuple        # arguments for the top rule's premises (empty if top_rule is None)
    is_premise: bool = False
    premise_name: str = ""

def build_arguments(premises, rules, max_depth=4):
    """Enumerate arguments constructible from `premises` (list of str) and `rules`
    (list of Rule) up to `max_depth` rule applications."""
    args = [Argument(p, None, (), is_premise=True, premise_name=p) for p in premises]
    for _ in range(max_depth):
        new_args = []
        concl_to_args = {}
        for a in args:
            concl_to_args.setdefault(a.conclusion, []).append(a)
        for rule in rules:
            if all(p in concl_to_args for p in rule.premises):
                import itertools
                for combo in itertools.product(*(concl_to_args[p] for p in rule.premises)):
                    new_args.append(Argument(rule.conclusion, rule, combo))
        for a in new_args:
            if a not in args:
                args.append(a)
    return args
```

## 2. Attack Types
ASPIC+ distinguishes attacks by *what is being targeted*, and both attack types below are only
ever possible against a **defeasible** element — a strict rule or an axiom premise can never be
legitimately attacked, since they hold beyond question by construction.
- **Rebutting attack:** argument A rebuts argument B on sub-argument B′ of B if **A's conclusion
  contradicts B′'s conclusion** and B′'s top rule is defeasible. Rebuttal targets a *conclusion*
  — it says "I reach the opposite conclusion from yours, at this specific point in your
  reasoning."
- **Undercutting attack:** argument A undercuts B on sub-argument B′ of B if A's conclusion is
  specifically a statement that **denies the applicability of B′'s top (defeasible) rule** —
  formally, A concludes a proposition naming that rule's non-applicability (written, e.g.,
  `¬appl(r)` for a defeasible rule r). Undercutting targets the **rule's licence to fire**, not
  any conclusion — it says "your inference step itself does not apply here," without disputing
  what the conclusion *would* mean if the rule did apply.
- **Undermining attack** (the premise-level counterpart): A attacks B by attacking one of B's
  *ordinary* (non-axiom) premises directly — a special case of rebuttal aimed at a leaf rather
  than an intermediate inference step.

The rebut/undercut distinction is the one most often confused: rebutting two arguments that reach
opposite conclusions about, say, "the suspect is guilty" vs. "the suspect is innocent" is
different in kind from undercutting an argument that infers guilt via a defeasible rule
"normally, fingerprints at the scene indicate presence" by arguing that rule specifically
*does not apply here* (e.g., because the fingerprints are known to be old) — the undercutter never
claims the suspect is innocent, only that this particular inference step is inapplicable.

```python
def attacks(a: Argument, b: Argument):
    """Yields (sub_argument_of_b, attack_type) for every attack a makes on b."""
    def walk(sub):
        if sub.top_rule is not None and not sub.top_rule.strict:
            if a.conclusion == f"not_{sub.conclusion}" or sub.conclusion == f"not_{a.conclusion}":
                yield (sub, "rebut")
            if a.conclusion == f"not_appl({sub.top_rule.name})":
                yield (sub, "undercut")
        if sub.is_premise and a.conclusion == f"not_{sub.conclusion}":
            yield (sub, "undermine")
        for child in sub.sub_arguments:
            yield from walk(child)
    yield from walk(b)
```

## 3. Preferences, Defeat, and Dung Semantics
Not every attack succeeds: a **preference ordering** over defeasible rules (or over the arguments
themselves) determines which attacks actually **defeat** their target — e.g., an attack from a
less-preferred defeasible rule against a more-preferred one may fail to defeat it. The resulting
**defeat relation** is exactly a Dung attack relation →, so the graduate course's grounded- and
preferred-extension computations apply directly on top of it. ASPIC+ is therefore best understood
as a **generator** of a Dung framework from structured ingredients (rules, premises, attacks,
preferences) — it does not replace Dung's acceptability semantics, it supplies the → relation
those semantics are computed over.

## 4. In-Class/Lab Exercise
From premises `{bird(tweety), penguin(tweety)}`, a strict rule `penguin(tweety) → not_flies` and a
defeasible rule `bird(tweety) ⇒ flies`, build both arguments, confirm the strict argument rebuts
the defeasible one at its top-level conclusion, and determine (given a stated preference for
strict over defeasible arguments) which one survives as defeating. Then add a defeasible rule
`¬appl(bird(tweety) ⇒ flies)` triggered by `penguin(tweety)` and confirm it instead *undercuts*
(rather than rebuts) the flies-argument, and explain in one sentence why this is a more
information-faithful way to model "penguins are an exception to the bird-flying rule" than
rebuttal is.
