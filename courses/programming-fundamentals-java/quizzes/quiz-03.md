# Quiz 3 — Methods & 1D Arrays (Week 6)

**Duration:** 15 minutes | **Format:** 5 short-answer/code questions | **Weight:** part of Quizzes (10%, best 5 of 6 counted)

1. Java is always pass-by-value. Explain what is actually copied when an array is passed as a
   method parameter, and why that allows a method to mutate the array's contents. (2 pts)
2. Why does the following method fail to make `x` equal to `10` in the caller?
   ```java
   static void setToTen(int x) {
       x = 10;
   }
   ```
   What would need to change for a method to be able to affect a caller's data this way? (2 pts)
3. Given `int[] values = {4, 8, 1, 9, 3, 7};`, what are the valid indices for this array? (1 pt)
4. What exception is thrown, and under what condition, when an array index is out of range?
   (2 pts)
5. Write the method signature (not the body) for a method that computes the average of an
   `int[]` given no other parameters (the array itself provides its own length). (3 pts)

**Answer key available to instructors in the course LMS gradebook (not distributed to students
before the quiz).**
