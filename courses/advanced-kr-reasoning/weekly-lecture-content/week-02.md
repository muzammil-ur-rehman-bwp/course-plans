# Week 2 — Lecture Content: Higher-Order Logic and Type Theory

## 1. Why First-Order Logic Has Expressive Limits
First-order logic (FOL) quantifies only over *individuals* in a domain — `∀x. P(x)` ranges over
elements of the domain, never over predicates or relations themselves. This is a real
expressiveness ceiling, not a notational inconvenience. The mathematical-induction principle for
natural numbers is naturally stated as a single second-order sentence:
```
∀P. [P(0) ∧ ∀n.(P(n) → P(n+1))] → ∀n. P(n)
```
quantifying over *every property* P. FOL cannot express this one sentence; it can only offer the
induction **schema** — a separate axiom for each concrete formula substituted for P, infinitely
many axioms, never capturing "for every property" in one statement. Other genuinely
second-order statements include "two graphs are isomorphic" (quantifying over bijections, i.e.
relations) and "R is a well-founded relation" (quantifying over all subsets of the domain to say
every nonempty one has an R-minimal element). **Second-order logic** adds quantification over
predicates and functions; **higher-order logic (HOL)** iterates this — predicates of predicates,
functions taking functions as arguments, and so on, with a type discipline needed to keep the
resulting language well-formed (an unrestricted "set of everything including itself" style
construction reintroduces paradox, exactly as in naive set theory).

## 2. The Simply-Typed Lambda Calculus
The simply-typed lambda calculus gives HOL its standard syntactic backbone. **Types** are built
from a small set of **base types** (e.g., `e` for entities, `t` for truth values) by the
function-type constructor: if σ and τ are types, so is `σ → τ` (the type of functions from σ to
τ). **Terms** are built by three rules:
- a **variable** `x` of a declared type;
- **abstraction**: if `M` is a term of type τ given `x:σ` in context, then `λx:σ. M` is a term of
  type `σ → τ` (a function that, given an argument for x, returns M);
- **application**: if `M` has type `σ → τ` and `N` has type σ, then `M N` has type τ.

**β-reduction** is the computation rule: `(λx:σ. M) N  →  M[x := N]` (substitute N for every free
occurrence of x in M). A term with no further β-reductions available is in **normal form**. The
Church–Rosser property guarantees that if a term has a normal form, every reduction order reaches
the same one — a fact this course uses without re-proving it.

Crucially, a function — including something standing for a relation, such as
`λx:e. λy:e. R x y` (type `e → e → t`, read: "the relation R") — is now a first-class *term* that
can itself be passed as an argument or appear inside another abstraction. `λR:(e→e→t). λx:e. R x x`
has type `(e→e→t) → e → t`: it takes a binary relation R and returns the property "being
R-related to oneself" (reflexivity-at-x). This is a genuinely higher-order object: it quantifies
*over relations*, which plain FOL cannot do.

## 3. A Minimal Typed Lambda Calculus Interpreter
```python
from dataclasses import dataclass
from typing import Union

@dataclass(frozen=True)
class Base:
    name: str
    def __repr__(self): return self.name

@dataclass(frozen=True)
class Arrow:
    dom: "Type"
    cod: "Type"
    def __repr__(self): return f"({self.dom}->{self.cod})"

Type = Union[Base, Arrow]

@dataclass(frozen=True)
class Var:
    name: str

@dataclass(frozen=True)
class Abs:
    var: str
    var_type: Type
    body: "Term"

@dataclass(frozen=True)
class App:
    fun: "Term"
    arg: "Term"

Term = Union[Var, Abs, App]


def type_check(term: Term, ctx: dict) -> Type:
    """Infers the type of `term` under context `ctx` (var name -> Type), or raises TypeError."""
    if isinstance(term, Var):
        if term.name not in ctx:
            raise TypeError(f"unbound variable {term.name}")
        return ctx[term.name]
    if isinstance(term, Abs):
        body_ctx = {**ctx, term.var: term.var_type}
        return Arrow(term.var_type, type_check(term.body, body_ctx))
    if isinstance(term, App):
        fun_type = type_check(term.fun, ctx)
        arg_type = type_check(term.arg, ctx)
        if not isinstance(fun_type, Arrow) or fun_type.dom != arg_type:
            raise TypeError(f"cannot apply {fun_type} to {arg_type}")
        return fun_type.cod
    raise TypeError(f"unknown term {term}")


def substitute(term: Term, var: str, replacement: Term) -> Term:
    if isinstance(term, Var):
        return replacement if term.name == var else term
    if isinstance(term, Abs):
        if term.var == var:
            return term  # var is shadowed; stop substituting
        return Abs(term.var, term.var_type, substitute(term.body, var, replacement))
    if isinstance(term, App):
        return App(substitute(term.fun, var, replacement), substitute(term.arg, var, replacement))
    raise TypeError(f"unknown term {term}")


def beta_reduce(term: Term) -> Term:
    """Reduces `term` to normal form under leftmost-outermost (normal-order) reduction."""
    if isinstance(term, App):
        fun = beta_reduce(term.fun)
        if isinstance(fun, Abs):
            return beta_reduce(substitute(fun.body, fun.var, term.arg))
        return App(fun, beta_reduce(term.arg))
    if isinstance(term, Abs):
        return Abs(term.var, term.var_type, beta_reduce(term.body))
    return term
```

## 4. Worked Example
Take `R : e -> e -> t`, `a : e`, `b : e`, and the term `(λx:e. λy:e. R x y) a b`:
```python
e, t = Base("e"), Base("t")
R = Var("R")
ctx = {"R": Arrow(e, Arrow(e, t)), "a": e, "b": e}

term = App(App(Abs("x", e, Abs("y", e, App(App(R, Var("x")), Var("y")))), Var("a")), Var("b"))
print(type_check(term, ctx))        # t
print(beta_reduce(term))            # App(App(R, a), b)  i.e. "R a b"
```
Type-checking confirms the whole application has type `t` (a truth value), and β-reduction
confirms it computes to exactly `R a b` — the relation R applied to a and b — which is what the
original un-reduced term was designed to mean.

## 5. Connections to KR Systems
Montague-style **compositional semantics** builds a sentence's meaning by function application
of typed lambda terms assigned to its words and phrases — a transitive verb might denote
`λy:e. λx:e. loves(x,y)` (type `e → e → t`), combined by application with its object and subject
in turn, with the final sentence meaning normalizing, by β-reduction, to a flat atomic formula —
directly the mechanism implemented above. **Typed feature structures**, used in some ontology and
grammar formalisms, enforce an analogous type discipline to prevent ill-typed compositions (e.g.,
refusing to combine a feature expecting an entity-typed filler with a truth-valued one) — the same
idea as this week's type-checker, applied to a different representational structure. Neither
system is implemented in full here; both are surveyed as evidence that the higher-order,
typed-lambda-calculus foundation introduced this week is not merely of logical interest but
underlies real representational machinery.

## 6. In-Class/Lab Exercise
Translate "Alice introduced Bob to Carol" compositionally: assign `introduce` the type
`e → e → e → t` and the term `λz:e. λy:e. λx:e. introduce x y z`; apply it to `carol`, `bob`,
`alice` in turn (object, indirect object, subject — matching standard argument order), type-check
each intermediate application, and confirm the fully-applied term β-reduces to
`introduce alice bob carol`.
