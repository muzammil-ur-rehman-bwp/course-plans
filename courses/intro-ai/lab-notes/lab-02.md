# Lab Notes 2 — Agents: Simple Reflex & Model-Based Reflex

**Concept recap:** a simple reflex agent decides purely from the current percept; a model-based
reflex agent additionally maintains internal state (a belief about the world) so it can act
sensibly even with partial observability (e.g., knowing cell A is clean while currently standing
in cell B).

**Common pitfalls:**
- Forgetting to update the internal model *before* checking the stopping condition in the
  model-based agent — this causes it to stop one step too early or too late.
- Hard-coding the vacuum world to exactly 2 cells in a way that makes `VacuumWorld` impossible
  to extend; prefer a dict keyed by location over two named variables.
- Mixing up "the agent's belief about the world" with "the world's actual state" when comparing
  the two agents' behavior — the whole point of partial observability is that these can differ.

**Debugging tip:** print both the world's true state and the model-based agent's internal
belief at every step; if they diverge and never reconverge, the agent's model-update logic has a
bug (most often: updating the wrong cell's belief).

**Instructor tip:** have students predict, before running the code, how many steps the
model-based agent will take compared to the simple reflex agent on the same starting
configuration — the gap (or lack of one, in this very small world) is a good discussion seed for
when internal state actually pays off.
