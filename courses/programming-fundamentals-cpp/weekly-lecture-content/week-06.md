# Week 6 — Lecture Content: Arrays (1D)

## 1. Declaring and Initializing Arrays
```cpp
int scores[5];                         // 5 ints, uninitialized (indeterminate values)
int grades[5] = {90, 85, 70, 60, 95};  // initialized, size inferred from the list
double temps[] = {98.6, 101.2, 99.1};  // size inferred as 3
```
An array is a fixed-size, contiguous block of elements of the same type. Once declared, its size
cannot change.

## 2. Indexing
```cpp
std::cout << grades[0] << "\n";  // 90 — indices start at 0
grades[2] = 75;                   // overwrite the third element
```
Valid indices for an array of size `n` are `0` through `n - 1`. **C++ performs no automatic bounds
checking** — reading or writing `grades[5]` on a 5-element array compiles and may run, but it
accesses memory outside the array: undefined behavior that can silently corrupt other data or
crash the program. There is no runtime safety net here; the programmer must get the bounds right.

## 3. Iterating Over an Array
```cpp
int grades[5] = {90, 85, 70, 60, 95};
int sum = 0;
for (int i = 0; i < 5; ++i) {
    sum += grades[i];
}
double average = static_cast<double>(sum) / 5;
std::cout << "Average = " << average << "\n";
```
Finding the maximum:
```cpp
int maxValue = grades[0];
int maxIndex = 0;
for (int i = 1; i < 5; ++i) {
    if (grades[i] > maxValue) {
        maxValue = grades[i];
        maxIndex = i;
    }
}
```

## 4. Passing Arrays to Functions
An array passed to a function **decays to a pointer to its first element** — the function does
not automatically know the size, so the size must be passed alongside it:
```cpp
double average(const int values[], int size) {
    int sum = 0;
    for (int i = 0; i < size; ++i) {
        sum += values[i];
    }
    return static_cast<double>(sum) / size;
}

int main() {
    int grades[5] = {90, 85, 70, 60, 95};
    std::cout << average(grades, 5) << "\n";
}
```
`const int values[]` signals that the function only reads the array; it will not modify the
caller's data. Since the function cannot resize or reassign the array itself (it only has a
pointer to the caller's data), modifications inside the function to the *elements* do persist —
there is no copy being made, unlike pass-by-value for primitives.

## 5. `std::array` (Brief Preview)
The C++ standard library offers `std::array<int, 5>`, a fixed-size array type that knows its own
size (`.size()`) and can be bounds-checked with `.at()`. This course uses raw arrays (`int[]`)
throughout, as they are the foundation other languages' and C++'s own `std::vector` build on, but
be aware `std::array` exists as a safer alternative you will likely meet in later courses.

## 6. In-Class Exercise
Write a function `int countAbove(const int values[], int size, int threshold)` that returns how
many elements exceed `threshold`, and call it on an array of 10 test scores.
