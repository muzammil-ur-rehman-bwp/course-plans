# Week 11 — Lecture Content: File I/O

## 1. Why File I/O?
Every program we've written so far loses all its data the moment it exits — variables, arrays,
and structs live only in memory (RAM) while the process runs. File I/O lets a program **persist**
data to disk so it is still there the next time the program runs.

## 2. Writing to a File
```cpp
#include <fstream>
#include <iostream>

int main() {
    std::ofstream outFile("scores.txt");  // creates/overwrites scores.txt
    if (!outFile.is_open()) {
        std::cout << "Error: could not open file for writing.\n";
        return 1;
    }

    outFile << "Alice 90\n";
    outFile << "Bob 85\n";
    outFile.close();
}
```
Always check `is_open()` (or the stream in a boolean context, `if (!outFile)`) before writing —
the file might not be writable (e.g., permissions, a full disk, or an invalid path).

## 3. Reading from a File
```cpp
#include <fstream>
#include <iostream>
#include <string>

int main() {
    std::ifstream inFile("scores.txt");
    if (!inFile.is_open()) {
        std::cout << "Error: could not open file for reading.\n";
        return 1;
    }

    std::string name;
    int score;
    while (inFile >> name >> score) {   // >> returns false on failure (e.g., end of file)
        std::cout << name << " scored " << score << "\n";
    }
    inFile.close();
}
```
`inFile >> name >> score` reads whitespace-separated tokens, exactly like `std::cin`, but from the
file stream instead of the keyboard. The `while` condition relies on the stream converting to
`false` once it fails to extract a value (including at end-of-file) — this is the standard idiom
for "read until there's nothing left."

## 4. `getline` vs. `>>`
```cpp
std::ifstream inFile("notes.txt");
std::string line;
while (std::getline(inFile, line)) {   // reads one whole line, including spaces
    std::cout << "Line: " << line << "\n";
}
```
Use `>>` when the file is organized as whitespace-separated tokens of known type (numbers, single
words); use `getline` when a line may contain spaces (e.g., a full name or sentence) or when you
want to process the file one line at a time regardless of its internal format.

## 5. Reading Structured Data into a Struct Array
```cpp
struct Student {
    std::string name;
    int score;
};

int main() {
    std::ifstream inFile("scores.txt");
    if (!inFile.is_open()) return 1;

    Student roster[100];
    int count = 0;
    while (count < 100 && inFile >> roster[count].name >> roster[count].score) {
        count++;
    }
    inFile.close();

    for (int i = 0; i < count; ++i) {
        std::cout << roster[i].name << ": " << roster[i].score << "\n";
    }
}
```
This combines three earlier topics directly: structs (Week 10), arrays (Week 6), and loops
(Week 4) — exactly the kind of integration the capstone project will require.

## 6. In-Class Exercise
Write a program that writes 5 `Product` records (name, price) to `products.txt`, then a second
part of the program (or a second run) that reads them back into an array of `Product` structs and
prints the total value.
