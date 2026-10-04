# Week 11 — Lecture Content: Templates II — Class Templates

## 1. From Function Templates to Class Templates
Last week's function templates generalized a single function over a type parameter. A **class
template** generalizes an entire class — every member function, every data member's type — over
one or more type parameters, letting you write one container (e.g. a stack) that works correctly
for `int`, `std::string`, or any other type, without rewriting it.

## 2. `template <typename T> class`: A Generic `Stack`
```cpp
template <typename T>
class Stack {
public:
    void push(const T& value) { data_.push_back(value); }

    void pop() {
        if (!data_.empty()) {
            data_.pop_back();
        }
    }

    const T& top() const { return data_.back(); }

    bool empty() const { return data_.empty(); }

    std::size_t size() const { return data_.size(); }

private:
    std::vector<T> data_;   // reuse std::vector<T> (itself a class template) internally
};

Stack<int> intStack;
intStack.push(1);
intStack.push(2);
std::cout << intStack.top() << "\n";   // 2
intStack.pop();

Stack<std::string> stringStack;
stringStack.push("hello");
stringStack.push("world");
std::cout << stringStack.top() << "\n";  // world
```
`Stack<int>` and `Stack<std::string>` are two entirely separate, independently-generated classes
— the compiler instantiates a distinct `Stack` for each `T` actually used in the program. Nothing
about using `int` in one and `std::string` in the other conflicts, because after instantiation
they share no code at runtime, only the template definition they were generated from.

## 3. Defining Member Functions Outside the Class Body
```cpp
template <typename T>
class Stack {
public:
    void push(const T& value);
    const T& top() const;
    bool empty() const;

private:
    std::vector<T> data_;
};

// Out-of-class definitions need the template header repeated, and Stack<T>:: as the scope:
template <typename T>
void Stack<T>::push(const T& value) {
    data_.push_back(value);
}

template <typename T>
const T& Stack<T>::top() const {
    return data_.back();
}

template <typename T>
bool Stack<T>::empty() const {
    return data_.empty();
}
```
Just like an ordinary class, a template class's member functions can be declared in the class
body and defined separately. The syntax requires the `template <typename T>` header again before
each definition, and the scope qualifier is `Stack<T>::`, not just `Stack::`. In practice, small
template classes are very often defined entirely inside the class body (as in section 2) to avoid
this repetition — both styles are correct.

## 4. Multiple Type Parameters: `Pair<T, U>`
```cpp
template <typename T, typename U>
class Pair {
public:
    Pair(const T& first, const U& second) : first_(first), second_(second) {}

    const T& getFirst() const { return first_; }
    const U& getSecond() const { return second_; }

private:
    T first_;
    U second_;
};

Pair<std::string, int> entry("apples", 42);
std::cout << entry.getFirst() << " -> " << entry.getSecond() << "\n";  // apples -> 42

Pair<int, int> point(3, 4);   // T and U can be the same type too
```
A class template can take more than one type parameter, each independent — `Pair<std::string,
int>` and `Pair<int, int>` are both valid instantiations of the same `Pair<T, U>` template, with
`T` and `U` substituted however the user needs. This is exactly the pattern the standard library's
own `std::pair<T1, T2>` and, as we'll see next week, `std::map<K, V>` are built on.

## 5. A Template Class Can Still Use Everything You Already Know
```cpp
template <typename T>
class Stack {
public:
    Stack() = default;
    Stack(const Stack& other) : data_(other.data_) {}   // copy constructor — still applies!

    void push(const T& value) { data_.push_back(value); }
    // ... as before ...

private:
    std::vector<T> data_;
};
```
Everything from earlier in the semester — constructors, destructors, the Rule of Three, operator
overloading — applies equally to template classes; `T` is just a placeholder for whatever
concrete type gets substituted in. A `Stack<T>`'s compiler-generated copy constructor here is
correct because `std::vector<T>`'s own copy constructor already deep-copies correctly, for any
`T` that is itself copyable.

## 6. In-Class Exercise
Implement `template <typename T, typename U> class Pair` as shown, then extend it with an
`operator==` that compares two `Pair<T, U>` objects for equality (requires `T` and `U` to support
`==`, an implicit constraint as in Week 10). Test it with at least two different `Pair`
instantiations in the same `main`.
