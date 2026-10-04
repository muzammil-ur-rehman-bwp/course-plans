# Lab Notes 9 — Interfaces

**Concept recap:** an interface declares a contract with no instance state; a class `implements`
it to fulfill that contract. A class may implement many interfaces while extending only one
class. A `default` method supplies a body on the interface itself, inherited automatically by
every implementer that doesn't override it.

**Common pitfalls:**
- Forgetting `@Override` on an interface method implementation — not required by the compiler
  the way it is for class overriding in some cases, but still good practice and still catches
  signature mistakes.
- Trying to give an interface an instance field — interfaces may only declare constants
  (implicitly `public static final`), never genuine per-instance state.
- Assuming `Car` and `Bicycle` need a common superclass to share the `Drivable` contract — they
  explicitly do not; that's the entire point of an interface.
- Confusing a `default` method with an abstract class's concrete method — a default method still
  cannot reference any instance field, because an interface has none.

**Debugging tip:** "Car is not abstract and does not override abstract method ... in Drivable"
means a required (non-default) interface method is missing its implementation — implement it.

**Instructor tip:** ask students, for Task C, which interfaces's method resolves first if both
`Drivable` and `Lockable` happened to declare a default method with the same signature — a
natural segue into why Java requires explicit resolution rather than silently picking one.
