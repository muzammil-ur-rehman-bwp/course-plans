# Lab Notes 5 — Functions

**Concept recap:** pass-by-value gives a function a copy (changes don't escape the function);
pass-by-reference (`&`) gives direct access to the caller's variable; overloaded functions share a
name but differ in parameter list; default arguments let a trailing parameter be omitted at the
call site.

**Common pitfalls:**
- Expecting a pass-by-value parameter to change the caller's variable — it never does; use `&`
  if that's the goal.
- Declaring two overloads that differ only in return type (not parameters) — this does not
  compile; overload resolution is based on parameters, not return type.
- Giving a default argument to a parameter that is followed by a non-default parameter — illegal;
  default arguments must trail.
- Shadowing: naming a local variable the same as a parameter and assuming they're independent —
  they refer to the same thing unless deliberately scoped differently.

**Debugging tip:** if a function "isn't working," check the parameter list first — is it `&`
where it needs to be, and are the types what you think they are? Many function bugs are parameter-
passing bugs, not logic bugs.

**Instructor tip:** write a visibly broken pass-by-value "swap" live, run it, and have students
explain *why* it doesn't swap before showing the reference-parameter fix.
