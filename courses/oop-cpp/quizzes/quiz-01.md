# Quiz 1 — Encapsulation (Week 2)

**Duration:** 15 minutes | **Format:** 5 short-answer/code questions | **Weight:** part of Quizzes (10%, best 5 of 6 counted)

1. What is the default access level of a `class`'s members, and how does it differ from a
   `struct`'s? (2 pts)
2. What does marking a member function `const` promise, and what won't compile if you try to
   modify a data member inside such a function? (2 pts)
3. Why is a setter usually the right place to validate a new value, rather than trusting every
   caller to pass a valid one? (2 pts)
4. What access level is visible to a derived class's own member functions but not to outside
   code? (2 pts)
5. Given the following, what is wrong with this class's design from an encapsulation standpoint?
   ```cpp
   class Thermostat {
   public:
       double targetTemp;
   };
   ```
   (2 pts)

**Answer key available to instructors in the course LMS gradebook (not distributed to students
before the quiz).**
