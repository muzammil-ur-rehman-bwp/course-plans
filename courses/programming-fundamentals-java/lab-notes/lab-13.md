# Lab Notes 13 — Encapsulation

**Concept recap:** `private` fields with validated public accessor/mutator methods let a class
enforce its own rules; `static` members belong to the class (one shared copy), instance members
belong to each object (a separate copy per object).

**Common pitfalls:**
- Adding a plain setter for every private field out of habit, defeating the purpose of
  encapsulation if the setter performs no validation at all.
- Trying to access an instance field directly from a `static` method — this does not compile,
  because a `static` method has no `this` and no particular object to use.
- Forgetting to increment a `static` counter in *every* constructor, if a class has more than
  one, leading to an undercount.
- Validating in the constructor but not in later mutator methods (or vice versa), leaving a path
  through the class that can still produce an invalid object state.

**Debugging tip:** if a `static` counter's value looks wrong, check whether every constructor
path actually increments it — a class with multiple constructors is the most common place to
miss one.

**Instructor tip:** deliberately try to call an instance method from a `static` `main` without an
object (e.g., calling `deposit(50)` with no `accountName.deposit(50)`) to produce the compile
error live — "non-static method cannot be referenced from a static context" is a message students
will see often and should learn to recognize immediately.
