# C++ Templates — Задачи за упражнение


## Задача 1 — Шаблонна функция за минимум

Напиши функция `myMin`, която приема два аргумента от произволен тип и връща по-малкия.  
Тествай я с `int`, `double` и `char`.

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
using namespace std;

template <typename T>
T myMin(T a, T b) {
    return (a < b) ? a : b;
}

int main() {
    cout << myMin(5, 3)       << endl; // 3
    cout << myMin(1.7, 2.5)   << endl; // 1.7
    cout << myMin('a', 'z')   << endl; // a
    return 0;
}
```

</details>

---

## Задача 2 — Шаблонна функция за сума на масив

Напиши функция `arraySum`, която приема масив и неговия размер и връща сумата на всички елементи.  
Тествай я с масив от `int` и масив от `double`.

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
using namespace std;

template <typename T>
T arraySum(T arr[], int size) {
    T sum = 0;
    for (int i = 0; i < size; i++) {
        sum += arr[i];
    }
    return sum;
}

int main() {
    int    nums[] = {1, 2, 3, 4, 5};
    double vals[] = {1.1, 2.2, 3.3};

    cout << arraySum(nums, 5) << endl; // 15
    cout << arraySum(vals, 3) << endl; // 6.6
    return 0;
}
```

</details>

---

## Задача 3 — Шаблонна функция за проверка дали са равни

Напиши функция `areEqual`, която приема два аргумента и връща `true` ако са равни, `false` ако не са.  
Тествай я с различни типове.

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
using namespace std;

template <typename T>
bool areEqual(T a, T b) {
    return a == b;
}

int main() {
    cout << areEqual(5, 5)       << endl; // 1 (true)
    cout << areEqual(3.14, 3.15) << endl; // 0 (false)
    cout << areEqual('a', 'a')   << endl; // 1 (true)
    return 0;
}
```

</details>

---

## Задача 4 — Шаблонна функция за степенуване

Напиши функция `power`, която приема база и цял показател `n` и връща `база^n`.  
*(Подсказка: използвай цикъл, не `pow` от библиотеката.)*

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
using namespace std;

template <typename T>
T power(T base, int n) {
    T result = 1;
    for (int i = 0; i < n; i++) {
        result *= base;
    }
    return result;
}

int main() {
    cout << power(2, 10)    << endl; // 1024
    cout << power(3.0, 3)   << endl; // 27
    cout << power(2.5, 2)   << endl; // 6.25
    return 0;
}
```

</details>

---

## Задача 5 — Шаблонен клас „Кутия" (Box)

Напиши шаблонен клас `Box`, който:
- Съхранява една стойност от произволен тип
- Има метод `get()` — връща стойността
- Има метод `set(val)` — задава нова стойност
- Има метод `print()` — отпечатва стойността

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
using namespace std;

template <typename T>
class Box {
    T value;
public:
    Box(T v) : value(v) {}

    T get() { return value; }

    void set(T v) { value = v; }

    void print() {
        cout << "Box съдържа: " << value << endl;
    }
};

int main() {
    Box<int>    b1(100);
    Box<string> b2("Здравей");

    b1.print();       // Box съдържа: 100
    b1.set(200);
    b1.print();       // Box съдържа: 200

    b2.print();       // Box съдържа: Здравей
    return 0;
}
```

</details>

---

## Задача 6 — Размяна без temp променлива

Напиши шаблонна функция `swapNoTemp`, която разменя стойностите на две променливи **без** да използва трета (temp) променлива.  
*(Подсказка: можеш да използваш аритметика или XOR.)*

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
using namespace std;

// Вариант с аритметика (работи за числови типове)
template <typename T>
void swapNoTemp(T& a, T& b) {
    a = a + b;
    b = a - b;
    a = a - b;
}

int main() {
    int x = 5, y = 10;
    swapNoTemp(x, y);
    cout << x << " " << y << endl; // 10 5

    double p = 1.5, q = 3.5;
    swapNoTemp(p, q);
    cout << p << " " << q << endl; // 3.5 1.5

    return 0;
}
```

</details>

---

## Задача 7 — Шаблонна функция за брой на елемент в масив

Напиши функция `countOccurrences`, която приема масив, размер и стойност, и връща колко пъти тази стойност се среща в масива.

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
using namespace std;

template <typename T>
int countOccurrences(T arr[], int size, T value) {
    int count = 0;
    for (int i = 0; i < size; i++) {
        if (arr[i] == value) count++;
    }
    return count;
}

int main() {
    int nums[] = {1, 2, 3, 2, 4, 2, 5};
    cout << countOccurrences(nums, 7, 2) << endl; // 3

    char letters[] = {'a', 'b', 'a', 'c', 'a'};
    cout << countOccurrences(letters, 5, 'a') << endl; // 3

    return 0;
}
```

</details>

---

## Задача 8 — Шаблонен клас „Двойка" (Pair)

Напиши шаблонен клас `Pair` с два template параметъра `T1` и `T2`, който:
- Съхранява две стойности (`first` и `second`)
- Има метод `print()`, който ги отпечатва

<details>
<summary>💡 Решение</summary>

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
        cout << "(" << first << ", " << second << ")" << endl;
    }
};

int main() {
    Pair<string, int>    p1("Иван", 25);
    Pair<double, double> p2(3.14, 2.71);
    Pair<int, bool>      p3(1, true);

    p1.print(); // (Иван, 25)
    p2.print(); // (3.14, 2.71)
    p3.print(); // (1, 1)

    return 0;
}
```

</details>

---

## Задача 9 — Шаблонна функция за намиране на индекс на максимум

