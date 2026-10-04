# Week 10 Summary — Templates I: Function Templates

**Key takeaways:**
- `template <typename T>` lets you write a function once for a placeholder type `T`, which the
  compiler instantiates concretely for each type actually used at a call site.
- The compiler deduces `T` from the arguments' types; when arguments have different types,
  deduction can be ambiguous and may need an explicit `funcName<Type>(...)` call.
- A template function places no explicit restriction on `T`, but its body implicitly requires
  whatever operations (`<`, `>`, `==`, etc.) it actually uses — compilation fails if `T` doesn't
  support them, often with an error pointing inside the template body.

**You should now be able to:** write a function template that replaces several type-specific
overloads, and trace how the compiler deduces or requires an explicit type parameter.

**Next week:** templates II — class templates, building a generic container like `Stack<T>`.
