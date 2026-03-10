# Задачи — Функции от по-висок ред в C++

---

## Задача 1 — Приложи функция върху всеки елемент

Напиши функция `applyToAll`, която приема масив, неговия размер и функция, и **принтира резултата** от прилагането на функцията върху всеки елемент.

**Пример:**
```
Вход:  {1, 2, 3, 4, 5}, умножи по 3
Изход: 3 6 9 12 15
```

<details>
<summary>Решение</summary>

```cpp
#include <iostream>
#include <functional>
using namespace std;

void applyToAll(int arr[], int size, function<int(int)> f) {
    for (int i = 0; i < size; i++) {
        cout << f(arr[i]) << " ";
    }
    cout << endl;
}

int main() {
    int numbers[] = {1, 2, 3, 4, 5};
    applyToAll(numbers, 5, [](int n) { return n * 3; });
    return 0;
}
```
</details>

---

## Задача 2 — Намери първия елемент по условие

Напиши функция `findFirst`, която приема масив, размер и предикат, и връща **първия елемент**, за който предикатът е верен. Ако няма такъв — върни `-1`.

**Пример:**
```
Вход:  {3, 7, 2, 8, 5}, по-голямо от 6
Изход: 7
```

<details>
<summary>Решение</summary>

```cpp
#include <iostream>
#include <functional>
using namespace std;

int findFirst(int arr[], int size, function<bool(int)> predicate) {
    for (int i = 0; i < size; i++) {
        if (predicate(arr[i])) {
            return arr[i];
        }
    }
    return -1;
}

int main() {
    int numbers[] = {3, 7, 2, 8, 5};
    int result = findFirst(numbers, 5, [](int n) { return n > 6; });
    cout << result << endl; // 7
    return 0;
}
```
</details>

---

## Задача 3 — Преброй елементите по условие

Напиши функция `countIf`, която приема масив, размер и предикат, и връща **броя на елементите**, за които предикатът е верен.

**Пример:**
```
Вход:  {1, 2, 3, 4, 5, 6}, четни числа
Изход: 3
```

<details>
<summary>Решение</summary>

```cpp
#include <iostream>
#include <functional>
using namespace std;

int countIf(int arr[], int size, function<bool(int)> predicate) {
    int count = 0;
    for (int i = 0; i < size; i++) {
        if (predicate(arr[i])) {
            count++;
        }
    }
    return count;
}

int main() {
    int numbers[] = {1, 2, 3, 4, 5, 6};
    cout << countIf(numbers, 6, [](int n) { return n % 2 == 0; }); // 3
    return 0;
}
```
</details>

---

## Задача 4 — Провери дали всички елементи отговарят на условие

Напиши функция `allMatch`, която връща `true`, ако **всички елементи** от масива отговарят на зададения предикат, и `false` в противен случай.

**Пример:**
```
Вход:  {2, 4, 6, 8}, всички четни?
Изход: true
```

<details>
<summary>Решение</summary>

```cpp
#include <iostream>
#include <functional>
using namespace std;

bool allMatch(int arr[], int size, function<bool(int)> predicate) {
    for (int i = 0; i < size; i++) {
        if (!predicate(arr[i])) {
            return false;
        }
    }
    return true;
}

int main() {
    int numbers[] = {2, 4, 6, 8};
    cout << boolalpha << allMatch(numbers, 4, [](int n) { return n % 2 == 0; }); // true
    return 0;
}
```
</details>

---

## Задача 5 — Провери дали поне един елемент отговаря на условие

Напиши функция `anyMatch`, която връща `true`, ако **поне един елемент** от масива отговаря на предиката.

**Пример:**
```
Вход:  {1, 3, 5, 4, 7}, има ли четно?
Изход: true
```

<details>
<summary>Решение</summary>

```cpp
#include <iostream>
#include <functional>
using namespace std;

bool anyMatch(int arr[], int size, function<bool(int)> predicate) {
    for (int i = 0; i < size; i++) {
        if (predicate(arr[i])) {
            return true;
        }
    }
    return false;
}

int main() {
    int numbers[] = {1, 3, 5, 4, 7};
    cout << boolalpha << anyMatch(numbers, 5, [](int n) { return n % 2 == 0; }); // true
    return 0;
}
```
</details>

---

## Задача 6 — Намери максимума по критерий

Напиши функция `maxBy`, която приема масив, размер и функция за оценка, и връща **елемента с най-висока оценка**.

**Пример:**
```
Вход:  {-5, 3, -1, 4, -2}, по абсолютна стойност
Изход: -5
```

<details>
<summary>Решение</summary>

```cpp
#include <iostream>
#include <functional>
#include <cmath>
using namespace std;

int maxBy(int arr[], int size, function<int(int)> score) {
    int best = arr[0];
    for (int i = 1; i < size; i++) {
        if (score(arr[i]) > score(best)) {
            best = arr[i];
        }
    }
    return best;
}

int main() {
    int numbers[] = {-5, 3, -1, 4, -2};
    int result = maxBy(numbers, 5, [](int n) { return abs(n); });
    cout << result << endl; // -5
    return 0;
}
```
</details>

---

## Задача 7 — Compose: комбинирай две функции

Напиши функция `compose`, която приема две функции `f` и `g`, и връща нова функция, която прилага първо `g`, после `f` върху аргумента (т.е. `f(g(x))`).

