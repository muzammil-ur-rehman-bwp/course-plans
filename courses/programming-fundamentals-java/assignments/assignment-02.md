# Assignment 2 — Methods, Arrays, Strings, Objects, Classes, `ArrayList` (Weeks 5–10)

**Weight:** 5% of course grade (one of 4 assignments, 20% total) | **Assigned:** Week 10 | **Due:** Start of Week 12

## Instructions
Submit a single source file `Assignment02.java`. Code must compile cleanly with `javac`.

## Questions
1. **(Methods/overloading, 15 pts)** Write an overloaded pair `static double discount(double
   price, double percentOff)` and `static double discount(double price, double percentOff,
   double maxDiscount)` (the second caps the discount amount at `maxDiscount`). Demonstrate both
   overloads being called.
2. **(Arrays, 20 pts)** Write `static int removeDuplicates(int[] values, int size)` that removes
   duplicate integers from an array in place, returning the new, shorter logical length (the
   array itself stays the same physical size, but only the first `size` returned elements are
   meaningful). Test on an array with several duplicates and print the array's meaningful
   portion before and after.
3. **(Strings, 15 pts)** Write `static boolean isPalindrome(String s)` that returns whether `s`
   reads the same forwards and backwards, ignoring case (use `.equalsIgnoreCase` or
   `.toLowerCase()`, plus `StringBuilder`'s `.reverse()` or manual character comparison — not
   `==`). Test it on at least 4 strings, including one with mixed case.
4. **(Classes, 25 pts)** Define a class `Employee` with private `String name` and `double
   salary` fields, a constructor, and a method `giveRaise(double percent)` that increases
   `salary` by that percentage. Demonstrate creating an `Employee`, calling `giveRaise`, and
   printing the updated salary.
5. **(`ArrayList`, 25 pts)** Write a `main` that builds an `ArrayList<Employee>` of at least 3
   `Employee` objects (reusing Question 4's class), then prints the name and salary of whichever
   employee has the highest salary, found by iterating the list.

## Submission
Upload `Assignment02.java` via the course submission system. Late policy per syllabus
(`course-plan.md` §7).
