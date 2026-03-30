# C++ ООП — Голяма Четворка & Овърлоудване на Оператори

---

## 🔹 Задача 1 — Конструктор по подразбиране и параметризиран конструктор

**Концепция:** Класът може да има **повече от един конструктор** — C++ избира правилния според подадените аргументи. Конструкторът без параметри се казва **конструктор по подразбиране**.

Създай клас `Box` с полета `width`, `height`, `depth` (дробни числа).

- Конструктор по подразбиране: задава всички размери на `1.0`
- Параметризиран конструктор: приема трите размера
- Метод `volume()` — връща обема

Създай два обекта (единия с параметри, другия без) и отпечатай обемите им.

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
using namespace std;

class Box {
public:
    double width;
    double height;
    double depth;

    // Конструктор по подразбиране
    Box() {
        width  = 1.0;
        height = 1.0;
        depth  = 1.0;
        cout << "Конструктор по подразбиране." << endl;
    }

    // Параметризиран конструктор
    Box(double w, double h, double d) {
        width  = w;
        height = h;
        depth  = d;
        cout << "Параметризиран конструктор: "
             << w << "x" << h << "x" << d << endl;
    }

    double volume() {
        return width * height * depth;
    }
};

int main() {
    Box b1;
    Box b2(3.0, 4.0, 5.0);

    cout << "Обем на b1: " << b1.volume() << endl;
    cout << "Обем на b2: " << b2.volume() << endl;

    return 0;
}
```

**Изход:**
```
Конструктор по подразбиране.
Параметризиран конструктор: 3x4x5
Обем на b1: 1
Обем на b2: 60
```

> 📌 Двата конструктора имат **едно и също** ime — `Box`. C++ различава кой да извика по **броя и типа** на аргументите.

</details>

---

## 🔹 Задача 2 — Деструктор и ред на унищожаване

**Концепция:** Деструкторът се извиква **автоматично** при унищожаване на обекта — когато scope-ът приключи. Обектите се унищожават в **обратен ред** на създаването.

Създай клас `Resource` с поле `name` (char масив с размер 50).

- Конструктор: приема `name`, отпечатва `"[name] е зареден."`
- Деструктор: отпечатва `"[name] е освободен."`

Създай три обекта в `main()` и наблюдавай реда на унищожаване.

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
#include <cstring>
using namespace std;

class Resource {
public:
    char name[50];

    Resource(const char* n) {
        strcpy(name, n);
        cout << name << " е зареден." << endl;
    }

    ~Resource() {
        cout << name << " е освободен." << endl;
    }
};

int main() {
    Resource r1("Текстура");
    Resource r2("Звук");
    Resource r3("Мрежа");

    cout << "--- Програмата работи ---" << endl;

    return 0;
}
```

**Изход:**
```
Текстура е зареден.
Звук е зареден.
Мрежа е зареден.
--- Програмата работи ---
Мрежа е освободена.
Звук е освободен.
Текстура е освободена.
```

> 📌 Унищожаването е в **обратен ред** — LIFO принцип (стек). Последно създаденият се освобождава пръв!

</details>

---

## 🔹 Задача 3 — Копиращ конструктор (Shallow vs Deep Copy)

**Концепция:** Когато клас съдържа **указател**, копирането по подразбиране копира само адреса — двата обекта сочат към **едно и също** място в паметта. Копиращият конструктор прави **дълбоко копие** (собствена памет).

Създай клас `StringWrapper` с поле `char* text`.

- Конструктор: приема `const char*`, заделя памет с `new` и копира текста
- Копиращ конструктор: прави **дълбоко копие**
- Деструктор: освобождава паметта
- `print()` — отпечатва текста

Провери, че промяна в копието **не засяга** оригинала.

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
#include <cstring>
using namespace std;

class StringWrapper {
public:
    char* text;

    // Конструктор
    StringWrapper(const char* t) {
        text = new char[strlen(t) + 1];
        strcpy(text, t);
        cout << "Конструктор: \"" << text << "\"" << endl;
    }

