# C++ Templates — Practice Tasks

A progressive set of exercises to master C++ templates, from the basics to advanced techniques.

---

## Task 1 — Function Template: Maximum

Write a function template `maxVal` that takes two arguments of the same type and returns the larger one.

```cpp
// Expected usage:
std::cout << maxVal(3, 7);       // 7
std::cout << maxVal(3.14, 2.71); // 3.14
```

<details>
<summary>Solution</summary>

```cpp
#include <iostream>

template <typename T>
T maxVal(T a, T b) {
    return (a > b) ? a : b;
}

int main() {
    std::cout << maxVal(3, 7) << "\n";
    std::cout << maxVal(3.14, 2.71) << "\n";
    std::cout << maxVal('a', 'z') << "\n";
}
```

</details>

---

## Task 2 — Function Template: Swap

Write a function template `mySwap` that swaps two variables of any type.

```cpp
int a = 1, b = 2;
mySwap(a, b); // a == 2, b == 1
```

<details>
<summary>Solution</summary>

```cpp
#include <iostream>

template <typename T>
void mySwap(T& a, T& b) {
    T temp = a;
    a = b;
    b = temp;
}

int main() {
    int a = 1, b = 2;
    mySwap(a, b);
    std::cout << a << " " << b << "\n"; // 2 1

    std::string s1 = "hello", s2 = "world";
    mySwap(s1, s2);
    std::cout << s1 << " " << s2 << "\n"; // world hello
}
```

</details>

---

## Task 3 — Function Template: Array Sum

Write a function template that takes a C-style array and its size, and returns the sum of all elements.

```cpp
int arr[] = {1, 2, 3, 4, 5};
std::cout << arraySum(arr, 5); // 15
```

<details>
<summary>Solution</summary>

```cpp
#include <iostream>

template <typename T>
T arraySum(const T arr[], int size) {
    T sum = T{};
    for (int i = 0; i < size; ++i)
        sum += arr[i];
    return sum;
}

int main() {
    int arr[] = {1, 2, 3, 4, 5};
    std::cout << arraySum(arr, 5) << "\n"; // 15

    double darr[] = {1.1, 2.2, 3.3};
    std::cout << arraySum(darr, 3) << "\n"; // 6.6
}
```

</details>

---

## Task 4 — Function Template with Two Type Parameters

Write a function template `multiply` that takes two parameters of potentially different types and returns their product. The return type should be deduced automatically.

```cpp
std::cout << multiply(3, 4.5); // 13.5
```

<details>
<summary>Solution</summary>

```cpp
#include <iostream>

template <typename T, typename U>
auto multiply(T a, U b) {
    return a * b;
}

int main() {
    std::cout << multiply(3, 4.5) << "\n";   // 13.5
    std::cout << multiply(2.5, 4) << "\n";   // 10
    std::cout << multiply(10, 20) << "\n";    // 200
}
```

</details>

---

## Task 5 — Simple Class Template

Create a class template `Box<T>` that stores a single value of type `T` and has `get()` and `set()` methods.

```cpp
Box<int> b(42);
std::cout << b.get(); // 42
b.set(100);
```

<details>
<summary>Solution</summary>

```cpp
#include <iostream>

template <typename T>
class Box {
    T value;
public:
    Box(T val) : value(val) {}
    T get() const { return value; }
    void set(T val) { value = val; }
};

int main() {
    Box<int> b(42);
    std::cout << b.get() << "\n";
    b.set(100);
    std::cout << b.get() << "\n";

    Box<std::string> sb("hello");
    std::cout << sb.get() << "\n";
}
```

</details>

---

## Task 6 — Class Template: Pair

Implement a class template `Pair<T, U>` that stores two values of possibly different types with `first()` and `second()` accessors.

<details>
<summary>Solution</summary>

```cpp
#include <iostream>
#include <string>

template <typename T, typename U>
class Pair {
    T first_;
    U second_;
public:
    Pair(T f, U s) : first_(f), second_(s) {}
    T first() const { return first_; }
    U second() const { return second_; }
};

int main() {
    Pair<int, std::string> p(1, "one");
    std::cout << p.first() << " -> " << p.second() << "\n";

    Pair<double, double> coords(3.14, 2.71);
    std::cout << coords.first() << ", " << coords.second() << "\n";
}
```

