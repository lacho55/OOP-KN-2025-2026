# Функции от по-висок ред в C++

Функциите от по-висок ред са функции, които **приемат други функции като аргументи** или **връщат функция като резултат**. Това е мощна концепция, която ни позволява да пишем по-гъвкав и преизползваем код.

---

## 1. Указатели към функции (Function Pointers)

Най-базовият начин да подадем функция като аргумент е чрез **указател към функция**.

```cpp
#include <iostream>
using namespace std;

int collect(int a, int b) {
    return a + b;
}

int multiply(int a, int b) {
    return a * b;
}

// Функция, която приема друга функция като аргумент
int calculate(int x, int y, int (*operation)(int, int)) {
    return operation(x, y);
}

int main() {
    cout << calculate(3, 4, collect);    // 7
    cout << calculate(3, 4, multiply);  // 12
    return 0;
}
```

> `int (*operation)(int, int)` означава: указател към функция, която приема два `int` и връща `int`.

---

## 2. Lambda функции

**Lambda** е анонимна (безименна) функция, която можем да дефинираме директно на място.

```cpp
#include <iostream>
using namespace std;

int calculate(int x, int y, int (*op)(int, int)) {
    return op(x, y);
}

int main() {
    // Lambda функция директно като аргумент
    auto result = calculate(5, 3, [](int a, int b) {
        return a - b;
    });

    cout << result; // 2
    return 0;
}
```

### Синтаксис на lambda:

```
[capture](параметри) -> тип_връщане { тяло }
```

| Част | Значение |
|---|---|
| `[]` | Capture list – какво да "улови" от околния scope |
| `(int a, int b)` | Параметри, като при нормална функция |
| `-> int` | Тип на връщаната стойност (незадължително) |
| `{ return a + b; }` | Тялото на функцията |

---

## 3. std::function

`std::function` е по-гъвкав начин да съхраняваме и подаваме функции — работи с обикновени функции, ламбди и методи.

```cpp
#include <iostream>
#include <functional>
using namespace std;

void applyTwice(int x, function<int(int)> f) {
    cout << f(f(x)) << endl;
}

int main() {
    applyTwice(2, [](int n) { return n * 3; }); // 2 -> 6 -> 18
    applyTwice(1, [](int n) { return n + 10; }); // 1 -> 11 -> 21
    return 0;
}
```

---

## 4. Capture в Lambda

Lambda може да "улови" променливи от обкръжаващия я scope.

```cpp
#include <iostream>
using namespace std;

int main() {
    int multiplier = 5;

    // Улавяме multiplier по стойност
    auto times = [multiplier](int x) {
        return x * multiplier;
    };

    cout << times(3);  // 15
    cout << times(10); // 50
    return 0;
}
```

### Видове capture:

| Синтаксис | Значение |
|---|---|
| `[]` | Нищо не се улавя |
| `[x]` | Улавя `x` по стойност |
| `[&x]` | Улавя `x` по референция |
| `[=]` | Улавя всичко по стойност |
| `[&]` | Улавя всичко по референция |

---

## 5. Практически пример — филтриране на масив

```cpp
#include <iostream>
#include <functional>
using namespace std;

void filter(int arr[], int size, function<bool(int)> predicate) {
    for (int i = 0; i < size; i++) {
        if (predicate(arr[i])) {
            cout << arr[i] << " ";
        }
    }
    cout << endl;
}

int main() {
    int numbers[] = {1, 2, 3, 4, 5, 6, 7, 8};
    int size = 8;

    // Филтрираме само четните числа
    filter(numbers, size, [](int n) { return n % 2 == 0; }); // 2 4 6 8

    // Филтрираме само нечетните числа
    filter(numbers, size, [](int n) { return n % 2 != 0; }); // 1 3 5 7

    return 0;
}
```

---

## 6. Практически пример — map

**map** прилага дадена функция върху всеки елемент от масива и записва резултата в нов масив.

```cpp
#include <iostream>
#include <functional>
using namespace std;

void map(int arr[], int result[], int size, function<int(int)> transform) {
    for (int i = 0; i < size; i++) {
        result[i] = transform(arr[i]);
    }
}

int main() {
    int numbers[] = {1, 2, 3, 4, 5};
    int size = 5;
    int doubled[5];
    int squared[5];

    // Умножаваме всеки елемент по 2
    map(numbers, doubled, size, [](int n) { return n * 2; });

    // Вдигаме на квадрат всеки елемент
    map(numbers, squared, size, [](int n) { return n * n; });

    cout << "Doubled: ";
    for (int i = 0; i < size; i++) cout << doubled[i] << " "; // 2 4 6 8 10
    cout << endl;

    cout << "Squared: ";
    for (int i = 0; i < size; i++) cout << squared[i] << " "; // 1 4 9 16 25
    cout << endl;

    return 0;
}
```

---

## 7. Практически пример — reduce

**reduce** обхожда масива и го "свива" до една единствена стойност, като комбинира елементите чрез подадена функция.

```cpp
#include <iostream>
#include <functional>
using namespace std;

int reduce(int arr[], int size, int initial, function<int(int, int)> combine) {
    int result = initial;
    for (int i = 0; i < size; i++) {
        result = combine(result, arr[i]);
    }
    return result;
}

int main() {
    int numbers[] = {1, 2, 3, 4, 5};
    int size = 5;

    // Сума на всички елементи
    int sum = reduce(numbers, size, 0, [](int acc, int n) { return acc + n; });

    // Произведение на всички елементи
    int product = reduce(numbers, size, 1, [](int acc, int n) { return acc * n; });

    cout << "Sum: " << sum << endl;         // 15
    cout << "Product: " << product << endl; // 120

    return 0;
}
```

> `acc` (accumulator) е натрупаната стойност до момента. При всяка стъпка я комбинираме със следващия елемент.

---

## Обобщение

| Концепция | Кога да използваме |
|---|---|
| **Указател към функция** | Прост случай, без улавяне на контекст |
| **Lambda** | Бърза анонимна функция на място |
| **std::function** | Когато искаме гъвкавост (съхраняване, подаване) |
| **Capture** | Когато lambda трябва да използва локални променливи |

---

> **Запомни:** Функциите от по-висок ред правят кода ни **по-модулен и преизползваем** — вместо да пишем отделна функция за всеки случай, подаваме поведението като аргумент.