    // Копиращ конструктор — дълбоко копие
    StringWrapper(const StringWrapper& other) {
        text = new char[strlen(other.text) + 1];  // нова памет!
        strcpy(text, other.text);
        cout << "Копиращ конструктор: \"" << text << "\"" << endl;
    }

    // Деструктор
    ~StringWrapper() {
        cout << "Деструктор: \"" << text << "\"" << endl;
        delete[] text;
    }

    void print() {
        cout << "Текст: \"" << text << "\"" << endl;
    }
};

int main() {
    StringWrapper original("Здравей");
    StringWrapper copy = original;   // извиква копиращия конструктор

    // Директна промяна в паметта на копието
    copy.text[0] = 'з';

    cout << "Оригинал: "; original.print();
    cout << "Копие:    "; copy.print();

    return 0;
}
```

**Изход:**
```
Конструктор: "Здравей"
Копиращ конструктор: "Здравей"
Оригинал: "Здравей"
Копие:    "здравей"
Деструктор: "здравей"
Деструктор: "Здравей"
```

> 📌 Без копиращ конструктор промяната `copy.text[0] = 'з'` щеше да засегне и оригинала, защото двата указателя щяха да сочат към **едно и също** място!

</details>

---

## 🔹 Задача 4 — Оператор за присвояване `operator=`

**Концепция:** Операторът за присвояване се извиква при `a = b` (за **вече съществуващи** обекти). За разлика от копиращия конструктор, трябва да: (1) освободи старата памет, (2) заяви нова, (3) копира данните, (4) върне `*this`.

Добави `operator=` към класа `StringWrapper` от задача 3.  
Провери, че присвояването работи правилно и **не се получава** memory leak.

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
#include <cstring>
using namespace std;

class StringWrapper {
public:
    char* text;

    StringWrapper(const char* t) {
        text = new char[strlen(t) + 1];
        strcpy(text, t);
    }

    StringWrapper(const StringWrapper& other) {
        text = new char[strlen(other.text) + 1];
        strcpy(text, other.text);
    }

    // Оператор за присвояване
    StringWrapper& operator=(const StringWrapper& other) {
        if (this == &other) return *this;  // защита от самоприсвояване!

        delete[] text;                     // освобождаваме старата памет

        text = new char[strlen(other.text) + 1];
        strcpy(text, other.text);

        cout << "operator= извикан: \"" << text << "\"" << endl;
        return *this;
    }

    ~StringWrapper() {
        delete[] text;
    }

    void print() {
        cout << "Текст: \"" << text << "\"" << endl;
    }
};

int main() {
    StringWrapper a("Оригинал");
    StringWrapper b("Временен");

    cout << "Преди присвояване: "; b.print();

    b = a;   // извиква operator=

    cout << "След присвояване:  "; b.print();

    // Тест за самоприсвояване
    a = a;
    cout << "След самоприсвояване: "; a.print();

    return 0;
}
```

**Изход:**
```
Преди присвояване: Текст: "Временен"
operator= извикан: "Оригинал"
След присвояване:  Текст: "Оригинал"
След самоприсвояване: Текст: "Оригинал"
```

> 📌 Проверката `if (this == &other)` е **задължителна** — без нея `a = a` ще изтрие паметта преди да я копира!

</details>

---

## 🔸 Задача 5 — Голямата Четворка в един клас

**Концепция:** **Голямата четворка** (Rule of Four) — ако класът управлява динамична памет, трябва да дефинираш **всички четири** специални метода: конструктор, деструктор, копиращ конструктор и `operator=`.

Създай клас `IntArray` с полета `int* data` и `int size`.

Имплементирай **всички четири** специални метода и добави:
- `set(index, value)` — задава стойност
- `get(index)` — връща стойност  
- `print()` — отпечатва елементите

Тествай всеки от четирите специални метода.

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
using namespace std;

class IntArray {
public:
    int* data;
    int  size;