</details>

---

## Task 7 — Non-Type Template Parameter

Write a function template `printArray` that takes a reference to a C-style array where the size is a non-type template parameter, so you don't need to pass the size explicitly.

```cpp
int arr[] = {10, 20, 30};
printArray(arr); // prints: 10 20 30
```

<details>
<summary>Solution</summary>

```cpp
#include <iostream>

template <typename T, std::size_t N>
void printArray(const T (&arr)[N]) {
    for (std::size_t i = 0; i < N; ++i)
        std::cout << arr[i] << " ";
    std::cout << "\n";
}

int main() {
    int arr[] = {10, 20, 30};
    printArray(arr); // 10 20 30

    double darr[] = {1.1, 2.2, 3.3, 4.4};
    printArray(darr); // 1.1 2.2 3.3 4.4
}
```

</details>

---

## Task 8 — Class Template with Non-Type Parameter: Static Array

Create a class template `StaticArray<T, N>` that wraps a fixed-size array with `get(index)`, `set(index, value)`, and `size()` methods.

<details>
<summary>Solution</summary>

```cpp
#include <iostream>
#include <stdexcept>

template <typename T, std::size_t N>
class StaticArray {
    T data[N]{};
public:
    T get(std::size_t index) const {
        if (index >= N) throw std::out_of_range("Index out of range");
        return data[index];
    }
    void set(std::size_t index, T value) {
        if (index >= N) throw std::out_of_range("Index out of range");
        data[index] = value;
    }
    constexpr std::size_t size() const { return N; }
};

int main() {
    StaticArray<int, 5> arr;
    arr.set(0, 10);
    arr.set(1, 20);
    std::cout << arr.get(0) << "\n"; // 10
    std::cout << arr.size() << "\n"; // 5
}
```

</details>

---

## Task 9 — Default Template Arguments

Write a class template `Container<T, size_t N = 10>` that holds up to `N` elements with `push()` and `print()` methods. Default capacity should be 10.

<details>
<summary>Solution</summary>

```cpp
#include <iostream>
#include <stdexcept>

template <typename T, std::size_t N = 10>
class Container {
    T data[N]{};
    std::size_t count = 0;
public:
    void push(T value) {
        if (count >= N) throw std::overflow_error("Container is full");
        data[count++] = value;
    }
    void print() const {
        for (std::size_t i = 0; i < count; ++i)
            std::cout << data[i] << " ";
        std::cout << "\n";
    }
};

int main() {
    Container<int> c;        // default capacity 10
    c.push(1); c.push(2); c.push(3);
    c.print(); // 1 2 3

    Container<double, 3> small;
    small.push(1.1); small.push(2.2); small.push(3.3);
    small.print(); // 1.1 2.2 3.3
}
```

</details>

---

## Task 10 — Template Specialization (Full)

Write a function template `printValue(T val)` that prints any value, but **fully specialize** it for `bool` so it prints `"true"` or `"false"` instead of `1` or `0`.

<details>
<summary>Solution</summary>

```cpp
#include <iostream>
#include <string>

template <typename T>
void printValue(T val) {
    std::cout << val << "\n";
}

template <>
void printValue<bool>(bool val) {
    std::cout << (val ? "true" : "false") << "\n";
}

int main() {
    printValue(42);         // 42
    printValue(3.14);       // 3.14
    printValue(true);       // true
    printValue(false);      // false
    printValue(std::string("hello")); // hello
}
```

</details>

---

## Task 11 — Partial Template Specialization

Create a class template `Storage<T>` with a method `describe()`. Partially specialize it for pointer types `Storage<T*>` so that `describe()` indicates it's storing a pointer.

<details>
<summary>Solution</summary>

