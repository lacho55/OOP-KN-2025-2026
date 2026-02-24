# C++ Templates — Complete Theory Guide

A comprehensive reference covering everything you need to know about C++ templates, from first principles to advanced metaprogramming.

---

## Table of Contents

1. [What Are Templates?](#1-what-are-templates)
2. [Function Templates](#2-function-templates)
3. [Class Templates](#3-class-templates)
4. [Non-Type Template Parameters](#4-non-type-template-parameters)
5. [Default Template Arguments](#5-default-template-arguments)
6. [Template Specialization](#6-template-specialization)
7. [Member Function Templates](#7-member-function-templates)
8. [Variadic Templates](#8-variadic-templates)
9. [Fold Expressions (C++17)](#9-fold-expressions-c17)
10. [SFINAE — Substitution Failure Is Not An Error](#10-sfinae--substitution-failure-is-not-an-error)
11. [`if constexpr` — Compile-Time Branching (C++17)](#11-if-constexpr--compile-time-branching-c17)
12. [Template Template Parameters](#12-template-template-parameters)
13. [Type Traits](#13-type-traits)
14. [`static_assert` with Templates](#14-static_assert-with-templates)
15. [CRTP — Curiously Recurring Template Pattern](#15-crtp--curiously-recurring-template-pattern)
16. [Concepts (C++20)](#16-concepts-c20)
17. [Template Metaprogramming](#17-template-metaprogramming)
18. [Type Erasure](#18-type-erasure)
19. [Best Practices & Common Pitfalls](#19-best-practices--common-pitfalls)

---

## 1. What Are Templates?

Templates are a feature of C++ that allows you to write **generic code** — code that works with any data type without being rewritten for each one. Instead of writing separate functions or classes for `int`, `double`, `std::string`, etc., you write a single **blueprint** and let the compiler generate the specific versions for you.

### The Core Idea

Without templates, you'd need to duplicate code:

```cpp
// Without templates — repetitive and error-prone
int    maxInt(int a, int b)       { return (a > b) ? a : b; }
double maxDouble(double a, double b) { return (a > b) ? a : b; }
float  maxFloat(float a, float b)    { return (a > b) ? a : b; }
// ... and so on for every type
```

With templates, you write it **once**:

```cpp
// With templates — one definition works for ALL types
template <typename T>
T maxVal(T a, T b) {
    return (a > b) ? a : b;
}
```

### How It Works Under the Hood

Templates use a mechanism called **instantiation**. The compiler doesn't generate any code from a template itself. Instead, when you *use* a template with a specific type, the compiler generates ("instantiates") a concrete version for that type.

```cpp
maxVal(3, 7);       // Compiler generates: int maxVal(int, int)
maxVal(3.14, 2.71); // Compiler generates: double maxVal(double, double)
maxVal('a', 'z');   // Compiler generates: char maxVal(char, char)
```

This is why templates must typically be defined in **header files** — the compiler needs to see the full template definition at the point where it's being used, so it can generate the code.

### Two Main Kinds of Templates

C++ has two primary kinds of templates:

- **Function templates** — generic functions
- **Class templates** — generic classes/structs

We'll cover both in depth.

---

## 2. Function Templates

A function template defines a family of functions. The compiler generates the actual function when you call it with specific types.

### Basic Syntax

```cpp
//  keyword    parameter list
//    |            |
template <typename T>   // <-- "For any type T..."
T maxVal(T a, T b) {    // <-- T is used like a regular type
    return (a > b) ? a : b;
}
```

The `typename` keyword introduces a **type parameter**. You can also use `class` instead — they mean the exact same thing in this context:

```cpp
template <class T>   // Identical to template <typename T>
T maxVal(T a, T b) { ... }
```

> **Convention:** Modern C++ tends to prefer `typename`, but `class` is still widely used.

### Template Argument Deduction

When you call a function template, the compiler can usually **deduce** the type from the arguments you pass:

```cpp
maxVal(3, 7);         // T is deduced as int
maxVal(3.14, 2.71);   // T is deduced as double

// You can also specify the type explicitly:
maxVal<int>(3, 7);         // Explicit: T = int
maxVal<double>(3.14, 2.71); // Explicit: T = double
```

Explicit specification is required when the compiler can't figure it out, for instance when the types don't match:

```cpp
// maxVal(3, 2.5);         // ERROR! T can't be both int and double
maxVal<double>(3, 2.5);    // OK — both arguments converted to double
```

### Multiple Type Parameters

A template can have more than one type parameter:

```cpp
// Two type parameters: T and U
// The return type uses 'auto' so the compiler deduces it
template <typename T, typename U>
auto multiply(T a, U b) {
    return a * b;  // The return type depends on what T * U produces
}

// Usage:
multiply(3, 4.5);    // T=int, U=double → returns double (13.5)
multiply(2.5, 4);    // T=double, U=int → returns double (10.0)
```

### Trailing Return Type (Alternative Syntax)

Before C++14's `auto` return type deduction, you had to use a **trailing return type**:

```cpp
// C++11 way — trailing return type with decltype
template <typename T, typename U>
auto multiply(T a, U b) -> decltype(a * b) {
    return a * b;
}

// C++14+ way — simpler, auto deduction
template <typename T, typename U>
auto multiply(T a, U b) {
    return a * b;
}
```

### Pass-by-Reference vs Pass-by-Value

Templates work with both. Use references to avoid expensive copies:

```cpp
// By value — fine for small types (int, double, char)
template <typename T>
T maxVal(T a, T b) { return (a > b) ? a : b; }

// By const reference — better for large types (std::string, std::vector)
template <typename T>
const T& maxRef(const T& a, const T& b) { return (a > b) ? a : b; }

// By non-const reference — when you need to modify the arguments
template <typename T>
void mySwap(T& a, T& b) {
    T temp = a;  // One copy
    a = b;       // Assignment
    b = temp;    // Assignment
}
```

---

## 3. Class Templates

Just as function templates generalize functions, **class templates** generalize classes. You can parameterize the types of member variables, method parameters, and return types.

### Basic Syntax

```cpp
// A simple generic container that holds one value
template <typename T>
class Box {
private:
    T value;           // Member variable of type T

public:
    // Constructor takes a T
    Box(T val) : value(val) {}

    // Getter returns a T
    T get() const { return value; }

    // Setter takes a T
    void set(T val) { value = val; }
};
```

### Using Class Templates

Unlike function templates, the compiler **cannot** deduce template arguments from constructor calls (prior to C++17). You must specify them:

```cpp
Box<int> intBox(42);              // T = int
Box<std::string> strBox("hello"); // T = std::string
Box<double> dblBox(3.14);         // T = double

std::cout << intBox.get();   // 42
std::cout << strBox.get();   // hello
```

> **C++17 addition:** Class Template Argument Deduction (CTAD) allows the compiler to
> deduce template arguments from constructor arguments:
> ```cpp
> Box b(42);          // C++17: T deduced as int
> Box s("hello"s);    // C++17: T deduced as std::string
> ```

### Multiple Type Parameters in Classes

```cpp
// A pair of two possibly different types
template <typename T, typename U>
class Pair {
    T first_;
    U second_;

public:
    Pair(T f, U s) : first_(f), second_(s) {}

    T first()  const { return first_; }
    U second() const { return second_; }
};

// Usage:
Pair<int, std::string> p(1, "one");
Pair<double, double> coords(3.14, 2.71);
```

### Defining Member Functions Outside the Class

When you define methods outside the class body, you must repeat the template prefix:

```cpp
template <typename T>
class Box {
    T value;
public:
    Box(T val);
    T get() const;
    void set(T val);
};

// Each method definition needs the template<typename T> prefix
// and the class name must be qualified as Box<T>
template <typename T>
Box<T>::Box(T val) : value(val) {}

template <typename T>
T Box<T>::get() const { return value; }

template <typename T>
void Box<T>::set(T val) { value = val; }
```

---

## 4. Non-Type Template Parameters

Template parameters don't have to be types! They can also be **values** — integers, enums, pointers, or (since C++20) floating-point numbers and literal class types.

### Syntax and Motivation

```cpp
// N is a non-type template parameter — it's a compile-time integer
template <typename T, std::size_t N>
class StaticArray {
    T data[N]{};   // Array size is fixed at compile time

public:
    T get(std::size_t i) const { return data[i]; }
    void set(std::size_t i, T val) { data[i] = val; }
    constexpr std::size_t size() const { return N; }
};

// Usage — N must be known at compile time
StaticArray<int, 5> arr;     // Array of 5 ints
StaticArray<double, 100> big; // Array of 100 doubles
```

The key point: `N` is **baked into the type** at compile time. `StaticArray<int, 5>` and `StaticArray<int, 10>` are completely different, unrelated types.

### Non-Type Parameters with Functions

A common and elegant use — deducing array sizes:

```cpp
// The compiler deduces both T and N from the array argument
template <typename T, std::size_t N>
void printArray(const T (&arr)[N]) {
    //                    ^^^^^^^
    //  "reference to array of N elements of type T"
    //  N is deduced from the actual array size

    for (std::size_t i = 0; i < N; ++i)
        std::cout << arr[i] << " ";
    std::cout << "\n";
}

// Usage — no need to pass the size manually!
int nums[] = {10, 20, 30};
printArray(nums);   // T=int, N=3 → prints: 10 20 30

double vals[] = {1.1, 2.2};
printArray(vals);   // T=double, N=2 → prints: 1.1 2.2
```

### What Types Can Be Non-Type Parameters?

| C++ Standard | Allowed Non-Type Parameter Types                              |
|--------------|---------------------------------------------------------------|
| C++98/03     | Integral types, enums, pointers, references                   |
| C++11        | Same + `nullptr_t`                                            |
| C++17        | Same + `auto` for deduced non-type params                     |
| C++20        | Same + floating-point, literal class types with `operator<=>` |

```cpp
// C++20: floating-point non-type parameter
template <double Factor>
double scale(double val) {
    return val * Factor;
}
scale<2.5>(10.0); // 25.0
```

---

## 5. Default Template Arguments

Just like function parameters can have default values, template parameters can have defaults.

### Syntax

```cpp
// N defaults to 10 if not specified
template <typename T, std::size_t N = 10>
class Container {
    T data[N]{};
    std::size_t count = 0;

public:
    void push(T value) { data[count++] = value; }

    void print() const {
        for (std::size_t i = 0; i < count; ++i)
            std::cout << data[i] << " ";
        std::cout << "\n";
    }
};

// Usage:
Container<int>     c1;  // N defaults to 10
Container<int, 3>  c2;  // N explicitly set to 3
Container<double>  c3;  // T=double, N=10
```

### Rules for Default Arguments

Defaults must go from **right to left** (just like function parameter defaults):

```cpp
// OK — default on rightmost parameter
template <typename T, typename Alloc = std::allocator<T>>
class MyVector { ... };

// OK — multiple defaults
template <typename T = int, std::size_t N = 10>
class Buffer { ... };

// ERROR — can't have default before non-default
// template <typename T = int, typename U>  // Won't compile
// class Bad { ... };
```

### Standard Library Example

The STL uses defaults extensively. For instance, `std::vector` actually has two template parameters:

```cpp
// Simplified declaration of std::vector
template <
    typename T,
    typename Allocator = std::allocator<T>  // default allocator
>
class vector { ... };

// That's why you can write:
std::vector<int> v;  // Uses default allocator
```

---

## 6. Template Specialization

Sometimes the generic template doesn't do the right thing for a specific type. **Specialization** lets you provide a custom implementation for particular types.

### Full (Explicit) Specialization

You provide a completely custom implementation for one specific set of template arguments:

```cpp
// -----------------------------------------------
// Primary template — the general case
// -----------------------------------------------
template <typename T>
void printValue(T val) {
    std::cout << val << "\n";
}

// -----------------------------------------------
// Full specialization for bool
// -----------------------------------------------
// template <>              <-- empty angle brackets = "I'm specializing"
// void printValue<bool>    <-- the specific type I'm specializing for
template <>
void printValue<bool>(bool val) {
    std::cout << (val ? "true" : "false") << "\n";
}

// Usage:
printValue(42);      // Uses primary template   → "42"
printValue(3.14);    // Uses primary template   → "3.14"
printValue(true);    // Uses bool specialization → "true"
printValue(false);   // Uses bool specialization → "false"
```

### How the Compiler Chooses

When you call `printValue(true)`, the compiler goes through this process:
1. Find all templates named `printValue`
2. Check if any specialization matches the argument type exactly
3. If yes → use the specialization
4. If no → use the primary template

### Partial Specialization (Classes Only)

Partial specialization lets you specialize for a **category** of types rather than one specific type. This is only available for class templates, not function templates.

```cpp
// -----------------------------------------------
// Primary template — handles any type T
// -----------------------------------------------
template <typename T>
class Storage {
    T value;
public:
    Storage(T val) : value(val) {}
    void describe() const {
        std::cout << "Storing a value: " << value << "\n";
    }
};

// -----------------------------------------------
// Partial specialization — handles ALL pointer types T*
// -----------------------------------------------
// Note: template still has a parameter (T), but the class
// argument is more specific (T*) than the primary (T)
template <typename T>
class Storage<T*> {
    T* ptr;
public:
    Storage(T* p) : ptr(p) {}
    void describe() const {
        std::cout << "Storing a pointer to: " << *ptr << "\n";
    }
};

// Usage:
int x = 42;

Storage<int>  s1(42);   // Matches primary template (T = int)
Storage<int*> s2(&x);   // Matches partial specialization (T = int, pattern = T*)

s1.describe();  // "Storing a value: 42"
s2.describe();  // "Storing a pointer to: 42"
```

### Why Can't Functions Be Partially Specialized?

The C++ standard doesn't allow partial specialization of function templates. Instead, you use **overloading**:

```cpp
// Primary template
template <typename T>
void process(T val) {
    std::cout << "Value: " << val << "\n";
}

// This is NOT partial specialization — it's an OVERLOAD
template <typename T>
void process(T* ptr) {
    std::cout << "Pointer to: " << *ptr << "\n";
}
```

---

## 7. Member Function Templates

A non-template class can have **template member functions**, and a template class can have additional template member functions with their own parameters.

### Template Method Inside a Regular Class

```cpp
class Printer {
    std::string prefix;

public:
    Printer(std::string p) : prefix(std::move(p)) {}

    // This is a member function template
    // The class itself is not a template, but this method is
    template <typename T>
    void print(T value) const {
        std::cout << prefix << ": " << value << "\n";
    }
};

// Usage:
Printer p("Value");
p.print(42);              // T = int        → "Value: 42"
p.print(3.14);            // T = double     → "Value: 3.14"
p.print("hello");         // T = const char*→ "Value: hello"
```

### Template Method Inside a Template Class

```cpp
template <typename T>
class Converter {
    T value;

public:
    Converter(T val) : value(val) {}

    // An additional template parameter U, independent of T
    template <typename U>
    U convertTo() const {
        return static_cast<U>(value);
    }
};

// Usage:
Converter<double> c(3.14);
int i = c.convertTo<int>();       // 3
float f = c.convertTo<float>();   // 3.14f
```

---

## 8. Variadic Templates

Introduced in C++11, variadic templates let you write templates that accept **any number of arguments** of any types.

### Syntax: The Parameter Pack

```cpp
// typename... Args   → "Args" is a PARAMETER PACK (zero or more types)
// Args... args       → "args" is a FUNCTION PARAMETER PACK (zero or more values)
template <typename... Args>
void myFunction(Args... args) {
    // sizeof...(args) gives the number of arguments at compile time
    std::cout << "Called with " << sizeof...(args) << " arguments\n";
}

myFunction();              // 0 arguments
myFunction(1);             // 1 argument
myFunction(1, 2.5, "hi"); // 3 arguments
```

### Recursive Unpacking (The Classic Pattern)

Since you can't iterate over a parameter pack directly, the classic approach is **recursion**: handle the first argument, then recurse on the rest.

```cpp
// -----------------------------------------------
// Base case — no arguments left
// -----------------------------------------------
void printAll() {
    std::cout << "\n";  // End the line when done
}

// -----------------------------------------------
// Recursive case — peel off the first argument
// -----------------------------------------------
template <typename T, typename... Rest>
void printAll(T first, Rest... rest) {
    //         ^^^^^   ^^^^^^^^^^
    //         |       The remaining arguments (parameter pack)
    //         The first argument (known type T)

    std::cout << first;

    if constexpr (sizeof...(rest) > 0)  // Avoid trailing space
        std::cout << " ";

    printAll(rest...);
    //       ^^^^^^^
    //       "Expand" the pack — this calls printAll with
    //       one fewer argument each time, until base case
}

// Trace of printAll(1, 2.5, "hi"):
//   printAll(1, 2.5, "hi")  → prints "1 ", calls printAll(2.5, "hi")
//   printAll(2.5, "hi")     → prints "2.5 ", calls printAll("hi")
//   printAll("hi")          → prints "hi", calls printAll()
//   printAll()              → prints "\n" (base case)
```

### Variadic Sum — Another Example

```cpp
// Base case: a single argument
template <typename T>
T sum(T val) {
    return val;
}

// Recursive case: first + sum of the rest
template <typename T, typename... Rest>
auto sum(T first, Rest... rest) {
    return first + sum(rest...);
}

// Usage:
sum(1, 2, 3, 4, 5);   // 15
sum(1.5, 2.5, 3.0);   // 7.0
```

---

## 9. Fold Expressions (C++17)

C++17 introduced **fold expressions** to simplify variadic templates by eliminating the need for recursion and base cases.

### Syntax

A fold expression applies a binary operator across all elements of a parameter pack:

```
( pack   op ... )          // Unary right fold
( ...    op pack )          // Unary left fold
( pack   op ... op init )   // Binary right fold
( init   op ... op pack )   // Binary left fold
```

### Examples

```cpp
// -----------------------------------------------
// Sum using fold expression — replaces recursive version!
// -----------------------------------------------
template <typename... Args>
auto sum(Args... args) {
    return (args + ...);
    //      ^^^^^^^^^^
    //  Unary right fold:
    //  (arg1 + (arg2 + (arg3 + arg4)))
}

sum(1, 2, 3, 4); // → (1 + (2 + (3 + 4))) → 10


// -----------------------------------------------
// Print all using fold with the comma operator
// -----------------------------------------------
template <typename... Args>
void printAll(Args... args) {
    ((std::cout << args << " "), ...);
    //  ^^^^^^^^^^^^^^^^^^^^^^^^
    //  Unary right fold with operator,
    //  Expands to: (cout << a1 << " "), ((cout << a2 << " "), (cout << a3 << " "))
    std::cout << "\n";
}

printAll(1, 2.5, "hello"); // → "1 2.5 hello"


// -----------------------------------------------
// Logical AND — check if all arguments are true
// -----------------------------------------------
template <typename... Args>
bool allTrue(Args... args) {
    return (args && ...);   // Fold with &&
}

allTrue(true, true, true);   // true
allTrue(true, false, true);  // false


// -----------------------------------------------
// Binary fold with init — sum with an initial value
// -----------------------------------------------
template <typename... Args>
auto sumFrom(int init, Args... args) {
    return (init + ... + args);
    //      ^^^^^^^^^^^^^^^^^^
    //  Binary left fold:
    //  ((init + arg1) + arg2) + arg3
}

sumFrom(100, 1, 2, 3); // 106
```

---

## 10. SFINAE — Substitution Failure Is Not An Error

SFINAE is a fundamental C++ rule: if substituting template arguments into a template causes an invalid type or expression, the compiler **silently discards** that template candidate rather than producing a compile error.

### Why It Matters

SFINAE lets you **conditionally enable/disable** templates based on type properties.

### `std::enable_if` — The Classic SFINAE Tool

```cpp
#include <type_traits>

// -----------------------------------------------
// This function only exists for arithmetic types
// -----------------------------------------------
template <typename T>
typename std::enable_if<std::is_arithmetic<T>::value, T>::type
//       ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
//  If T is arithmetic → this becomes just "T" (the return type)
//  If T is NOT arithmetic → substitution failure → this overload is discarded
half(T val) {
    return val / 2;
}

// Usage:
half(10);     // OK: int is arithmetic → returns 5
half(3.14);   // OK: double is arithmetic → returns 1.57
// half("hi"); // ERROR: no matching function (the template was discarded)
```

### Breaking Down `enable_if`

`std::enable_if` is actually a very simple struct:

```cpp
// Simplified implementation of std::enable_if
template <bool Condition, typename T = void>
struct enable_if {};  // Primary: has NO member 'type'

template <typename T>
struct enable_if<true, T> {
    using type = T;   // Specialization: has member 'type' only when true
};

// So:
// enable_if<true, int>::type  → int        (exists)
// enable_if<false, int>::type → ERROR       (no member 'type')
//                                           (but SFINAE makes it non-fatal)
```

### Cleaner Syntax with C++14 and C++17

```cpp
// C++14: _v and _t suffixes
template <typename T>
std::enable_if_t<std::is_arithmetic_v<T>, T>
half(T val) { return val / 2; }

// C++17: constexpr if is often a better choice (see next section)
```

---

## 11. `if constexpr` — Compile-Time Branching (C++17)

`if constexpr` evaluates conditions **at compile time** and completely discards the branches that aren't taken. Unlike SFINAE, it's readable and intuitive.

### Syntax and Behavior

```cpp
#include <type_traits>
#include <string>

template <typename T>
void describe(T val) {
    // Each condition is checked at COMPILE TIME
    // Only the matching branch is compiled

    if constexpr (std::is_integral_v<T>) {
        // This code ONLY exists when T is integral
        std::cout << val << " is an integer type\n";
    }
    else if constexpr (std::is_floating_point_v<T>) {
        // This code ONLY exists when T is floating-point
        std::cout << val << " is a floating-point type\n";
    }
    else {
        // Everything else
        std::cout << val << " is some other type\n";
    }
}

// Usage:
describe(42);                     // "42 is an integer type"
describe(3.14);                   // "3.14 is a floating-point type"
describe(std::string("hello"));   // "hello is some other type"
```

### `if constexpr` vs Regular `if`

The key difference: with regular `if`, **all branches must compile** even if they're never executed. With `if constexpr`, discarded branches are not compiled at all.

```cpp
template <typename T>
void example(T val) {
    // Regular if — BOTH branches must compile for ALL types
    // if (std::is_integral_v<T>)
    //     std::cout << val % 2;    // ERROR if T is std::string
    //                               // (can't use % on strings)

    // if constexpr — only the matching branch needs to compile
    if constexpr (std::is_integral_v<T>)
        std::cout << val % 2;       // OK — only compiled when T is integral
    else
        std::cout << val;           // OK — only compiled otherwise
}
```

### Replacing Recursive Variadic Templates

```cpp
// Elegant printAll using if constexpr — no separate base case needed!
template <typename T, typename... Rest>
void printAll(T first, Rest... rest) {
    std::cout << first;

    if constexpr (sizeof...(rest) > 0) {
        // This recursive call only exists when there are remaining args
        std::cout << " ";
        printAll(rest...);
    } else {
        std::cout << "\n";
    }
}
```

---

## 12. Template Template Parameters

A **template template parameter** is a template parameter that is itself a template. This lets you write code that is parameterized on the *container type* rather than on the element type.

### The Problem

Suppose you want a function that works with any container. A naive approach doesn't work well:

```cpp
// This works, but you must specify the full type:
template <typename Container>
void fill(Container& c) { ... }

fill(std::vector<int>{});  // OK but Container = std::vector<int>
                            // Can't change element type independently
```

### The Solution: Template Template Parameters

```cpp
// Container is a template template parameter:
//   "a template that takes two type arguments"
template <
    template <typename, typename> class Container,
    //       ^^^^^^^^^^^^^^^^^^^ ^^^^^ ^^^^^^^^^
    //       Container's own params     "Container" is the name
    typename T
>
void fillAndPrint() {
    Container<T, std::allocator<T>> c;

    for (T i = 1; i <= 5; ++i)
        c.push_back(i);

    for (const auto& val : c)
        std::cout << val << " ";
    std::cout << "\n";
}

// Usage — pass the TEMPLATE itself, not an instantiated type:
fillAndPrint<std::vector, int>();    // Uses vector<int>
fillAndPrint<std::deque, double>(); // Uses deque<double>
fillAndPrint<std::list, int>();     // Uses list<int>
```

> **Note:** The `std::allocator<T>` parameter is needed because `std::vector` actually has
> two template parameters: `template <typename T, typename Alloc>`. Our template template
> parameter must match this signature.

---

## 13. Type Traits

Type traits are **compile-time queries** about types. They're templates (usually structs) in `<type_traits>` that let you ask questions like "is this type an integer?" or "is this type a pointer?"

### How They Work

Every type trait is a struct template with a static `value` member:

```cpp
#include <type_traits>

// Querying properties:
std::is_integral<int>::value;          // true
std::is_integral<double>::value;       // false
std::is_floating_point<double>::value; // true
std::is_pointer<int*>::value;          // true
std::is_same<int, int>::value;         // true
std::is_same<int, double>::value;      // false

// C++17 shorthand (_v suffix):
std::is_integral_v<int>;               // true (same as ::value)
```

### Common Type Traits

| Trait | Checks |
|---|---|
| `is_integral<T>` | int, char, bool, long, etc. |
| `is_floating_point<T>` | float, double, long double |
| `is_arithmetic<T>` | integral or floating-point |
| `is_pointer<T>` | any pointer type |
| `is_reference<T>` | lvalue or rvalue reference |
| `is_const<T>` | whether T is const-qualified |
| `is_same<T, U>` | whether T and U are the same type |
| `is_base_of<Base, Derived>` | inheritance check |
| `is_constructible<T, Args...>` | can T be constructed with Args |
| `is_convertible<From, To>` | implicit conversion possible |

### Type Transformation Traits

Some traits *transform* types rather than query them:

```cpp
std::remove_const<const int>::type;       // int
std::remove_reference<int&>::type;        // int
std::remove_pointer<int*>::type;          // int
std::add_const<int>::type;                // const int
std::decay<const int&>::type;             // int

// C++14 shorthand (_t suffix):
std::remove_const_t<const int>;           // int
```

### Building Your Own Type Trait

You can create custom type traits using template specialization:

```cpp
// -----------------------------------------------
// Custom is_same — checks if two types are identical
// -----------------------------------------------

// Primary template: T and U are different → false
template <typename T, typename U>
struct IsSame {
    static constexpr bool value = false;
};

// Partial specialization: T and T are the same → true
template <typename T>
struct IsSame<T, T> {
    static constexpr bool value = true;
};

// Usage:
IsSame<int, int>::value;       // true
IsSame<int, double>::value;    // false
```

---

## 14. `static_assert` with Templates

`static_assert` is a compile-time assertion. Combined with type traits, it gives you clear, custom error messages when templates are misused.

### Syntax

```cpp
static_assert(condition, "error message");
// If condition is false → compilation fails with the given message
```

### Usage in Templates

```cpp
template <typename T>
T safeDivide(T a, T b) {
    // This check happens at COMPILE TIME
    static_assert(
        std::is_floating_point_v<T>,
        "safeDivide requires a floating-point type!"
    );

    if (b == T{0})
        throw std::runtime_error("Division by zero");
    return a / b;
}

safeDivide(10.0, 3.0);  // OK — double is floating-point
safeDivide(7.5f, 2.5f); // OK — float is floating-point
// safeDivide(10, 3);   // COMPILE ERROR:
//   "safeDivide requires a floating-point type!"
```

### `static_assert` vs SFINAE vs Concepts

| Approach | Effect | Error Quality |
|---|---|---|
| `static_assert` | Hard error with custom message | Great — you write the message |
| SFINAE / `enable_if` | Silently removes overload | Poor — cryptic "no matching function" |
| Concepts (C++20) | Constrains and removes overload | Good — compiler explains the constraint |

Use `static_assert` when you want to give users a **clear error message** about why something won't work.

---

## 15. CRTP — Curiously Recurring Template Pattern

CRTP is a pattern where a class inherits from a template instantiated with **itself** as the argument. It enables **static polymorphism** — polymorphic behavior without virtual functions.

### The Pattern

```cpp
// The base class template takes the derived class as a parameter
template <typename Derived>
class Base {
    // Can call Derived's methods via static_cast
};

// The derived class passes itself to the base
class MyClass : public Base<MyClass> {
    // ...
};
```

### Example: Instance Counter

A common use case — counting how many instances of a class exist:

```cpp
template <typename Derived>
class Counter {
    static int count;   // Each Derived class gets its OWN count

protected:
    Counter()  { ++count; }       // Increment on construction
    Counter(const Counter&) { ++count; }
    ~Counter() { --count; }       // Decrement on destruction

public:
    static int alive() { return count; }
};

// CRITICAL: Each instantiation of Counter<T> has its own static variable
template <typename Derived>
int Counter<Derived>::count = 0;

// -----------------------------------------------
// Now any class can get counting by inheriting from Counter<itself>
// -----------------------------------------------
class Widget : public Counter<Widget> {
    //                         ^^^^^^
    //  Widget passes ITSELF as the template argument
};

class Gadget : public Counter<Gadget> {};

// Widget::alive() and Gadget::alive() are INDEPENDENT counters
// because Counter<Widget> and Counter<Gadget> are different classes
```

### Why CRTP Instead of Virtual Functions?

- **No virtual function overhead** — no vtable, no indirection
- **Inlined at compile time** — the compiler knows the exact types
- **Used by the STL** — `std::enable_shared_from_this` uses CRTP

### Example: Static Polymorphism

```cpp
template <typename Derived>
class Shape {
public:
    double area() const {
        // Call the derived class's implementation — no virtual!
        return static_cast<const Derived*>(this)->areaImpl();
    }
};

class Circle : public Shape<Circle> {
    double radius;
public:
    Circle(double r) : radius(r) {}
    double areaImpl() const { return 3.14159 * radius * radius; }
};

class Square : public Shape<Square> {
    double side;
public:
    Square(double s) : side(s) {}
    double areaImpl() const { return side * side; }
};

// Usage (note: no base class pointer polymorphism — that requires virtual)
template <typename T>
void printArea(const Shape<T>& shape) {
    std::cout << "Area: " << shape.area() << "\n";
}

Circle c(5);
Square s(4);
printArea(c);  // "Area: 78.5398"
printArea(s);  // "Area: 16"
```

---

## 16. Concepts (C++20)

Concepts are C++20's answer to the long-standing problem of constraining templates in a readable way. They replace SFINAE for most use cases.

### What Is a Concept?

A concept is a **named set of requirements** on a type. It's a compile-time predicate.

### Defining Concepts

```cpp
#include <concepts>

// -----------------------------------------------
// A concept that requires T to support + and return a T
// -----------------------------------------------
template <typename T>
concept Addable = requires(T a, T b) {
    { a + b } -> std::convertible_to<T>;
    //  ^^^^     ^^^^^^^^^^^^^^^^^^^^^^^^
    //  expression   return type constraint
};

// -----------------------------------------------
// A concept that requires a type to be printable
// -----------------------------------------------
template <typename T>
concept Printable = requires(T val, std::ostream& os) {
    { os << val } -> std::same_as<std::ostream&>;
};

// -----------------------------------------------
// A concept for "container-like" types
// -----------------------------------------------
template <typename T>
concept Container = requires(T c) {
    c.begin();
    c.end();
    c.size();
    typename T::value_type;  // Must have a value_type member type
};
```

### Using Concepts to Constrain Templates

There are multiple ways to apply a concept:

```cpp
// -----------------------------------------------
// Method 1: Constrained template parameter
// -----------------------------------------------
template <Addable T>
T add(T a, T b) { return a + b; }

// -----------------------------------------------
// Method 2: requires clause
// -----------------------------------------------
template <typename T>
    requires Addable<T>
T add(T a, T b) { return a + b; }

// -----------------------------------------------
// Method 3: Trailing requires clause
// -----------------------------------------------
template <typename T>
T add(T a, T b) requires Addable<T> { return a + b; }

// -----------------------------------------------
// Method 4: Abbreviated template syntax (auto)
// -----------------------------------------------
auto add(Addable auto a, Addable auto b) { return a + b; }
```

### Standard Library Concepts

C++20's `<concepts>` header provides many built-in concepts:

```cpp
std::integral<T>          // int, char, bool, etc.
std::floating_point<T>    // float, double
std::signed_integral<T>   // signed int, etc.
std::same_as<T, U>        // T and U are the same type
std::convertible_to<T, U> // T is implicitly convertible to U
std::derived_from<D, B>   // D derives from B
std::movable<T>           // T supports move operations
std::copyable<T>          // T supports copy operations
std::equality_comparable<T>  // T supports == and !=
std::totally_ordered<T>      // T supports <, >, <=, >=
```

### Better Error Messages

This is the killer feature. Compare:

```cpp
// With enable_if — error message:
//   "error: no matching function for call to 'add'"
//   "note: candidate template ignored: requirement
//    'std::is_arithmetic_v<std::basic_string<char>>' was not satisfied"

// With concepts — error message:
//   "error: constraints not satisfied for 'add'"
//   "note: because 'std::string' does not satisfy 'Addable'"
```

---

## 17. Template Metaprogramming

Template metaprogramming (TMP) is the practice of using templates to perform **computations at compile time**. The "programs" are templates; the "execution" happens during compilation.

### Compile-Time Factorial

```cpp
// -----------------------------------------------
// Each instantiation computes one multiplication
// -----------------------------------------------
template <unsigned N>
struct Factorial {
    // Recursive case: N * (N-1)!
    static constexpr unsigned long long value = N * Factorial<N - 1>::value;
};

// -----------------------------------------------
// Base case: 0! = 1
// -----------------------------------------------
template <>
struct Factorial<0> {
    static constexpr unsigned long long value = 1;
};

// Compile-time trace for Factorial<5>:
//   Factorial<5>::value = 5 * Factorial<4>::value
//   Factorial<4>::value = 4 * Factorial<3>::value
//   Factorial<3>::value = 3 * Factorial<2>::value
//   Factorial<2>::value = 2 * Factorial<1>::value
//   Factorial<1>::value = 1 * Factorial<0>::value
//   Factorial<0>::value = 1 (base case)
//   → 5 * 4 * 3 * 2 * 1 * 1 = 120

// This is all resolved at compile time — no runtime cost!
static_assert(Factorial<5>::value == 120);
static_assert(Factorial<10>::value == 3628800);
```

### Compile-Time Fibonacci

```cpp
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

static_assert(Fibonacci<10>::value == 55);
static_assert(Fibonacci<20>::value == 6765);
```

### Modern Alternative: `constexpr` Functions

Since C++11 (and greatly improved in C++14/17), `constexpr` functions offer a much cleaner way to do compile-time computation:

```cpp
// Same result, much more readable
constexpr unsigned long long factorial(unsigned n) {
    unsigned long long result = 1;
    for (unsigned i = 2; i <= n; ++i)
        result *= i;
    return result;
}

constexpr unsigned long long fibonacci(unsigned n) {
    if (n <= 1) return n;
    unsigned long long a = 0, b = 1;
    for (unsigned i = 2; i <= n; ++i) {
        unsigned long long next = a + b;
        a = b;
        b = next;
    }
    return b;
}

static_assert(factorial(5) == 120);
static_assert(fibonacci(10) == 55);
```

> **Rule of thumb:** Prefer `constexpr` functions for compile-time computation in modern C++.
> Use template metaprogramming when you need type-level computation (manipulating types, not just values).

---

## 18. Type Erasure

Type erasure is a technique that combines templates with runtime polymorphism to create objects that can hold **any type** that satisfies certain requirements — without the user needing to know about templates.

### The Problem

You want a variable that can hold "anything callable with signature `int(int)`" — functions, lambdas, functors — all in the same variable.

### The Technique

Three layers:

1. **Concept** (abstract base) — defines the interface
2. **Model** (template derived) — wraps any concrete type
3. **Wrapper** (the public class) — hides everything behind a clean API

```cpp
#include <memory>

class AnyCallable {
    // -----------------------------------------------
    // Layer 1: Concept — abstract interface
    // -----------------------------------------------
    struct Concept {
        virtual int call(int arg) = 0;
        virtual ~Concept() = default;
    };

    // -----------------------------------------------
    // Layer 2: Model — template that wraps ANY callable
    // -----------------------------------------------
    template <typename F>
    struct Model : Concept {
        F func;
        Model(F f) : func(std::move(f)) {}
        int call(int arg) override { return func(arg); }
        //                 ^^^^^^^^
        //  Virtual dispatch at runtime — the "erasure" of the original type
    };

    // -----------------------------------------------
    // The only member: a pointer to the abstract Concept
    // -----------------------------------------------
    std::unique_ptr<Concept> impl;

public:
    // -----------------------------------------------
    // Layer 3: Template constructor — accepts ANYTHING callable
    // -----------------------------------------------
    template <typename F>
    AnyCallable(F f) : impl(std::make_unique<Model<F>>(std::move(f))) {}
    //          ^
    //  The template parameter F is "erased" here
    //  After construction, only the Concept interface remains

    int operator()(int arg) {
        return impl->call(arg);
    }
};

// -----------------------------------------------
// Usage — all stored in the same type!
// -----------------------------------------------
int doubleIt(int x) { return x * 2; }

struct Squarer {
    int operator()(int x) { return x * x; }
};

int main() {
    AnyCallable f1(doubleIt);                    // Stores a function pointer
    AnyCallable f2(Squarer{});                   // Stores a functor
    AnyCallable f3([](int x) { return x + 100; }); // Stores a lambda

    // All three have the SAME type: AnyCallable
    std::cout << f1(5) << "\n";  // 10
    std::cout << f2(5) << "\n";  // 25
    std::cout << f3(5) << "\n";  // 105
}
```

> **Real-world example:** `std::function<int(int)>` is exactly this pattern!
> It uses type erasure internally to store any callable matching the signature.

---

## 19. Best Practices & Common Pitfalls

### Where to Put Template Code

Templates must be visible at the point of instantiation. In practice, this means:

```cpp
// ✅ DO: Define templates in header files
// mytemplate.h
template <typename T>
T maxVal(T a, T b) {
    return (a > b) ? a : b;
}

// ❌ DON'T: Put template definitions in .cpp files
// This causes linker errors because other translation units can't see the definition
```

### Avoid Overly Complex SFINAE

```cpp
// ❌ Hard to read SFINAE
template <typename T,
          typename = std::enable_if_t<
              std::is_arithmetic_v<T> &&
              !std::is_same_v<T, bool> &&
              std::is_signed_v<T>>>
void process(T val) { ... }

// ✅ Prefer concepts (C++20)
template <typename T>
    requires std::is_arithmetic_v<T> && (!std::is_same_v<T, bool>) && std::is_signed_v<T>
void process(T val) { ... }

// ✅ Even better: name the concept
template <typename T>
concept SignedNumber = std::is_arithmetic_v<T> && !std::is_same_v<T, bool> && std::is_signed_v<T>;

template <SignedNumber T>
void process(T val) { ... }
```

### Common Mistakes

**1. Forgetting `typename` for dependent types:**
```cpp
template <typename T>
void example() {
    // ❌ The compiler doesn't know T::value_type is a type
    // T::value_type x;

    // ✅ Use typename to tell the compiler it's a type
    typename T::value_type x;
}
```

**2. Forgetting `template` keyword for dependent template members:**
```cpp
template <typename T>
void example(T& obj) {
    // ❌ Parser confusion
    // obj.get<int>();

    // ✅ Explicitly tell the compiler it's a template
    obj.template get<int>();
}
```

**3. Infinite template recursion:**
```cpp
// ❌ Missing base case → infinite recursion at compile time
template <unsigned N>
struct Bad {
    static constexpr int value = Bad<N - 1>::value; // Never stops!
};

// ✅ Always provide a base case specialization
template <>
struct Bad<0> {
    static constexpr int value = 1;
};
```

### Compilation Tips

```bash
# Compile with C++17 (for fold expressions, if constexpr)
g++ -std=c++17 -o program program.cpp

# Compile with C++20 (for concepts)
g++ -std=c++20 -o program program.cpp

# Extra warnings — recommended when learning
g++ -std=c++20 -Wall -Wextra -pedantic -o program program.cpp
```

---

> **Where to go next:** Once comfortable with these topics, explore:
> expression templates, policy-based design, tag dispatching,
> `std::variant` + `std::visit`, and the ranges library (C++20).
