# Week 11 Summary — Templates II: Class Templates

**Key takeaways:**
- `template <typename T> class` generalizes an entire class over a type parameter; `Stack<int>`
  and `Stack<std::string>` are separate classes the compiler generates from the same template.
- Member functions can be defined outside the class body using `template <typename T>` and the
  `ClassName<T>::` scope qualifier, though small templates are often defined entirely inline.
- A class template can take multiple independent type parameters (`Pair<T, U>`), the same pattern
  `std::pair` and `std::map` are built on.
- Everything learned earlier this semester (constructors, the Rule of Three, operator
  overloading) applies equally to template classes.

**You should now be able to:** design and implement a generic container class template with
multiple type parameters where needed, and instantiate it for multiple concrete types in one
program.

**Next week:** exception handling — `try`/`catch`/`throw`, the standard exception hierarchy, and
writing custom exception classes.
