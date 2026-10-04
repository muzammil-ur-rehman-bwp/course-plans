# Lab Notes 1 — Environment Setup & PEAS Analysis

**Concept recap:** PEAS (Performance measure, Environment, Actuators, Sensors) specifies a task
environment precisely before any agent design decision is made; a table-driven agent just looks
up an action from a stored percept-history table, which does not scale but illustrates the
agent-function idea concretely.

**Common pitfalls:**
- Confusing the performance measure (how an outside observer judges success) with the agent's
  own goal or strategy — they are not the same thing.
- Writing a PEAS "sensor" list that secretly describes actuators, or vice versa; double-check
  each item answers "how does it perceive" vs. "how does it act."
- Using an unhashable type (e.g., a list) as a dict key for the percept-history table; use
  tuples instead.

**Debugging tip:** if `TableDrivenAgent` always returns `"NoOp"`, print the exact key you are
looking up and compare it character-for-character against the keys stored in the table — a
common bug is building the table with a list and looking it up with a tuple (or vice versa).

**Instructor tip:** spend extra time on the PEAS exercise for environments students find
genuinely ambiguous (e.g., "a personal assistant app") — productive disagreement about what
counts as a sensor vs. an actuator here pays off for the rest of the semester.
