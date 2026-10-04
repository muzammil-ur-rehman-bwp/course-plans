# Lab Notes 5 — A General-Purpose Rule Engine

**Concept recap:** forward chaining repeatedly fires any rule whose premises are already known
until a fixed point; backward chaining recursively reduces a goal to its rule premises,
succeeding immediately on facts already in working memory.

**Common pitfalls:**
- Hard-coding a rule base's facts/predicates into the engine functions "just this once" — this
  defeats the entire point of the lab; the engine must take `facts` and `rules` as plain
  parameters and never reference a domain-specific string literal internally.
- Omitting cycle protection (`visited`) in `backward_chain` — a rule base with a cyclic
  dependency (even an unintentional one from a typo) causes infinite recursion; always track
  goals currently being proved and fail (not succeed) on revisiting one.
- Forgetting the forward-chaining fixed-point check, causing an infinite loop when no rule's
  conclusion is new — the `changed` flag must be reset to `False` at the start of each pass and
  only set `True` when a genuinely new fact is added.
- Mutating the caller's `facts` set in place inside `forward_chain` — copy it first
  (`facts = set(facts)`) so repeated calls with the same original fact set do not interfere with
  each other across Task D's two domains.

**Debugging tip:** print the `derived_by` trace after forward chaining on a small rule base and
manually confirm each entry's rule premises were in fact already in working memory at some
earlier pass — this catches a "fired too early" bug where a rule's applicability check ran against
a stale snapshot of facts.

**Instructor tip:** have students write the fault-diagnosis rule base *after* the animal rule
base and deliberately reuse variable/function names from the first attempt — if any engine code
needs to change for the second domain to work, that is the signal their design is not yet
domain-independent.
