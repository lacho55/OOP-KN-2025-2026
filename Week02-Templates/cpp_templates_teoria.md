# C++ Templates — Основна теория

## Съдържание

1. [Какво е template?](#1-какво-е-template)
2. [Function templates — шаблонни функции](#2-function-templates--шаблонни-функции)
3. [Class templates — шаблонни класове](#3-class-templates--шаблонни-класове)
4. [Template с няколко типа параметъра](#4-template-с-няколко-типа-параметъра)
5. [Default template параметри](#5-default-template-параметри)
6. [Шаблони с не-типов параметър](#6-шаблони-с-не-типов-параметър)
7. [Кога компилаторът генерира кода?](#7-кога-компилаторът-генерира-кода)
8. [Честа грешка](#8-честа-грешка)
9. [Обобщение](#9-обобщение)

---

## 1. Какво е template?

Представи си, че искаш да напишеш функция, която връща по-голямото от две числа.  
Ако числата са `int`, ще напишеш:

```cpp
int max(int a, int b) {
    return (a > b) ? a : b;
}
```

Но какво ако искаш да го правиш и с `double`, и с `float`, и с `char`?  
Без templates трябва да пишеш една и съща функция три пъти!

**Templates решават точно този проблем.**  
Казваш на компилатора: *„Напиши тази функция за какъвто тип поискам."*

---

## 2. Function templates — шаблонни функции

### Синтаксис

```cpp
template <typename T>
T functionName(T parameter) {
    // тялото на функцията
}
```

- `template <typename T>` — казваш „тази функция е шаблон, `T` е типът"
- `T` е просто име (placeholder). Може да е `T`, `Type`, `MyType` — каквото искаш.
- Когато извикаш функцията, компилаторът замества `T` с реалния тип.

### Пример: намиране на максимум

```cpp
#include <iostream>
using namespace std;

template <typename T>
T myMax(T a, T b) {
    return (a > b) ? a : b;
}

int main() {
    cout << myMax(3, 7)       << endl; // T = int    → извежда 7
    cout << myMax(3.5, 2.1)   << endl; // T = double → извежда 3.5
    cout << myMax('a', 'z')   << endl; // T = char   → извежда z
    return 0;
}
```

**Какво се случва зад кулисите?**  
Компилаторът вижда `myMax(3, 7)` и автоматично генерира:

```cpp
int myMax(int a, int b) {
    return (a > b) ? a : b;
}
```

Вижда `myMax(3.5, 2.1)` и генерира:

```cpp
double myMax(double a, double b) {
    return (a > b) ? a : b;
}
```

Ти пишеш веднъж — компилаторът прави останалото.

---

### Явно задаване на типа

Можеш и сам да кажеш какъв тип да се използва:

```cpp
myMax<int>(3, 7);     // изрично казваш: използвай int
myMax<double>(3, 7);  // изрично казваш: използвай double
```

---

### Пример: размяна на две стойности

```cpp
#include <iostream>
using namespace std;

template <typename T>
void mySwap(T& a, T& b) {
    T temp = a;
    a = b;
    b = temp;
}

int main() {
    int x = 5, y = 10;
    mySwap(x, y);
    cout << x << " " << y << endl; // извежда: 10 5

    double p = 1.5, q = 2.5;
    mySwap(p, q);
    cout << p << " " << q << endl; // извежда: 2.5 1.5

    return 0;
}
```

---

### Пример: отпечатване на масив

```cpp
#include <iostream>
using namespace std;

template <typename T>
void printArray(T arr[], int size) {
    for (int i = 0; i < size; i++) {
        cout << arr[i] << " ";
    }
    cout << endl;
}

int main() {
    int    nums[] = {1, 2, 3, 4, 5};
    double vals[] = {1.1, 2.2, 3.3};
    char   chars[] = {'a', 'b', 'c'};

    printArray(nums,  5); // 1 2 3 4 5
    printArray(vals,  3); // 1.1 2.2 3.3
    printArray(chars, 3); // a b c

    return 0;
}
```

---

## 3. Class templates — шаблонни класове

Точно като функциите, можем да правим и **шаблонни класове**.  
Полезно е когато искаш клас да работи с различни типове данни.

### Синтаксис

```cpp
template <typename T>
class ClassName {
    T member;
public:
    ClassName(T val) : member(val) {}
    T getMember() { return member; }
};
```

### Пример: кутия (Box)

```cpp
#include <iostream>
using namespace std;

template <typename T>
class Box {
    T value;
public:
    Box(T val) : value(val) {}

    T getValue() {
        return value;
    }

    void setValue(T val) {
        value = val;
    }
};

int main() {
    Box<int>    intBox(42);
    Box<double> dblBox(3.14);
    Box<string> strBox("Здравей!");

    cout << intBox.getValue() << endl; // 42
    cout << dblBox.getValue() << endl; // 3.14
    cout << strBox.getValue() << endl; // Здравей!

    return 0;
}
```

> **Важно:** При class templates трябва **изрично** да кажеш типа: `Box<int>`, `Box<double>`.  
> При function templates компилаторът може да го познае сам от аргументите.

---

### Пример: прост стек (Stack)

```cpp
#include <iostream>
using namespace std;

template <typename T>
class Stack {
    T data[100];
    int top;
public:
    Stack() : top(-1) {}

    void push(T val) {
        data[++top] = val;
    }

    T pop() {
        return data[top--];
    }

    bool isEmpty() {
        return top == -1;
    }
};

int main() {
    Stack<int> s;
    s.push(10);
    s.push(20);
    s.push(30);

    cout << s.pop() << endl; // 30
    cout << s.pop() << endl; // 20

    Stack<string> ss;
    ss.push("Hello");
    ss.push("World");
    cout << ss.pop() << endl; // World

    return 0;
}
```

---

## 4. Template с няколко типа параметъра

Можеш да имаш повече от един template параметър:

```cpp
#include <iostream>
using namespace std;

template <typename T1, typename T2>
void printPair(T1 a, T2 b) {
    cout << "(" << a << ", " << b << ")" << endl;
}

int main() {
    printPair(1, 3.14);        // (1, 3.14)
    printPair("hello", 42);    // (hello, 42)
    printPair('A', true);      // (A, 1)
    return 0;
}
```

---

### Пример: клас Pair

```cpp
#include <iostream>
using namespace std;

template <typename T1, typename T2>
class Pair {
public:
    T1 first;
    T2 second;

    Pair(T1 f, T2 s) : first(f), second(s) {}

    void print() {
        cout << first << " : " << second << endl;
    }
};

int main() {
    Pair<string, int>    p1("Възраст", 25);
    Pair<double, double> p2(3.14, 2.71);

    p1.print(); // Възраст : 25
    p2.print(); // 3.14 : 2.71

    return 0;
}
```

---

## 5. Default template параметри

Можеш да зададеш стойност по подразбиране за template параметър:

```cpp
#include <iostream>
using namespace std;

template <typename T = int>   // по подразбиране T е int
class Container {
    T value;
public:
    Container(T v) : value(v) {}
    void print() { cout << value << endl; }
};

int main() {
    Container<>     c1(42);    // T = int (по подразбиране)
    Container<double> c2(3.14); // T = double

    c1.print(); // 42
    c2.print(); // 3.14

    return 0;
}
```

---

## 6. Шаблони с не-типов параметър

Template параметрите не трябва да са само типове — могат да са и **числа**:

```cpp
#include <iostream>
using namespace std;

template <typename T, int SIZE>
class FixedArray {
    T data[SIZE];
public:
    void set(int index, T val) {
        data[index] = val;
    }
    T get(int index) {
        return data[index];
    }
    int size() { return SIZE; }
};

int main() {
    FixedArray<int, 5>    arr;   // масив от 5 int-а
    FixedArray<double, 3> dArr;  // масив от 3 double-а

    arr.set(0, 10);
    arr.set(1, 20);
    cout << arr.get(0) << endl; // 10
    cout << arr.size() << endl; // 5

    return 0;
}
```

> Размерът на масива е фиксиран по **времето на компилация** — много ефективно!

---

## 7. Кога компилаторът генерира кода?

Templates работят на принципа **"code generation at compile time"** (генериране на код по времето на компилация).

```
Ти пишеш:          template<typename T> T myMax(T a, T b) { ... }

Компилаторът вижда: myMax(3, 7)    → генерира int    версия
                    myMax(1.5, 2.5) → генерира double версия
                    myMax('a', 'b') → генерира char   версия
```

Ако никога не извикаш `myMax` с `string`, компилаторът **няма да генерира** `string` версия.

---

## 8. Честа грешка

```cpp
template <typename T>
T myMax(T a, T b) {
    return (a > b) ? a : b;
}

// ❌ ГРЕШКА: компилаторът не знае как да определи T
myMax(3, 3.14);  // 3 е int, 3.14 е double — различни типове!

// ✅ ПРАВИЛНО: кажи изрично типа
myMax<double>(3, 3.14);  // компилаторът преобразува 3 → 3.0
```

---

## 9. Обобщение

| Концепция | Синтаксис | Кога се използва |
|---|---|---|
| Function template | `template <typename T> T f(T a)` | Функции за различни типове |
| Class template | `template <typename T> class C {}` | Класове за различни типове |
| Много параметри | `template <typename T1, typename T2>` | Нужни са 2+ различни типа |
| Default параметър | `template <typename T = int>` | Има най-честа стойност |
| Числов параметър | `template <typename T, int N>` | Размери, константи |

---

> **Запомни:** Templates са начин да кажеш на компилатора „напиши тази функция/клас за всеки тип, който поискам". Ти пишеш кода **веднъж** — компилаторът го генерира за всеки конкретен тип.
