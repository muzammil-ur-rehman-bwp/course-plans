# Week 12 Summary — Exception Handling

**Key takeaways:**
- `Throwable` splits into `Error` (serious JVM problems) and `Exception`, which further splits
  into unchecked `RuntimeException`s (no `throws` required) and everything else, which is checked
  and must be declared with `throws` or caught — a compiler-enforced discipline.
- A custom exception class extends `Exception` to be checked, or `RuntimeException` to be
  unchecked, depending on whether callers can reasonably be expected to recover from it.
- Multiple `catch` clauses are checked top to bottom and must be ordered specific to general, or
  the compiler flags an unreachable clause.
- try-with-resources automatically closes any `AutoCloseable` resource when the block exits,
  normally or via an exception, replacing manual `finally`-block cleanup.

**You should now be able to:** declare and throw a checked exception correctly, write a custom
exception class, and use try-with-resources for resource cleanup.

**Next week:** the Collections Framework — `List`/`ArrayList`, `Map`/`HashMap`, and sorting with
lambdas and `Comparator`.