    // 1. Конструктор
    IntArray(int n) {
        size = n;
        data = new int[size];
        for (int i = 0; i < size; i++) data[i] = 0;
        cout << "Конструктор: масив с " << size << " елемента." << endl;
    }

    // 2. Копиращ конструктор
    IntArray(const IntArray& other) {
        size = other.size;
        data = new int[size];
        for (int i = 0; i < size; i++) data[i] = other.data[i];
        cout << "Копиращ конструктор." << endl;
    }

    // 3. Оператор за присвояване
    IntArray& operator=(const IntArray& other) {
        if (this == &other) return *this;

        delete[] data;

        size = other.size;
        data = new int[size];
        for (int i = 0; i < size; i++) data[i] = other.data[i];

        cout << "operator= извикан." << endl;
        return *this;
    }

    // 4. Деструктор
    ~IntArray() {
        delete[] data;
        cout << "Деструктор: масив с " << size << " елемента." << endl;
    }

    void set(int index, int value) {
        if (index >= 0 && index < size)
            data[index] = value;
    }

    int get(int index) {
        if (index >= 0 && index < size)
            return data[index];
        return -1;
    }

    void print() {
        cout << "[ ";
        for (int i = 0; i < size; i++) cout << data[i] << " ";
        cout << "]" << endl;
    }
};

int main() {
    // 1. Конструктор
    IntArray a(4);
    a.set(0, 10); a.set(1, 20); a.set(2, 30); a.set(3, 40);
    cout << "a = "; a.print();

    // 2. Копиращ конструктор
    IntArray b = a;
    b.set(0, 99);
    cout << "a = "; a.print();
    cout << "b = "; b.print();

    // 3. operator=
    IntArray c(2);
    c = a;
    cout << "c = "; c.print();

    return 0;
}
```

**Изход:**
```
Конструктор: масив с 4 елемента.
a = [ 10 20 30 40 ]
Копиращ конструктор.
a = [ 10 20 30 40 ]
b = [ 99 20 30 40 ]
Конструктор: масив с 2 елемента.
operator= извикан.
c = [ 10 20 30 40 ]
Деструктор: масив с 4 елемента.
Деструктор: масив с 4 елемента.
Деструктор: масив с 4 елемента.
```

> 📌 Три деструктора за три обекта — всеки управлява **собствена** памет. Без голямата четворка щеше да се получи **двойно освобождаване** (double free) и **undefined behavior**!

</details>

---

## 🔸 Задача 6 — `operator+` за събиране на вектори

**Концепция:** Овърлоудването на оператори позволява да използваме стандартните C++ оператори (`+`, `-`, `*`, `==`, ...) с наши собствени класове. Операторът се дефинира като метод с ключовата дума `operator`.

Създай клас `Vector2D` с полета `x` и `y` (дробни числа).

Овърлоуднай:
- `operator+` — събира два вектора (компонентно)
- `print()` — отпечатва `"(x, y)"`

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
using namespace std;

class Vector2D {
public:
    double x;
    double y;

    Vector2D(double x = 0, double y = 0) {
        this->x = x;
        this->y = y;
    }

    // operator+
    Vector2D operator+(const Vector2D& other) const {
        return Vector2D(x + other.x, y + other.y);
    }

    void print() {
        cout << "(" << x << ", " << y << ")" << endl;
    }
};

int main() {
    Vector2D v1(3.0, 4.0);
    Vector2D v2(1.0, 2.0);

    Vector2D v3 = v1 + v2;

    cout << "v1 = "; v1.print();
    cout << "v2 = "; v2.print();
    cout << "v1 + v2 = "; v3.print();

    return 0;
}
```

**Изход:**
```
v1 = (3, 4)
v2 = (1, 2)
v1 + v2 = (4, 6)
```

> 📌 `v1 + v2` е еквивалентно на `v1.operator+(v2)`. C++ просто ни дава по-четим синтаксис!

</details>

---

## 🔸 Задача 7 — `operator-`, `operator*` (скалар) и `operator==`

