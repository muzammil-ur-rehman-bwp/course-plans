# Quiz 2 — Operators & Control Flow (Week 4)

**Duration:** 15 minutes | **Format:** 5 short-answer/code questions | **Weight:** part of Quizzes (10%, best 5 of 6 counted)

1. What is the value of `9 / 2` and `9 % 2` in C++? (2 pts)
2. Rewrite `static_cast<double>(a) / b` using a C-style cast instead, then explain in one
   sentence why `static_cast` is generally preferred. (2 pts)
3. Given `int x = 7;`, what does the following print, and why?
   ```cpp
   if (x = 5) {
       std::cout << "yes";
   } else {
       std::cout << "no";
   }
   ```
   (2 pts)
4. Convert this `if`/`else if` chain into an equivalent `switch` statement:
   ```cpp
   if (choice == 1) std::cout << "Add";
   else if (choice == 2) std::cout << "Subtract";
   else std::cout << "Unknown";
   ```
   (2 pts)
5. Write a `for` loop header (just the header, not the body) that iterates `i` from `10` down to
   `1`, inclusive. (2 pts)

**Answer key available to instructors in the course LMS gradebook (not distributed to students
before the quiz).**