```cpp
#include <iostream>

template <typename T>
class Storage {
    T value;
public:
    Storage(T val) : value(val) {}
    void describe() const {
        std::cout << "Storing value: " << value << "\n";
    }
};

template <typename T>
class Storage<T*> {
    T* ptr;
public:
    Storage(T* p) : ptr(p) {}
    void describe() const {
        std::cout << "Storing pointer to value: " << *ptr << "\n";
    }
};

int main() {
    Storage<int> s1(42);
    s1.describe(); // Storing value: 42

    int x = 99;
    Storage<int*> s2(&x);
    s2.describe(); // Storing pointer to value: 99
}
```

</details>

---

## Task 12 — Member Function Templates

Create a regular (non-template) class `Printer` that has a **member function template** `print(T val)` which prints any value passed to it.

<details>
<summary>Solution</summary>

```cpp
#include <iostream>
#include <string>

class Printer {
public:
    template <typename T>
    void print(T val) const {
        std::cout << val << "\n";
    }
};

int main() {
    Printer p;
    p.print(42);
    p.print(3.14);
    p.print(std::string("hello"));
    p.print('A');
}
```

</details>

---

## Task 13 — Variadic Function Template: Print All

Write a variadic function template `printAll(args...)` that prints all its arguments separated by spaces.

```cpp
printAll(1, 2.5, "hello", 'A');
// Output: 1 2.5 hello A
```

<details>
<summary>Solution</summary>

```cpp
#include <iostream>

// Base case
void printAll() {
    std::cout << "\n";
}

template <typename T, typename... Args>
void printAll(T first, Args... rest) {
    std::cout << first;
    if constexpr (sizeof...(rest) > 0)
        std::cout << " ";
    printAll(rest...);
}

int main() {
    printAll(1, 2.5, "hello", 'A');
    printAll(42);
    printAll("one", "two", "three");
}
```

</details>

---

## Task 14 — Variadic Template: Sum

Write a variadic function template `sum(args...)` that returns the sum of all arguments.

```cpp
std::cout << sum(1, 2, 3, 4, 5); // 15
std::cout << sum(1.5, 2.5);      // 4.0
```

<details>
<summary>Solution</summary>

```cpp
#include <iostream>

template <typename T>
T sum(T val) {
    return val;
}

template <typename T, typename... Args>
auto sum(T first, Args... rest) {
    return first + sum(rest...);
}

int main() {
    std::cout << sum(1, 2, 3, 4, 5) << "\n";  // 15
    std::cout << sum(1.5, 2.5) << "\n";         // 4
    std::cout << sum(1, 2.5, 3) << "\n";        // 6.5
}
```

</details>

---

## Task 15 — Fold Expressions (C++17)

Rewrite the `sum` from Task 14 using a **fold expression** instead of recursion.

<details>
<summary>Solution</summary>

```cpp
#include <iostream>

template <typename... Args>
auto sum(Args... args) {
    return (args + ...); // unary right fold
}

int main() {
    std::cout << sum(1, 2, 3, 4, 5) << "\n"; // 15
    std::cout << sum(1.5, 2.5) << "\n";       // 4
    std::cout << sum(10) << "\n";              // 10
}
```

</details>

---

## Task 16 — SFINAE: Enable If Arithmetic

Write a function template `half(T val)` that only compiles for arithmetic types (int, float, double, etc.) using `std::enable_if`.

```cpp
half(10);    // OK → 5
half(3.0);   // OK → 1.5
// half("hi"); // Should NOT compile
```

<details>
<summary>Solution</summary>

```cpp
#include <iostream>
#include <type_traits>

template <typename T>
typename std::enable_if<std::is_arithmetic<T>::value, T>::type
half(T val) {
    return val / 2;
}

int main() {
    std::cout << half(10) << "\n";   // 5
    std::cout << half(3.0) << "\n";  // 1.5

    // half(std::string("hi")); // compile error
}
```

</details>

---

## Task 17 — `if constexpr` Dispatch

Write a function template `describe(T val)` that uses `if constexpr` to print different messages depending on whether `T` is integral, floating-point, or something else.

<details>
<summary>Solution</summary>

