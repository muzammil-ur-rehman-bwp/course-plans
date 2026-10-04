# Lab Notes 6 — Inheritance I

**Concept recap:** `class Derived : public Base` inherits `Base`'s `public`/`protected` members;
a derived constructor must chain to a base constructor (explicitly in the initializer list when
`Base` has no usable default constructor); construction runs base-first, destruction runs
derived-first.

**Common pitfalls:**
- Forgetting to chain to `Animal`'s constructor at all when `Animal` has no default constructor —
  this fails to compile with an error about no matching base-class constructor.
- Trying to access `name_` as if it were `public` from *outside* the `Animal`/`Dog` hierarchy —
  `protected` still blocks that; only `Dog`'s own member functions (like `bark()`) may use it
  directly.
- Writing `Dog`'s constructor with the base-class call in the wrong place (not actually in the
  initializer list) — `Animal(name)` must appear in `Dog`'s own initializer list, not as a
  statement in the constructor body.
- Using `private` instead of `protected` for `name_` and then being confused why `Dog::bark()`
  can't access it — re-check the access specifier if a derived class can't reach an inherited
  member it should be able to.

**Debugging tip:** a compiler error citing "no matching function for call to Animal()" inside
`Dog`'s constructor almost always means you forgot to explicitly chain to a non-default `Animal`
constructor in `Dog`'s initializer list.

**Instructor tip:** show the exact compiler error produced by omitting the explicit base-
constructor call, so students learn to recognize and fix it quickly rather than guessing.
