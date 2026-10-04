# Assignment 1 — C++ Basics, Operators, Control Flow, Loops (Weeks 1–4)

**Weight:** 5% of course grade (one of 4 assignments, 20% total) | **Assigned:** Week 4 | **Due:** Start of Week 6

## Instructions
Submit a single source file `assignment01.cpp` containing a separate, clearly commented function
for each question below, called from `main` with the specified test inputs. Code must compile
cleanly with `g++ -std=c++17 -Wall`.

## Questions
1. **(Basics, 15 pts)** Write a function `double circleArea(double radius)` that returns the
   area of a circle (`π * r²`, use `3.14159` or `<cmath>`'s `M_PI`). Call it for radii `2.0`,
   `5.5`, and `0.0`, printing each labeled result.
2. **(Operators, 20 pts)** Write a function `void describeNumber(int n)` that prints whether `n`
   is even or odd (using `%`), and whether it is positive, negative, or zero, using a single
   function with no more than two `if` statements combined using logical operators. Call it for
   at least 4 test values covering all combinations.
3. **(Control flow, 20 pts)** Write a function `char letterGrade(double score)` that returns the
   letter grade for a 0–100 score (A ≥ 85, B ≥ 70, C ≥ 55, D ≥ 40, F otherwise) using an
   `if`/`else if`/`else` chain. Test it on at least one score in each band, plus the exact
   boundary values (85, 70, 55, 40).
4. **(Loops, 20 pts)** Write a function `int countPrimesBelow(int limit)` that returns how many
   prime numbers are less than `limit`, using nested loops (outer loop over candidates, inner
   loop testing divisibility). Test it for `limit = 10` (expect 4: 2, 3, 5, 7) and `limit = 50`.
5. **(Integration, 25 pts)** Write a `main` that: reads a list of up to 20 scores from the user
   (sentinel-terminated by entering `-1`), computes and prints the average, and prints the letter
   grade (reusing Question 3's function) for the average. Handle the "no scores entered" edge
   case without dividing by zero.

## Submission
Upload `assignment01.cpp` via the course submission system. Late policy per syllabus
(`course-plan.md` §7).
