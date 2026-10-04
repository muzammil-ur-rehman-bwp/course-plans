# Quiz 2 — Constructors/Destructors & Operator Overloading I (Week 4)

**Duration:** 15 minutes | **Format:** 5 short-answer/code questions | **Weight:** part of Quizzes (10%, best 5 of 6 counted)

1. Why must a `const` data member or a reference data member be initialized in the member
   initializer list, rather than assigned in the constructor body? (2 pts)
2. State the Rule of Three in one or two sentences. (2 pts)
3. Given a class that owns a raw pointer with a correct destructor but no user-defined copy
   constructor, what specific bug occurs when an object of that class is copied and both copies
   are later destroyed? (2 pts)
4. Why should `operator+` for a value type like `Fraction` return a new object instead of
   modifying and returning `*this`? (2 pts)
5. What is the key difference in return type/behavior between `operator+` and `operator+=` for a
   typical value type? (2 pts)

**Answer key available to instructors in the course LMS gradebook (not distributed to students
before the quiz).**
