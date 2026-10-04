# Lab Notes 8 — Virtual Functions & Object Slicing

**Concept recap:** `virtual` enables dynamic dispatch — the call resolves via the object's actual
(dynamic) type, not the pointer/reference's static type; a polymorphic base class needs a
`virtual` destructor or deleting through a base pointer skips derived cleanup; object slicing
discards the derived part of an object passed/assigned by value as its base type.

**Common pitfalls:**
- **Missing virtual destructor:** the single most consequential pitfall in this lab — `delete`
  through a base pointer without a `virtual` destructor skips the derived destructor entirely,
  leaking any resources it would have freed; this is undefined behavior, not merely a leak, so
  "it worked when I tested it" is not evidence it is correct.
- **Object slicing:** passing a `Dog` to a `void describe(Animal a)` by-value parameter silently
  compiles and silently discards `Dog`'s own data/behavior — no warning, no error, just wrong
  behavior, which is exactly why it is so dangerous.
- Forgetting `override` on the derived version, which (once `virtual` exists in the base) would
  otherwise only be caught by careful signature-matching, not a compile error — always use
  `override`, every time, from Week 8 onward.
- Assuming `virtual` is "automatically inherited" for new functions added only in the derived
  class — `virtual`/dynamic dispatch only applies to functions declared `virtual` somewhere in
  the hierarchy and called by that name through a base pointer/reference.

**Debugging tip:** if a memory checker reports a leak only involving derived-class resources
(never base-class ones), suspect a missing `virtual` destructor on the base class first.

**Instructor tip:** run Task C's broken version under a memory/leak checker if available in your
environment, and show the leak report naming `Dog`'s resource specifically — concrete evidence is
far more convincing than the rule stated abstractly.