**Пример:**
```
f = умножи по 2
g = добави 3
compose(f, g)(5) -> f(g(5)) -> f(8) -> 16
```

<details>
<summary>Решение</summary>

```cpp
#include <iostream>
#include <functional>
using namespace std;

function<int(int)> compose(function<int(int)> f, function<int(int)> g) {
    return [f, g](int x) {
        return f(g(x));
    };
}

int main() {
    auto f = [](int x) { return x * 2; };
    auto g = [](int x) { return x + 3; };

    auto fg = compose(f, g);
    cout << fg(5) << endl; // 16
    return 0;
}
```
</details>

---

## Задача 8 — Приложи функция N пъти

Напиши функция `applyNTimes`, която приема функция `f`, начална стойност `x` и число `n`, и прилага `f` върху `x` точно `n` пъти.

**Пример:**
```
f = умножи по 2, x = 1, n = 4
1 -> 2 -> 4 -> 8 -> 16
Изход: 16
```

<details>
<summary>Решение</summary>

```cpp
#include <iostream>
#include <functional>
using namespace std;

int applyNTimes(function<int(int)> f, int x, int n) {
    for (int i = 0; i < n; i++) {
        x = f(x);
    }
    return x;
}

int main() {
    auto result = applyNTimes([](int x) { return x * 2; }, 1, 4);
    cout << result << endl; // 16
    return 0;
}
```
</details>

---

## Задача 9 — Трансформирай и сумирай (map + reduce)

Напиши програма, която използва `map` и `reduce`, за да намери **сумата на квадратите** на всички елементи в масив.

**Пример:**
```
Вход:  {1, 2, 3, 4}
Изход: 1 + 4 + 9 + 16 = 30
```

<details>
<summary>Решение</summary>

```cpp
#include <iostream>
#include <functional>
using namespace std;

void map(int arr[], int result[], int size, function<int(int)> transform) {
    for (int i = 0; i < size; i++) {
        result[i] = transform(arr[i]);
    }
}

int reduce(int arr[], int size, int initial, function<int(int, int)> combine) {
    int result = initial;
    for (int i = 0; i < size; i++) {
        result = combine(result, arr[i]);
    }
    return result;
}

int main() {
    int numbers[] = {1, 2, 3, 4};
    int size = 4;
    int squared[4];

    map(numbers, squared, size, [](int n) { return n * n; });

    int sum = reduce(squared, size, 0, [](int acc, int n) { return acc + n; });

    cout << sum << endl; // 30
    return 0;
}
```
</details>

---

## Задача 10 — Сортирай масив с comparator

Напиши функция `bubbleSort`, която приема масив, размер и функция `comparator`, и сортира масива **според подадения критерий** (по-голямо или по-малко).

**Пример:**
```
Вход:  {5, 2, 8, 1, 4}
Изход (възходящо): 1 2 4 5 8
Изход (низходящо): 8 5 4 2 1
```

<details>
<summary>Решение</summary>

```cpp
#include <iostream>
#include <functional>
using namespace std;

void bubbleSort(int arr[], int size, function<bool(int, int)> comparator) {
    for (int i = 0; i < size - 1; i++) {
        for (int j = 0; j < size - i - 1; j++) {
            if (comparator(arr[j], arr[j + 1])) {
                int temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
    }
}

int main() {
    int numbers[] = {5, 2, 8, 1, 4};
    int size = 5;

    // Възходящо
    bubbleSort(numbers, size, [](int a, int b) { return a > b; });
    for (int i = 0; i < size; i++) cout << numbers[i] << " "; // 1 2 4 5 8
    cout << endl;

    // Низходящо
    bubbleSort(numbers, size, [](int a, int b) { return a < b; });
    for (int i = 0; i < size; i++) cout << numbers[i] << " "; // 8 5 4 2 1
    cout << endl;

    return 0;
}
```
</details>

---

## Задача 11 — Генератор на функции (връщане на функция)

Напиши функция `makeMultiplier`, която приема число `n` и **връща нова функция**, която умножава своя аргумент по `n`.

**Пример:**
```
auto triple = makeMultiplier(3);
triple(5)  -> 15
triple(10) -> 30
```

<details>
<summary>Решение</summary>

```cpp
#include <iostream>
#include <functional>
using namespace std;

function<int(int)> makeMultiplier(int n) {
    return [n](int x) {
        return x * n;
    };
}

int main() {
    auto triple = makeMultiplier(3);
    auto double_ = makeMultiplier(2);

    cout << triple(5)  << endl; // 15
    cout << triple(10) << endl; // 30
    cout << double_(7) << endl; // 14
    return 0;
}
```
</details>

---

## Задача 12 — Филтрирай и принтирай по два критерия

Напиши функция `filter`, която приема масив, размер и предикат. После я извикай два пъти — веднъж за числа **по-големи от 3**, и веднъж за числа **кратни на 3** — и принтирай резултатите.

**Пример:**
```
Вход: {1, 3, 5, 6, 9, 2, 4}
По-големи от 3:  5 6 9 4
Кратни на 3:     3 6 9
```

<details>
<summary>Решение</summary>

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
    int numbers[] = {1, 3, 5, 6, 9, 2, 4};
    int size = 7;

    cout << "По-големи от 3: ";
    filter(numbers, size, [](int n) { return n > 3; });

    cout << "Кратни на 3:    ";
    filter(numbers, size, [](int n) { return n % 3 == 0; });

    return 0;
}
```
</details>
