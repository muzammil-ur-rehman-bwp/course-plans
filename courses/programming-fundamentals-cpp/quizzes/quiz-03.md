# Quiz 3 — Functions & 1D Arrays (Week 6)

**Duration:** 15 minutes | **Format:** 5 short-answer/code questions | **Weight:** part of Quizzes (10%, best 5 of 6 counted)

1. What is the difference between pass-by-value and pass-by-reference? Give one reason to choose
   each. (2 pts)
2. Why does the following function fail to swap the caller's variables?
   ```cpp
   void swap(int a, int b) {
       int temp = a;
       a = b;
       b = temp;
   }
   ```
   What one change fixes it? (2 pts)
3. Given `int values[6] = {4, 8, 1, 9, 3, 7};`, what are the valid indices for this array? (1 pt)
4. Why must the size of an array be passed as a separate parameter to a function, rather than the
   function computing it itself from the array parameter? (2 pts)
5. Write the function signature (not the body) for a function that computes the average of a
   `const` array of `double`s given its size. (3 pts)

**Answer key available to instructors in the course LMS gradebook (not distributed to students
before the quiz).**
