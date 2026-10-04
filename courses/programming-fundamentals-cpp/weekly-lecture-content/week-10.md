# Week 10 — Lecture Content: Structs

## 1. The Problem with Parallel Arrays
```cpp
// Awkward: three separate arrays that must always be kept in sync by index
std::string names[3] = {"Alice", "Bob", "Cara"};
int ages[3] = {20, 22, 21};
double gpas[3] = {3.8, 3.4, 3.9};
```
If one array is sorted or an element removed without updating the others identically, the data
silently becomes inconsistent. A `struct` groups one record's fields together, so they move and
stay consistent as a unit.

## 2. Defining and Using a `struct`
```cpp
struct Student {
    std::string name;
    int age;
    double gpa;
};

int main() {
    Student s1;
    s1.name = "Alice";
    s1.age = 20;
    s1.gpa = 3.8;

    Student s2 = {"Bob", 22, 3.4};  // aggregate initialization, in member order

    std::cout << s1.name << " is " << s1.age << " with GPA " << s1.gpa << "\n";
}
```
A `struct` defines a new type; `.` accesses a member on a struct value. By default, all members
of a `struct` are `public` (accessible from anywhere) — this is the key difference from `class`,
covered in Week 13.

## 3. Arrays of Structs
```cpp
struct Student {
    std::string name;
    int age;
    double gpa;
};

int main() {
    Student roster[3] = {
        {"Alice", 20, 3.8},
        {"Bob", 22, 3.4},
        {"Cara", 21, 3.9}
    };

    double total = 0;
    for (int i = 0; i < 3; ++i) {
        total += roster[i].gpa;
    }
    std::cout << "Average GPA: " << total / 3 << "\n";
}
```
This is the same array-processing pattern from Week 6, now operating on whole records instead of
single numbers — each `roster[i]` is a complete `Student`.

## 4. Passing Structs to Functions
```cpp
void printStudent(const Student& s) {   // pass by const reference: no copy, cannot modify
    std::cout << s.name << " (" << s.age << "): " << s.gpa << "\n";
}

void raiseGpa(Student& s, double amount) {  // pass by reference: modifies the caller's struct
    s.gpa += amount;
}
```
As with any sizeable parameter, pass structs **by reference** to avoid copying all their fields;
add `const` when the function should only read, not modify, the struct.

## 5. Structs vs. Arrays — When to Use Which
- Use an **array** (or array of structs) when you have many items of the *same kind*.
- Use a **struct** when you have *several related fields describing one thing* (even just one
  instance of it).
- Use an **array of structs** when you have many items, each with several related fields — this
  is the pattern the capstone project will build on.

## 6. In-Class Exercise
Design a `struct Product` with `name`, `price`, and `quantity` fields; write a function
`double totalValue(const Product products[], int size)` that returns the sum of `price * quantity`
over an array of products.
