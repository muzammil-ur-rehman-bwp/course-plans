# Week 7 — Lecture Content: 2D Arrays and Strings

## 1. Declaring and Indexing 2D Arrays
```cpp
int grid[3][4];                 // 3 rows, 4 columns
int matrix[2][3] = {{1, 2, 3}, {4, 5, 6}};
std::cout << matrix[1][2] << "\n";  // 6 — row 1, column 2
```
C++ stores 2D arrays in **row-major order**: all of row 0's elements, then all of row 1's, and so
on, contiguously in memory. Indexing is `[row][column]`, and (as with 1D arrays) there is no
automatic bounds checking.

## 2. Processing a 2D Array with Nested Loops
```cpp
int matrix[2][3] = {{1, 2, 3}, {4, 5, 6}};

for (int row = 0; row < 2; ++row) {
    int rowSum = 0;
    for (int col = 0; col < 3; ++col) {
        rowSum += matrix[row][col];
    }
    std::cout << "Row " << row << " sum = " << rowSum << "\n";
}
```
The outer loop walks rows, the inner loop walks columns within the current row — the same
nested-loop pattern introduced in Week 4, now applied to real grid data.

## 3. C-Strings
A C-string is a `char` array terminated by a null character `'\0'`:
```cpp
#include <cstring>

char name[20] = "Alice";              // compiler appends '\0' automatically
std::cout << strlen(name) << "\n";    // 5 (does not count the '\0')

char a[] = "cat", b[] = "dog";
if (strcmp(a, b) == 0) {              // strcmp returns 0 only if equal
    std::cout << "Same\n";
}
```
C-strings require manual size management and are easy to misuse (e.g., forgetting the null
terminator, or overflowing a fixed buffer with `strcpy`). They appear in this course mainly so you
can recognize and read legacy C-style code.

## 4. `std::string`
```cpp
#include <string>

std::string name = "Alice";
std::string greeting = "Hello, " + name + "!";   // concatenation with +
std::cout << greeting.length() << "\n";           // length, no manual null-terminator handling
std::cout << name[0] << "\n";                      // 'A' — indexing works like an array
std::cout << greeting.substr(7, 5) << "\n";        // "Alice" — substring(start, length)

if (name == "Alice") {                              // == compares contents, not addresses
    std::cout << "Matched\n";
}

size_t pos = greeting.find("Hello");                // returns std::string::npos if not found
if (pos != std::string::npos) {
    std::cout << "Found at index " << pos << "\n";
}
```

## 5. Why Prefer `std::string`?
| | C-string (`char[]`) | `std::string` |
|---|---|---|
| Size management | Manual, fixed buffer | Automatic, grows as needed |
| Comparison | `strcmp` (easy to misuse `==`, which compares addresses) | `==` compares contents directly |
| Concatenation | Manual with `strcat` (buffer-overflow risk) | `+` operator, safe |
| Safety | No bounds checking | Still no bounds checking on `[]`, but far fewer manual pitfalls |

Modern C++ code uses `std::string` by default and reaches for C-strings only when interoperating
with older C APIs.

## 6. In-Class Exercise
Write a function `bool isPalindrome(const std::string& text)` that checks whether a string reads
the same forwards and backwards (ignore case), then test it on `"Racecar"` and `"Hello"`.
