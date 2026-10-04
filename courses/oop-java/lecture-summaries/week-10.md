# Week 10 Summary — Generics I: Generic Classes

**Key takeaways:**
- A generic class like `Box<T>` or `Pair<T, U>` replaces an `Object`-based container, moving type
  errors from a runtime `ClassCastException` to a compile-time error, and removing the need for
  casts on retrieval.
- A type parameter is a placeholder filled in with a real type at the point of use; the same
  class definition serves every instantiation, each fully and independently type-checked.
- A bounded type parameter (`<T extends Number>`) lets the class body rely on methods that bound
  type guarantees, which a fully unbounded `<T>` would not allow.
- A raw type (using a generic class with no type argument) compiles but produces unchecked-call
  warnings and throws away every type-safety guarantee generics provide — always supply the type
  argument.

**You should now be able to:** write and instantiate a generic class with one or two type
parameters, and recognize a raw-type usage as a warning sign.

**Next week:** generics II — generic methods, wildcards, and the bridge to the Collections
Framework.