**Концепция:** Можем да овърлоудваме **множество** оператори за един клас. Операторът за умножение по скалар `operator*(double)` взима `double`, а не друг вектор.

Разшири класа `Vector2D` от задача 6:

- `operator-` — изважда два вектора
- `operator*(double scalar)` — умножава вектора по скалар
- `operator==` — проверява дали два вектора са равни
- `length()` — връща дължината на вектора (√(x² + y²))

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
#include <cmath>
using namespace std;

class Vector2D {
public:
    double x;
    double y;

    Vector2D(double x = 0, double y = 0) {
        this->x = x;
        this->y = y;
    }

    Vector2D operator+(const Vector2D& other) const {
        return Vector2D(x + other.x, y + other.y);
    }

    Vector2D operator-(const Vector2D& other) const {
        return Vector2D(x - other.x, y - other.y);
    }

    Vector2D operator*(double scalar) const {
        return Vector2D(x * scalar, y * scalar);
    }

    bool operator==(const Vector2D& other) const {
        return (x == other.x) && (y == other.y);
    }

    double length() const {
        return sqrt(x * x + y * y);
    }

    void print() const {
        cout << "(" << x << ", " << y << ")" << endl;
    }
};

int main() {
    Vector2D v1(6.0, 8.0);
    Vector2D v2(1.0, 2.0);

    cout << "v1 = "; v1.print();
    cout << "v2 = "; v2.print();

    Vector2D diff = v1 - v2;
    cout << "v1 - v2 = "; diff.print();

    Vector2D scaled = v1 * 0.5;
    cout << "v1 * 0.5 = "; scaled.print();

    cout << "Дължина на v1: " << v1.length() << endl;

    cout << "v1 == v2: " << (v1 == v2 ? "да" : "не") << endl;
    cout << "v1 == v1: " << (v1 == v1 ? "да" : "не") << endl;

    return 0;
}
```

**Изход:**
```
v1 = (6, 8)
v2 = (1, 2)
v1 - v2 = (5, 6)
v1 * 0.5 = (3, 4)
Дължина на v1: 10
v1 == v2: не
v1 == v1: да
```

> 📌 `operator==` връща `bool`. Добра практика е да дефинираш и `operator!=` като `return !(*this == other);`.

</details>

---

## 🔸 Задача 8 — `operator<<` за отпечатване (приятелска функция)

**Концепция:** `operator<<` не може да е метод на класа (защото лявата страна е `ostream`, не нашият клас). Дефинираме го като **приятелска функция** (`friend`) с достъп до private полетата.

Добави `operator<<` към `Vector2D`, за да можем да пишем:
```cpp
cout << v1;
```

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
using namespace std;

class Vector2D {
public:
    double x;
    double y;

    Vector2D(double x = 0, double y = 0) {
        this->x = x;
        this->y = y;
    }

    Vector2D operator+(const Vector2D& other) const {
        return Vector2D(x + other.x, y + other.y);
    }

    // Приятелска функция — достъп до полетата, но не е метод
    friend ostream& operator<<(ostream& os, const Vector2D& v) {
        os << "(" << v.x << ", " << v.y << ")";
        return os;  // връщаме os за верижно извикване: cout << a << b;
    }
};

int main() {
    Vector2D v1(3.0, 4.0);
    Vector2D v2(1.0, 2.0);

    cout << "v1 = " << v1 << endl;
    cout << "v2 = " << v2 << endl;
    cout << "v1 + v2 = " << (v1 + v2) << endl;

    // Верижно: 
    cout << v1 << " + " << v2 << " = " << (v1 + v2) << endl;

    return 0;
}
```

**Изход:**
```
v1 = (3, 4)
v2 = (1, 2)
v1 + v2 = (4, 6)
(3, 4) + (1, 2) = (4, 6)
```

> 📌 `operator<<` трябва да връща `ostream&` за да работи **верижното** `cout << a << b << c`. Без `return os` верижното извикване не работи!

</details>

---

## 🔸 Задача 9 — `operator[]` (индексен оператор)

