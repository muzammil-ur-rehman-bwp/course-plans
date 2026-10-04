# Week 13 Summary — More on Classes: Encapsulation, Static vs. Instance

**Key takeaways:**
- Encapsulation means making fields `private` and exposing controlled, validated access through
  public methods, so a class can enforce its own rules rather than allowing arbitrary external
  changes.
- Not every private field needs a plain setter — a method like `withdraw` can validate before
  changing state, which a blanket setter would not.
- A `static` field/method belongs to the class itself (one shared copy); an instance field/method
  belongs to each object individually (a separate copy per object).
- This course still explicitly excludes inheritance, polymorphism, interfaces, and `abstract`
  classes — those belong to the follow-on Object-Oriented Programming course.

**You should now be able to:** design a class with private fields and validated public methods;
distinguish and correctly use `static` vs. instance members.

**This week:** Assignment 3 was assigned (file I/O, exceptions, recursion) — see
`assignments/assignment-03.md`. Quiz 6 (recursion & encapsulation) — see `quizzes/quiz-06.md`.

**Next week:** sorting and searching algorithms.
