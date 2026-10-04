# Lab Notes 15 — Debugging & Testing OOP Code

**Concept recap:** a stack trace reads top to bottom, with the top frame showing exactly where an
exception was thrown. Four recurring OOP bugs — missing `@Override`, a wrong `equals` parameter
type, `equals` without a matching `hashCode`, and a raw generic type — all compile cleanly and
only misbehave at runtime. A hand-rolled test harness (`check(label, condition)`) exercises a
class's behavior without needing a full testing framework.

**Common pitfalls:**
- Reading a stack trace bottom-to-top out of habit from reading code top-to-bottom — the
  *origin* of the exception is always the top frame, not the bottom.
- Fixing the symptom reported in a lower stack frame instead of the actual root cause in the top
  frame.
- Writing `check(...)` calls that only test the "happy path," missing the exact edge case (e.g. a
  `null` field) that caused Task A's bug in the first place.
- Treating the bug checklist as exhaustive — it is a starting checklist of recurring mistakes
  from this course, not a complete list of every possible bug.

**Debugging tip:** when a stack trace spans a class hierarchy, identify which concrete class's
override is named in the top frame — that tells you immediately which version of the method
actually ran, which is often the whole question in a polymorphism-related bug.

**Instructor tip:** have students predict, before running Task A, which class's code they expect
the top stack frame to name, then compare — mispredictions here usually reveal a dynamic-dispatch
misunderstanding worth revisiting from Week 7.