**Концепция:** `operator[]` позволява достъп до елементи по индекс, точно като при масив. Дефинираме две версии — една за четене и писане, друга само за четене (`const`).

Добави `operator[]` към класа `IntArray` от задача 5, за да можем да пишем:
```cpp
arr[2] = 99;
cout << arr[2];
```

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
using namespace std;

class IntArray {
public:
    int* data;
    int  size;

    IntArray(int n) {
        size = n;
        data = new int[size];
        for (int i = 0; i < size; i++) data[i] = 0;
    }

    IntArray(const IntArray& other) {
        size = other.size;
        data = new int[size];
        for (int i = 0; i < size; i++) data[i] = other.data[i];
    }

    IntArray& operator=(const IntArray& other) {
        if (this == &other) return *this;
        delete[] data;
        size = other.size;
        data = new int[size];
        for (int i = 0; i < size; i++) data[i] = other.data[i];
        return *this;
    }

    ~IntArray() { delete[] data; }

    // operator[] за четене И писане
    int& operator[](int index) {
        return data[index];
    }

    // operator[] само за четене (за const обекти)
    const int& operator[](int index) const {
        return data[index];
    }

    void print() {
        cout << "[ ";
        for (int i = 0; i < size; i++) cout << data[i] << " ";
        cout << "]" << endl;
    }
};

int main() {
    IntArray arr(5);

    // Писане чрез operator[]
    arr[0] = 10;
    arr[1] = 20;
    arr[2] = 30;
    arr[3] = 40;
    arr[4] = 50;

    // Четене чрез operator[]
    cout << "arr[2] = " << arr[2] << endl;

    arr.print();

    // Модификация
    arr[2] *= 10;
    cout << "arr след arr[2] *= 10: "; arr.print();

    return 0;
}
```

**Изход:**
```
arr[2] = 30
[ 10 20 30 40 50 ]
arr след arr[2] *= 10: [ 10 20 300 40 50 ]
```

> 📌 `operator[]` връща **референция** (`int&`) — затова можем да пишем `arr[2] = 99`. Ако връщаше `int` (по стойност), щяхме да можем само да четем!

</details>

---

## 🔸 Задача 10 — `operator++` и `operator--` (prefix и postfix)

**Концепция:** Унарните оператори `++` и `--` имат **две форми**: prefix (`++x` — увеличи и върни новата стойност) и postfix (`x++` — върни старата стойност, после увеличи). C++ различава формите по **dummy параметъра `int`** при postfix.

Създай клас `Counter` с поле `value`.

Овърлоуднай:
- `operator++` (prefix) — увеличава с 1, връща нова стойност
- `operator++` (postfix) — увеличава с 1, връща стара стойност
- `operator--` (prefix и postfix) — намалява с 1

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
using namespace std;

class Counter {
public:
    int value;

    Counter(int v = 0) : value(v) {}

    // Prefix ++  →  ++c
    Counter& operator++() {
        ++value;
        return *this;
    }

    // Postfix ++  →  c++  (dummy параметър int)
    Counter operator++(int) {
        Counter old = *this;  // запазваме старата стойност
        ++value;
        return old;           // връщаме старата
    }

    // Prefix --
    Counter& operator--() {
        --value;
        return *this;
    }

    // Postfix --
    Counter operator--(int) {
        Counter old = *this;
        --value;
        return old;
    }

    void print() {
        cout << "Counter = " << value << endl;
    }
};

int main() {
    Counter c(5);

    cout << "Начална стойност: "; c.print();

    Counter a = ++c;
    cout << "След ++c: c = " << c.value << ", a = " << a.value << endl;

    Counter b = c++;
    cout << "След c++: c = " << c.value << ", b = " << b.value << endl;

    --c;
    cout << "След --c: "; c.print();

    c--;
    cout << "След c--: "; c.print();

    return 0;
}
```

**Изход:**
```
Начална стойност: Counter = 5
След ++c: c = 6, a = 6
След c++: c = 7, b = 6
След --c: Counter = 6
След c--: Counter = 5
```