Напиши функция `indexOfMax`, която приема масив и неговия размер и връща **индекса** на най-големия елемент.

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
using namespace std;

template <typename T>
int indexOfMax(T arr[], int size) {
    int maxIdx = 0;
    for (int i = 1; i < size; i++) {
        if (arr[i] > arr[maxIdx]) {
            maxIdx = i;
        }
    }
    return maxIdx;
}

int main() {
    int    nums[] = {3, 7, 1, 9, 2};
    double vals[] = {1.5, 3.3, 2.2};

    cout << indexOfMax(nums, 5) << endl; // 3 (стойност 9)
    cout << indexOfMax(vals, 3) << endl; // 1 (стойност 3.3)

    return 0;
}
```

</details>

---

## Задача 10 — Шаблонен клас „Масив" с фиксиран размер

Напиши шаблонен клас `FixedArray` с параметри `T` (тип) и `N` (размер), който:
- Съхранява `N` елемента от тип `T`
- Има методи `set(index, value)` и `get(index)`
- Има метод `print()`, който отпечатва всички елементи

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
using namespace std;

template <typename T, int N>
class FixedArray {
    T data[N];
public:
    void set(int index, T value) {
        data[index] = value;
    }

    T get(int index) {
        return data[index];
    }

    void print() {
        for (int i = 0; i < N; i++) {
            cout << data[i] << " ";
        }
        cout << endl;
    }

    int size() { return N; }
};

int main() {
    FixedArray<int, 5> arr;
    arr.set(0, 10);
    arr.set(1, 20);
    arr.set(2, 30);
    arr.set(3, 40);
    arr.set(4, 50);
    arr.print(); // 10 20 30 40 50

    FixedArray<double, 3> dArr;
    dArr.set(0, 1.1);
    dArr.set(1, 2.2);
    dArr.set(2, 3.3);
    dArr.print(); // 1.1 2.2 3.3

    return 0;
}
```

</details>

---

## Задача 11 — Шаблонна функция за обръщане на масив

Напиши функция `reverseArray`, която приема масив и размер и го обръща **на място** (in-place).

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
using namespace std;

template <typename T>
void reverseArray(T arr[], int size) {
    int left = 0, right = size - 1;
    while (left < right) {
        T temp = arr[left];
        arr[left]  = arr[right];
        arr[right] = temp;
        left++;
        right--;
    }
}

int main() {
    int nums[] = {1, 2, 3, 4, 5};
    reverseArray(nums, 5);
    for (int i = 0; i < 5; i++) cout << nums[i] << " ";
    cout << endl; // 5 4 3 2 1

    char letters[] = {'a', 'b', 'c', 'd'};
    reverseArray(letters, 4);
    for (int i = 0; i < 4; i++) cout << letters[i] << " ";
    cout << endl; // d c b a

    return 0;
}
```

</details>

---

## Задача 12 — Шаблонна функция за проверка дали масив е сортиран

Напиши функция `isSorted`, която приема масив и размер и връща `true` ако масивът е наредена в нарастващ ред, иначе `false`.

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
using namespace std;

template <typename T>
bool isSorted(T arr[], int size) {
    for (int i = 0; i < size - 1; i++) {
        if (arr[i] > arr[i + 1]) return false;
    }
    return true;
}

int main() {
    int sorted[]   = {1, 2, 3, 4, 5};
    int unsorted[] = {1, 3, 2, 4, 5};

    cout << isSorted(sorted,   5) << endl; // 1 (true)
    cout << isSorted(unsorted, 5) << endl; // 0 (false)

    double dArr[] = {1.1, 2.2, 3.3};
    cout << isSorted(dArr, 3) << endl; // 1 (true)

    return 0;
}
```

</details>

---

## Задача 13 — Шаблонна функция с два различни типа

Напиши функция `multiply`, която приема два аргумента от **различни** типове (`T1` и `T2`) и връща тяхното произведение.  
Типът на резултата нека е `T1`.

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
using namespace std;

template <typename T1, typename T2>
T1 multiply(T1 a, T2 b) {
    return a * static_cast<T1>(b);
}

int main() {
    cout << multiply(3, 2.5)   << endl; // 7  (int * double → int)
    cout << multiply(3.0, 2)   << endl; // 6  (double * int → double)
    cout << multiply(2.5, 3.0) << endl; // 7.5

    return 0;
}
```

</details>

---

## Задача 15 — Шаблонен клас „Минимален контейнер"

Напиши шаблонен клас `MinContainer`, който:
- Пази масив от стойности (максимум 50 елемента)
- Има метод `add(val)` — добавя стойност
- Има метод `getMin()` — връща най-малката стойност от всички добавени
- Има метод `size()` — връща броя на добавените елементи

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
using namespace std;

template <typename T>
class MinContainer {
    T data[50];
    int count;
public:
    MinContainer() : count(0) {}

    void add(T val) {
        if (count < 50) {
            data[count++] = val;
        }
    }

    T getMin() {
        T minVal = data[0];
        for (int i = 1; i < count; i++) {
            if (data[i] < minVal) {
                minVal = data[i];
            }
        }
        return minVal;
    }

    int size() { return count; }
};

int main() {
    MinContainer<int> mc;
    mc.add(5);
    mc.add(3);
    mc.add(8);
    mc.add(1);
    mc.add(7);

    cout << "Брой: " << mc.size()   << endl; // 5
    cout << "Мин:  " << mc.getMin() << endl; // 1

    MinContainer<double> mcd;
    mcd.add(2.5);
    mcd.add(1.1);
    mcd.add(3.7);

    cout << "Мин:  " << mcd.getMin() << endl; // 1.1

    return 0;
}
```

</details>

---