```cpp
#include <iostream>
#include <type_traits>
#include <string>

template <typename T>
void describe(T val) {
    if constexpr (std::is_integral_v<T>) {
        std::cout << val << " is an integer type\n";
    } else if constexpr (std::is_floating_point_v<T>) {
        std::cout << val << " is a floating-point type\n";
    } else {
        std::cout << val << " is some other type\n";
    }
}

int main() {
    describe(42);                      // 42 is an integer type
    describe(3.14);                    // 3.14 is a floating-point type
    describe(std::string("hello"));    // hello is some other type
    describe(true);                    // 1 is an integer type
}
```

</details>

---

## Task 18 — Template Template Parameters

Write a function template `fillAndPrint` that accepts a **template template parameter** for the container type (e.g., `std::vector` or `std::deque`) and fills it with values 1 through 5, then prints them.

<details>
<summary>Solution</summary>

```cpp
#include <iostream>
#include <vector>
#include <deque>
#include <list>

template <template <typename, typename> class Container, typename T>
void fillAndPrint() {
    Container<T, std::allocator<T>> c;
    for (T i = 1; i <= 5; ++i)
        c.push_back(i);
    for (const auto& val : c)
        std::cout << val << " ";
    std::cout << "\n";
}

int main() {
    fillAndPrint<std::vector, int>();   // 1 2 3 4 5
    fillAndPrint<std::deque, double>(); // 1 2 3 4 5
    fillAndPrint<std::list, int>();     // 1 2 3 4 5
}
```

</details>

---

## Task 19 — Type Traits: Custom `is_same`

Implement your own version of `std::is_same` called `IsSame<T, U>` using template specialization. It should have a static `value` member that is `true` if `T` and `U` are the same type.

<details>
<summary>Solution</summary>

```cpp
#include <iostream>

template <typename T, typename U>
struct IsSame {
    static constexpr bool value = false;
};

template <typename T>
struct IsSame<T, T> {
    static constexpr bool value = true;
};

int main() {
    std::cout << std::boolalpha;
    std::cout << IsSame<int, int>::value << "\n";         // true
    std::cout << IsSame<int, double>::value << "\n";      // false
    std::cout << IsSame<float, float>::value << "\n";     // true
    std::cout << IsSame<int, const int>::value << "\n";   // false
}
```

</details>

---

## Task 20 — Static Assertion with Templates

Write a template function `safeDivide(T a, T b)` that uses `static_assert` to ensure the function is only instantiated with floating-point types.

<details>
<summary>Solution</summary>

```cpp
#include <iostream>
#include <type_traits>

template <typename T>
T safeDivide(T a, T b) {
    static_assert(std::is_floating_point_v<T>,
                  "safeDivide requires a floating-point type");
    if (b == T{0})
        throw std::runtime_error("Division by zero");
    return a / b;
}

int main() {
    std::cout << safeDivide(10.0, 3.0) << "\n";   // 3.33333
    std::cout << safeDivide(7.5f, 2.5f) << "\n";  // 3

    // safeDivide(10, 3); // static_assert failure
}
```

</details>

---

## Task 21 — CRTP (Curiously Recurring Template Pattern)

Implement a CRTP base class `Counter<T>` that tracks how many instances of the derived class currently exist. Create a derived class `Widget` that uses it.

<details>
<summary>Solution</summary>

```cpp
#include <iostream>

template <typename Derived>
class Counter {
    static int count;
protected:
    Counter()  { ++count; }
    Counter(const Counter&) { ++count; }
    ~Counter() { --count; }
public:
    static int alive() { return count; }
};

template <typename Derived>
int Counter<Derived>::count = 0;

class Widget : public Counter<Widget> {
public:
    std::string name;
    Widget(std::string n) : name(std::move(n)) {}
};

class Gadget : public Counter<Gadget> {};

int main() {
    {
        Widget w1("a"), w2("b");
        Gadget g1;
        std::cout << "Widgets: " << Widget::alive() << "\n"; // 2
        std::cout << "Gadgets: " << Gadget::alive() << "\n"; // 1
    }
    std::cout << "Widgets: " << Widget::alive() << "\n"; // 0
    std::cout << "Gadgets: " << Gadget::alive() << "\n"; // 0
}
```

</details>

---

## Task 22 — Concepts (C++20)

Define a **concept** `Addable` that checks whether two values of a type can be added with `+`. Then write a function template `add` constrained by this concept.

<details>
<summary>Solution</summary>