> 📌 Prefix `++` е по-ефективен от postfix — postfix трябва да създаде **копие** на старата стойност преди да увеличи!

</details>

---

## 🔸 Задача 11 — `operator>`, `operator<`, `operator!=` за клас `Fraction`

**Концепция:** Можем да овърлоудваме **сравнителните оператори**, за да сравняваме обекти по логичен начин. Обичайно `!=` се дефинира чрез `==`, а `>` — чрез `<`.

Създай клас `Fraction` (дроб) с полета `numerator` и `denominator`.

Овърлоуднай:
- `operator==` — равенство
- `operator!=` — различие  
- `operator<` — по-малко
- `operator>` — по-голямо
- `operator<<` — отпечатване `"числител/знаменател"`

> 💡 За сравнение: a/b < c/d ↔ a\*d < c\*b

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
using namespace std;

class Fraction {
public:
    int numerator;
    int denominator;

    Fraction(int n = 0, int d = 1) {
        numerator   = n;
        denominator = d;
    }

    bool operator==(const Fraction& other) const {
        return numerator * other.denominator == other.numerator * denominator;
    }

    bool operator!=(const Fraction& other) const {
        return !(*this == other);
    }

    bool operator<(const Fraction& other) const {
        return numerator * other.denominator < other.numerator * denominator;
    }

    bool operator>(const Fraction& other) const {
        return other < *this;
    }

    Fraction operator+(const Fraction& other) const {
        return Fraction(
            numerator * other.denominator + other.numerator * denominator,
            denominator * other.denominator
        );
    }

    friend ostream& operator<<(ostream& os, const Fraction& f) {
        os << f.numerator << "/" << f.denominator;
        return os;
    }
};

int main() {
    Fraction a(1, 2);  // 1/2
    Fraction b(3, 4);  // 3/4
    Fraction c(2, 4);  // 2/4 = 1/2

    cout << a << " == " << c << " : " << (a == c ? "да" : "не") << endl;
    cout << a << " != " << b << " : " << (a != b ? "да" : "не") << endl;
    cout << a << " <  " << b << " : " << (a < b  ? "да" : "не") << endl;
    cout << b << " >  " << a << " : " << (b > a  ? "да" : "не") << endl;

    cout << a << " + " << b << " = " << (a + b) << endl;

    return 0;
}
```

**Изход:**
```
1/2 == 2/4 : да
1/2 != 3/4 : да
1/2 <  3/4 : да
3/4 >  1/2 : да
1/2 + 3/4 = 10/8
```

> 📌 `operator>` се дефинира чрез `operator<` (`other < *this`) — пишем логиката само веднъж!

</details>

---

## 🔸 Задача 12 — Пълен клас `Matrix2x2` с оператори

**Концепция:** Обединяваме голямата четворка и овърлоудване на оператори в един реалистичен клас — матрица 2×2.

Създай клас `Matrix2x2` с поле `double data[2][2]`.

Имплементирай:
- **Голямата четворка** (конструктор, копиращ конструктор, `operator=`, деструктор)
- `operator+` — събиране на матрици
- `operator*` — умножение на матрици (матрично!)
- `operator==` — проверка за равенство
- `operator<<` — отпечатване

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
using namespace std;

class Matrix2x2 {
public:
    double data[2][2];

    // Конструктор по подразбиране (нулева матрица)
    Matrix2x2() {
        for (int i = 0; i < 2; i++)
            for (int j = 0; j < 2; j++)
                data[i][j] = 0;
    }

    // Параметризиран конструктор
    Matrix2x2(double a, double b, double c, double d) {
        data[0][0] = a; data[0][1] = b;
        data[1][0] = c; data[1][1] = d;
    }

    // Копиращ конструктор
    Matrix2x2(const Matrix2x2& other) {
        for (int i = 0; i < 2; i++)
            for (int j = 0; j < 2; j++)
                data[i][j] = other.data[i][j];
    }

    // operator=
    Matrix2x2& operator=(const Matrix2x2& other) {
        if (this == &other) return *this;
        for (int i = 0; i < 2; i++)
            for (int j = 0; j < 2; j++)
                data[i][j] = other.data[i][j];
        return *this;
    }

    // Деструктор (данните са статични, няма динамична памет)
    ~Matrix2x2() {}

    // operator+
    Matrix2x2 operator+(const Matrix2x2& other) const {
        Matrix2x2 result;
        for (int i = 0; i < 2; i++)
            for (int j = 0; j < 2; j++)
                result.data[i][j] = data[i][j] + other.data[i][j];
        return result;
    }

    // operator* (матрично умножение)
    Matrix2x2 operator*(const Matrix2x2& other) const {
        Matrix2x2 result;
        for (int i = 0; i < 2; i++)
            for (int j = 0; j < 2; j++)
                for (int k = 0; k < 2; k++)
                    result.data[i][j] += data[i][k] * other.data[k][j];
        return result;
    }

    // operator==
    bool operator==(const Matrix2x2& other) const {
        for (int i = 0; i < 2; i++)
            for (int j = 0; j < 2; j++)
                if (data[i][j] != other.data[i][j]) return false;
        return true;
    }

    // operator<<
    friend ostream& operator<<(ostream& os, const Matrix2x2& m) {
        os << "[ " << m.data[0][0] << "  " << m.data[0][1] << " ]\n";
        os << "[ " << m.data[1][0] << "  " << m.data[1][1] << " ]";
        return os;
    }
};

int main() {
    Matrix2x2 A(1, 2, 3, 4);
    Matrix2x2 B(5, 6, 7, 8);

    cout << "A =\n" << A << "\n\n";
    cout << "B =\n" << B << "\n\n";

    cout << "A + B =\n" << (A + B) << "\n\n";
    cout << "A * B =\n" << (A * B) << "\n\n";

    Matrix2x2 C = A;
    cout << "A == C: " << (A == C ? "да" : "не") << endl;
    cout << "A == B: " << (A == B ? "да" : "не") << endl;

    return 0;
}
```

