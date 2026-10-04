# Week 13 — Lecture Content: Introduction to the STL

> `std::vector` and `std::map` are not special, built-in magic — they are class templates, built
> on exactly the ideas from Weeks 10–11. This week treats them as such, while also introducing the
> one new idea they rely on: iterators.

## 1. `std::vector<T>`: A Growable, Type-Safe Array
```cpp
#include <vector>

std::vector<int> scores;          // starts empty — no fixed size needed up front
scores.push_back(85);
scores.push_back(92);
scores.push_back(78);

std::cout << scores.size() << "\n";    // 3
std::cout << scores[1] << "\n";        // 92 — indexing works like a built-in array
scores.pop_back();                      // removes the last element (78)

for (std::size_t i = 0; i < scores.size(); ++i) {
    std::cout << scores[i] << " ";
}
```
`std::vector<T>` replaces the fixed-size C-style arrays from the prerequisite course with a
container that grows as needed (`push_back`), tracks its own size (`size()`), and still supports
`[]` indexing. It is the default choice for a sequential collection in modern C++ — prefer it over
a raw `new[]`-allocated array (Week 2's course prerequisite material) in essentially all new code.

## 2. `std::map<K, V>`: An Associative Container
```cpp
#include <map>

std::map<std::string, int> inventory;
inventory["apples"] = 50;
inventory["bananas"] = 30;
inventory["apples"] += 10;             // update an existing key

if (inventory.find("bananas") != inventory.end()) {
    std::cout << "Bananas in stock: " << inventory["bananas"] << "\n";
}

if (inventory.count("cherries") == 0) {
    std::cout << "No cherries tracked.\n";
}
```
`std::map<K, V>` stores key-value pairs, keeping them sorted by key, and provides fast lookup by
key instead of by position. `inventory["apples"]` both reads and (if the key doesn't yet exist)
inserts a default-valued entry — a convenient but occasionally surprising behavior; `find()` or
`count()` are the correct way to check for a key's presence **without** accidentally inserting it.

## 3. Iterators: A Uniform Way to Traverse Any Container
```cpp
std::vector<int> scores = {85, 92, 78};

// Explicit iterator loop:
for (std::vector<int>::iterator it = scores.begin(); it != scores.end(); ++it) {
    std::cout << *it << " ";   // *it dereferences the iterator, like a pointer
}

// The same, with std::map:
std::map<std::string, int> inventory = {{"apples", 50}, {"bananas", 30}};
for (std::map<std::string, int>::iterator it = inventory.begin(); it != inventory.end(); ++it) {
    std::cout << it->first << " = " << it->second << "\n";   // it->first/it->second: key/value
}
```
Every STL container exposes `begin()` (an iterator to its first element) and `end()` (a "one past
the last element" marker, never itself dereferenced). An **iterator** behaves like a generalized
pointer — `*it` dereferences it, `++it` advances it — and the *same* `begin()`/`end()`/`*`/`++`
pattern works whether the underlying container is a `std::vector`, a `std::map`, or any other STL
container, regardless of how that container is actually implemented internally.

## 4. Range-`for`: Iterators, With Less Syntax
```cpp
for (int score : scores) {
    std::cout << score << " ";
}

for (const auto& entry : inventory) {           // auto deduces std::pair<const std::string,int>
    std::cout << entry.first << " = " << entry.second << "\n";
}
```
A range-`for` loop uses a container's iterators automatically behind the scenes — it is exactly
equivalent to the explicit iterator loops in section 3, just without writing out `begin()`/`end()`
/`*`/`++` by hand. Prefer range-`for` whenever you don't need the iterator itself (e.g. to erase an
element or remember a position); reach for an explicit iterator when you do.

## 5. `std::vector`/`std::map` Are Templates You Already Understand
```cpp
// Conceptually, std::vector is shaped like:
template <typename T>
class vector {
public:
    void push_back(const T& value);
    T& operator[](std::size_t index);
    std::size_t size() const;
    // iterator begin(); iterator end(); ...
};
```
Nothing here is a new language feature — `std::vector<T>` is a class template exactly like last
week's `Stack<T>`, just far more thoroughly implemented, tested, and optimized than anything built
in a few weeks of coursework should aim to be. This is also the practical lesson: once the
standard library already provides a correct, efficient generic container, prefer it over writing
your own, and reserve hand-written templates (Week 11) for cases the STL genuinely doesn't cover.

## 6. In-Class Exercise
Read a list of student names and scores into a `std::vector<std::pair<std::string, int>>` (or a
small struct of your own), then build a `std::map<std::string, int>` from it, and print every
entry in sorted-by-name order using a range-`for` loop over the map — note that `std::map`
iterates in key order automatically, with no sorting code required.