```cpp
#include <iostream>
#include <string>
#include <concepts>

template <typename T>
concept Addable = requires(T a, T b) {
    { a + b } -> std::convertible_to<T>;
};

template <Addable T>
T add(T a, T b) {
    return a + b;
}

int main() {
    std::cout << add(3, 4) << "\n";                           // 7
    std::cout << add(1.5, 2.5) << "\n";                       // 4
    std::cout << add(std::string("hello "), std::string("world")) << "\n"; // hello world

    // struct Foo {};
    // add(Foo{}, Foo{}); // won't compile — Foo is not Addable
}
```

</details>

---

## Task 23 — Template Metaprogramming: Compile-Time Factorial

Compute the factorial of a number **entirely at compile time** using template metaprogramming (recursive struct specialization).

```cpp
static_assert(Factorial<5>::value == 120);
```

<details>
<summary>Solution</summary>

```cpp
#include <iostream>

template <unsigned N>
struct Factorial {
    static constexpr unsigned long long value = N * Factorial<N - 1>::value;
};

template <>
struct Factorial<0> {
    static constexpr unsigned long long value = 1;
};

int main() {
    static_assert(Factorial<0>::value == 1);
    static_assert(Factorial<5>::value == 120);
    static_assert(Factorial<10>::value == 3628800);

    std::cout << "5! = " << Factorial<5>::value << "\n";
    std::cout << "10! = " << Factorial<10>::value << "\n";
    std::cout << "20! = " << Factorial<20>::value << "\n";
}
```

</details>

---

## Task 24 — Compile-Time Fibonacci with Templates

Compute the Nth Fibonacci number at compile time using template metaprogramming.

```cpp
static_assert(Fibonacci<10>::value == 55);
```

<details>
<summary>Solution</summary>

```cpp
#include <iostream>

template <unsigned N>
struct Fibonacci {
    static constexpr unsigned long long value =
        Fibonacci<N - 1>::value + Fibonacci<N - 2>::value;
};

template <>
struct Fibonacci<0> {
    static constexpr unsigned long long value = 0;
};

template <>
struct Fibonacci<1> {
    static constexpr unsigned long long value = 1;
};

int main() {
    static_assert(Fibonacci<0>::value == 0);
    static_assert(Fibonacci<1>::value == 1);
    static_assert(Fibonacci<10>::value == 55);
    static_assert(Fibonacci<20>::value == 6765);

    std::cout << "Fib(10) = " << Fibonacci<10>::value << "\n";
    std::cout << "Fib(20) = " << Fibonacci<20>::value << "\n";
    std::cout << "Fib(30) = " << Fibonacci<30>::value << "\n";
}
```

</details>

---

## Task 25 — Type Erasure with Templates

Implement a simplified `AnyCallable` class that can store and invoke any callable (function, lambda, functor) with signature `int(int)`. Use templates internally but present a non-template interface to the user.

<details>
<summary>Solution</summary>

```cpp
#include <iostream>
#include <memory>

class AnyCallable {
    struct Concept {
        virtual int call(int) = 0;
        virtual ~Concept() = default;
    };

    template <typename F>
    struct Model : Concept {
        F func;
        Model(F f) : func(std::move(f)) {}
        int call(int arg) override { return func(arg); }
    };

    std::unique_ptr<Concept> impl;

public:
    template <typename F>
    AnyCallable(F f) : impl(std::make_unique<Model<F>>(std::move(f))) {}

    int operator()(int arg) {
        return impl->call(arg);
    }
};

// Some test callables
int doubleIt(int x) { return x * 2; }

struct Squarer {
    int operator()(int x) { return x * x; }
};

int main() {
    AnyCallable f1(doubleIt);
    std::cout << f1(5) << "\n"; // 10

    AnyCallable f2(Squarer{});
    std::cout << f2(5) << "\n"; // 25

    AnyCallable f3([](int x) { return x + 100; });
    std::cout << f3(5) << "\n"; // 105
}
```

</details>

---

> **Tip:** Compile with C++17 or C++20 flags for the later tasks:
> ```bash
> g++ -std=c++20 -o solution solution.cpp
> ```
