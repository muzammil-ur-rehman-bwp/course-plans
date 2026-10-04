# Quiz 6 — Class Templates & Exception Handling (Week 13)

**Duration:** 15 minutes | **Format:** 5 short-answer/code questions | **Weight:** part of Quizzes (10%, best 5 of 6 counted)

1. Are `Stack<int>` and `Stack<std::string>` the same class at runtime, or two separately
   generated classes? Explain briefly. (2 pts)
2. What goes in the scope-qualifier position when defining a class template's member function
   outside the class body (e.g. for `template <typename T> class Stack`)? (2 pts)
3. What happens to the call stack between a `throw` and the matching `catch`, and what runs
   automatically during that process? (2 pts)
4. Why should a `catch` clause take its exception parameter by reference rather than by value?
   (2 pts)
5. Given `std::invalid_argument` and `std::out_of_range`, which standard header declares them,
   and name one situation where each would be the more appropriate type to throw. (2 pts)

**Answer key available to instructors in the course LMS gradebook (not distributed to students
before the quiz).**
