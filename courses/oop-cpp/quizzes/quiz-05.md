# Quiz 5 — Abstract Classes & Function Templates (Week 11)

**Duration:** 15 minutes | **Format:** 5 short-answer/code questions | **Weight:** part of Quizzes (10%, best 5 of 6 counted)

1. What makes a class "abstract" in C++, and what happens if you try to instantiate one directly?
   (2 pts)
2. How does C++ express the equivalent of an "interface" from languages like Java, given that it
   has no `interface` keyword? (2 pts)
3. Given `template <typename T> T maxOf(T a, T b) { return a > b ? a : b; }`, what implicit
   requirement does this place on any type `T` used with it? (2 pts)
4. Why does `maxOf(3, 4.5)` fail to compile without an explicit type argument, even though `3`
   could reasonably convert to `3.0`? (2 pts)
5. Why does a base class with a pure virtual function still need a `virtual` destructor if it is
   ever used polymorphically? (2 pts)

**Answer key available to instructors in the course LMS gradebook (not distributed to students
before the quiz).**
