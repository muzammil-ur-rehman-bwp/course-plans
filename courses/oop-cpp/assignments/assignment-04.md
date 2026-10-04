# Assignment 4 — Exception Handling, Introduction to the STL (Weeks 12–13)

**Weight:** 5% of course grade (one of 4 assignments, 20% total) | **Assigned:** Week 13 | **Due:** Start of Week 15

## Instructions
Submit a single source file `assignment04.cpp`. Code must compile cleanly with
`g++ -std=c++17 -Wall`.

## Questions
1. **(Custom exceptions, 20 pts)** Write `class InvalidTransactionError : public
   std::runtime_error` carrying a `double attemptedAmount_` and a `getAttemptedAmount() const`
   method. Write `class Account` (reusing or adapting earlier work) whose `withdraw(double)`
   throws it when the amount exceeds the balance, and whose `deposit(double)` throws
   `std::invalid_argument` for a non-positive amount.
2. **(Ordered `catch` clauses, 15 pts)** Write a `try` block exercising both failure modes from
   Question 1, with ordered `catch` clauses (`InvalidTransactionError` first, then
   `std::invalid_argument`, then a general `std::exception` catch-all), all catching by
   reference, each printing a distinct, informative message.
3. **(`std::vector` of accounts, 20 pts)** Store several `Account` objects in a `std::vector<Account>`
   representing a small bank's customers; write a function `double totalAssets(const
   std::vector<Account>& accounts)` that sums every account's balance using an iterator-based
   loop (not `[]` indexing).
4. **(`std::map` lookup, 25 pts)** Write a `std::map<std::string, Account>` keyed by account
   holder name (or account number, your choice, stated in a comment); write a function `bool
   transferFunds(std::map<std::string, Account>& accounts, const std::string& fromKey, const
   std::string& toKey, double amount)` that looks up both accounts (correctly handling a missing
   key without inserting a spurious default entry — recall `find()`/`count()` from lecture),
   calls `withdraw`/`deposit`, and correctly propagates or catches any exception thrown by either
   call, returning `false` on failure without leaving the accounts partially modified.
5. **(Integration, 20 pts)** In `main`, build the `std::map` of accounts, run `totalAssets`
   before and after several successful and several deliberately-failing `transferFunds` calls,
   and confirm (by printing `totalAssets`'s result) that total assets are conserved across every
   successful transfer and unchanged across every failed one.

## Submission
Upload `assignment04.cpp` via the course submission system. Late policy per syllabus
(`course-plan.md` §7).
