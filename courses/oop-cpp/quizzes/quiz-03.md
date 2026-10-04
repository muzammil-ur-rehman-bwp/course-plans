# Quiz 3 — Operator Overloading II (`friend`) & Composition (Week 5)

**Duration:** 15 minutes | **Format:** 5 short-answer/code questions | **Weight:** part of Quizzes (10%, best 5 of 6 counted)

1. Why can't `operator<<` for printing a user-defined type be an ordinary member function of
   that type? (2 pts)
2. What access does declaring a function `friend` inside a class grant it, and why should
   `friend` be used sparingly? (2 pts)
3. What must a correct `operator<<` implementation return, and why does chaining
   (`std::cout << a << b`) depend on it? (2 pts)
4. In one sentence, distinguish composition ("has-a") from the inheritance relationship covered
   starting next week ("is-a"). (2 pts)
5. If `class Car` has a member `Engine engine_;` declared after `std::string model_;`, which is
   constructed first when a `Car` object is created? (2 pts)

**Answer key available to instructors in the course LMS gradebook (not distributed to students
before the quiz).**