**Изход:**
```
A =
[ 1  2 ]
[ 3  4 ]

B =
[ 5  6 ]
[ 7  8 ]

A + B =
[ 6  8 ]
[ 10  12 ]

A * B =
[ 19  22 ]
[ 43  50 ]

A == C: да
A == B: не
```

> 📌 Матричното умножение **не е** компонентно! `(A*B)[i][j] = Σ A[i][k] * B[k][j]` — именно затова овърлоудването е полезно: скриваме тази сложност зад прост `*`.

</details>

---

## 📝 Обобщение на ключови понятия

| Понятие | Синтаксис | Обяснение |
|---------|-----------|-----------|
| Конструктор по подразбиране | `Foo() { ... }` | Извиква се при `Foo obj;` |
| Параметризиран конструктор | `Foo(int x) { ... }` | Извиква се при `Foo obj(5);` |
| Деструктор | `~Foo() { ... }` | Извиква се автоматично при унищожаване |
| Копиращ конструктор | `Foo(const Foo& other)` | Дълбоко копие при `Foo b = a;` |
| `operator=` | `Foo& operator=(const Foo&)` | Присвояване при `b = a;` (за съществуващи обекти) |
| Самоприсвояване | `if (this == &other)` | Задължителна проверка в `operator=`! |
| `operator+` | `Foo operator+(const Foo&)` | `a + b` → нов обект |
| `operator==` | `bool operator==(const Foo&)` | Сравнение на два обекта |
| `operator<<` (friend) | `friend ostream& operator<<(ostream&, const Foo&)` | `cout << obj` |
| `operator[]` | `T& operator[](int)` | `arr[i]` — четене и писане |
| Prefix `++` | `Foo& operator++()` | `++x` — върни нова стойност |
| Postfix `++` | `Foo operator++(int)` | `x++` — върни стара стойност |
