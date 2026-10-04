# Lab Notes 14 — UML Diagramming & SOLID Refactoring

**Concept recap:** a UML class diagram shows class name, attributes, and methods; hollow-triangle
arrows mean inheritance, filled-diamond lines mean composition; SRP says split unrelated
responsibilities into separate classes; OCP says prefer adding a new class over editing an
existing `if`/`switch` on type.

**Common pitfalls:**
- Using the wrong arrow direction/type in the UML diagram — the hollow triangle points from
  derived *to* base (not the other way), and composition is a diamond, not an arrowhead.
- In the SRP refactor, splitting classes but leaving them tightly coupled in ways that still mix
  responsibilities (e.g. `ReportPrinter` reaching back into `DataReader`'s internals) — the goal
  is genuinely independent responsibilities, not just renamed files.
- In the OCP refactor, keeping a disguised type-string check (e.g. `dynamic_cast` chains instead
  of virtual dispatch) — the point is to let the compiler/vtable pick the right `area()`, not to
  re-implement the same branching by another means.
- Treating "composition vs. inheritance" as a purely syntactic choice rather than a genuine "is-a
  (really?) vs. has-a" design judgment about the specific domain.

**Debugging tip:** if adding a new shape type in Task D requires touching more than one existing
function, the refactor in Task C likely still has a hidden type check somewhere — search for any
remaining `if`/`switch` on a type name or tag.

**Instructor tip:** have students trade UML diagrams with a partner and attempt to reconstruct
the class declarations from the diagram alone — a strong test of whether the diagram actually
communicates the design.
