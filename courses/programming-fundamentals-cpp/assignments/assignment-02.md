# Assignment 2 — Functions, Arrays, Pointers, Dynamic Memory, Structs (Weeks 5–10)

**Weight:** 5% of course grade (one of 4 assignments, 20% total) | **Assigned:** Week 10 | **Due:** Start of Week 12

## Instructions
Submit a single source file `assignment02.cpp`. Code must compile cleanly with
`g++ -std=c++17 -Wall` and must correctly free every dynamic allocation it makes.

## Questions
1. **(Functions, 15 pts)** Write an overloaded pair `double discount(double price, double
   percentOff)` and `double discount(double price, double percentOff, double maxDiscount)`
   (the second caps the discount amount at `maxDiscount`). Demonstrate both overloads being
   called.
2. **(Arrays, 20 pts)** Write `void removeDuplicates(int values[], int& size)` that removes
   duplicate integers from an array in place, updating `size` (passed by reference) to the new,
   shorter length. Test on an array with several duplicates and print the array before and after.
3. **(Pointers, 15 pts)** Write `void findMinMax(const int values[], int size, int* minOut, int*
   maxOut)` that writes the array's minimum and maximum through the two output pointers. Test it
   on at least two different arrays.
4. **(Dynamic memory, 25 pts)** Write a function `int* buildSquares(int n)` that dynamically
   allocates an array of `n` ints, fills it with the squares of `1..n`, and returns the pointer.
   In `main`, call it, print the result, then free the memory correctly. Explain in a comment why
   the caller (not the function) is responsible for freeing memory the function allocated.
5. **(Structs, 25 pts)** Define `struct Employee { std::string name; double salary; };`. Write
   `Employee giveRaise(Employee e, double percent)` that returns a *new* `Employee` with an
   increased salary (does not modify the original), and demonstrate in `main` that the original
   `Employee` passed in is unchanged after the call.

## Submission
Upload `assignment02.cpp` via the course submission system. Late policy per syllabus
(`course-plan.md` §7).
