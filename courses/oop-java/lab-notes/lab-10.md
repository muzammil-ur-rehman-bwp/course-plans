# Lab Notes 10 — Generic Classes

**Concept recap:** a type parameter like `<T>` is a placeholder filled in at the point of use; a
generic class definition serves every instantiation, each independently type-checked. A bounded
type parameter (`<T extends Number>`) lets the class body rely on methods the bound guarantees. A
raw type (no type argument supplied) compiles but produces an unchecked warning and throws away
type safety.

**Common pitfalls:**
- Writing `Box<T>` but then declaring fields/variables as the raw `Box` out of habit — always
  supply the type argument.
- Assuming `<T extends Number>` means `T` literally must be the class `Number` — it means `T`
  must be `Number` *or any subclass* (`Integer`, `Double`, etc.).
- Forgetting the diamond operator (`new Box<>()`) and writing `new Box()` instead, which compiles
  as a raw-type instantiation, not a shorthand for the parameterized one.
- Mixing two different type arguments for the same variable across a method, expecting Java to
  reconcile them automatically — each instantiation is independent and fixed once declared.

**Debugging tip:** `javac -Xlint:unchecked` surfaces raw-type and unchecked-cast warnings that a
plain `javac` run can hide — run it before submitting Task D.

**Instructor tip:** show the pre-generics `ObjectBox` from the lecture failing at runtime with a
`ClassCastException` side-by-side with `Box<T>` catching the same mistake at compile time, to
make the payoff concrete.
