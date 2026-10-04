# Week 4 — Lecture Content: Loops — `for`, `while`, `do-while`

## 1. The `for` Loop
Best when the number of iterations is known in advance (counter-controlled):
```cpp
int sum = 0;
for (int i = 1; i <= 10; ++i) {
    sum += i;
}
std::cout << "Sum 1..10 = " << sum << "\n";  // 55
```
The three parts — initialization, condition, update — run in a fixed order: initialize once,
check the condition, run the body if true, run the update, re-check the condition, and so on.

## 2. The `while` Loop
Best when the number of iterations is not known in advance (sentinel-controlled):
```cpp
int value;
int total = 0;
std::cout << "Enter numbers, 0 to stop:\n";
std::cin >> value;
while (value != 0) {
    total += value;
    std::cin >> value;
}
std::cout << "Total = " << total << "\n";
```
The condition is checked **before** each iteration, so if the first input is `0`, the body never
runs at all.

## 3. The `do-while` Loop
Guarantees the body runs **at least once**, because the condition is checked **after** the body:
```cpp
int choice;
do {
    std::cout << "1) Add  2) Remove  3) Quit\nChoice: ";
    std::cin >> choice;
    // handle choice ...
} while (choice != 3);
```
This matches menu-driven programs well — you always want to show the menu at least once before
checking whether the user wants to quit.

## 4. `break`, `continue`, and Nested Loops
```cpp
for (int row = 1; row <= 5; ++row) {
    for (int col = 1; col <= 5; ++col) {
        if (col > row) continue;      // skip printing above the diagonal
        std::cout << "* ";
    }
    std::cout << "\n";
}
```
- `break` exits the nearest enclosing loop immediately.
- `continue` skips the rest of the current iteration and proceeds to the next one.
- Nested loops (a loop inside a loop) are the standard tool for anything grid-shaped — this
  previews 2D arrays in Week 7.

## 5. Common Pitfalls
```cpp
// Off-by-one: this loop runs 0..9, not 1..10
for (int i = 0; i <= 10; ++i) { /* runs 11 times, not 10 — check your bounds */ }

// Infinite loop: forgot to update the loop variable
int i = 0;
while (i < 10) {
    std::cout << i;
    // missing i++  -> this never terminates
}
```
Before running a loop, ask: what are the exact start and stop bounds, and is there a line in the
body that guarantees progress toward the stop condition?

## 6. In-Class Exercise
Write a program using a `while` loop that reads integers from the user until a negative number is
entered, then prints the count and average of the non-negative numbers entered (handle the
"no numbers entered" case without dividing by zero).
