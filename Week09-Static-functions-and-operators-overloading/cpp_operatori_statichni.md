# C++ ООП — Предефиниране на оператори, Статични данни и функции

---

## 🔹 Задача 1 — Основни концепции: предефиниране на унарен оператор

**Концепция:** Предефинирането на оператори (`operator overloading`) позволява да дадем специално значение на стандартните оператори (`+`, `-`, `*`, `==`, `++` и др.) за потребителски типове. Дефинира се като метод или приятелска функция с ключовата дума `operator` последвана от символа на оператора. Унарните оператори работят с един операнд.

Създай клас `Counter` с поле `value`. Предефинирай:
- унарния `operator++` (префиксен) — увеличава `value` с 1 и връща обекта
- унарния `operator--` (префиксен) — намалява `value` с 1 и връща обекта
- метод `print()` — отпечатва стойността

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
using namespace std;

class Counter {
    int value;
public:
    Counter(int v = 0) : value(v) {}

    // Префиксен ++: operator++ без аргументи
    Counter& operator++() {
        ++value;
        return *this;   // връщаме самия обект
    }

    // Префиксен --
    Counter& operator--() {
        --value;
        return *this;
    }

    void print() const {
        cout << "Counter: " << value << endl;
    }
};

int main() {
    Counter c(5);
    c.print();

    ++c;
    ++c;
    c.print();  // 7

    --c;
    c.print();  // 6

    return 0;
}
```

**Изход:**
```
Counter: 5
Counter: 7
Counter: 6
```

> 📌 Префиксният `operator++` се декларира без параметри и връща референция към `*this`. Постфиксният (`c++`) се декларира с фиктивен параметър `int` — `Counter operator++(int)` — и връща **копие** (стойността преди увеличаването).

</details>

---

## 🔹 Задача 2 — Бинарен оператор като метод на клас

**Концепция:** Бинарните оператори работят с два операнда. Когато се дефинират като **метод на клас**, левият операнд е текущият обект (`*this`), а десният се подава като параметър. Синтаксисът е: `ReturnType operator@(const Type& rhs) const`.

Създай клас `Vector2D` с полета `x` и `y`. Предефинирай:
- `operator+` — събира два вектора и връща нов
- `operator-` — изважда два вектора и връща нов
- `operator*` — умножава вектора по скалар `double` и връща нов
- метод `print()` — отпечатва вектора в стил `(x, y)`

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
using namespace std;

class Vector2D {
public:
    double x, y;

    Vector2D(double x = 0, double y = 0) : x(x), y(y) {}

    // Бинарен +: лявото е *this, дясното е rhs
    Vector2D operator+(const Vector2D& rhs) const {
        return Vector2D(x + rhs.x, y + rhs.y);
    }

    // Бинарен -
    Vector2D operator-(const Vector2D& rhs) const {
        return Vector2D(x - rhs.x, y - rhs.y);
    }

    // Умножение по скалар (вектор * число)
    Vector2D operator*(double scalar) const {
        return Vector2D(x * scalar, y * scalar);
    }

    void print() const {
        cout << "(" << x << ", " << y << ")" << endl;
    }
};

int main() {
    Vector2D v1(3.0, 4.0);
    Vector2D v2(1.0, 2.0);

    Vector2D sum  = v1 + v2;
    Vector2D diff = v1 - v2;
    Vector2D scaled = v1 * 2.0;

    cout << "v1 + v2 = "; sum.print();
    cout << "v1 - v2 = "; diff.print();
    cout << "v1 * 2  = "; scaled.print();

    return 0;
}
```

**Изход:**
```
v1 + v2 = (4, 6)
v1 - v2 = (2, 2)
v1 * 2  = (6, 8)
```

> 📌 Методите са декларирани с `const` накрая, защото не модифицират обекта. Връщат **нов обект** (по стойност), не `*this`. Ако искаме `2.0 * v1` (скалар вляво), трябва приятелска функция — вижте Задача 4.

</details>

---

## 🔹 Задача 3 — Оператор за сравнение и присвояване

**Концепция:** Операторите за сравнение (`==`, `!=`, `<`, `>`) обикновено връщат `bool`. Операторът за присвояване `operator=` трябва да върне референция към `*this`, за да поддържа верижно присвояване (`a = b = c`). Винаги проверявайте за самоприсвояване (`if (this == &rhs) return *this`).

Създай клас `Fraction` (дроб) с полета `numerator` и `denominator`. Предефинирай:
- `operator==` — две дроби са равни ако `a/b == c/d` (т.е. `a*d == b*c`)
- `operator<` — сравнява дроби
- `operator=` — присвояване с проверка за самоприсвояване
- метод `print()` — отпечатва в стил `a/b`

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
using namespace std;

class Fraction {
public:
    int numerator, denominator;

    Fraction(int n = 0, int d = 1) : numerator(n), denominator(d) {}

    // Оператор ==
    bool operator==(const Fraction& rhs) const {
        return numerator * rhs.denominator == denominator * rhs.numerator;
    }

    // Оператор !=  (дефиниран чрез ==)
    bool operator!=(const Fraction& rhs) const {
        return !(*this == rhs);
    }

    // Оператор <
    bool operator<(const Fraction& rhs) const {
        return numerator * rhs.denominator < denominator * rhs.numerator;
    }

    // Оператор = (присвояване)
    Fraction& operator=(const Fraction& rhs) {
        if (this == &rhs) return *this;  // защита от самоприсвояване
        numerator   = rhs.numerator;
        denominator = rhs.denominator;
        return *this;   // задължително!
    }

    void print() const {
        cout << numerator << "/" << denominator << endl;
    }
};

int main() {
    Fraction a(1, 2);   // 1/2
    Fraction b(2, 4);   // 2/4 == 1/2
    Fraction c(3, 4);   // 3/4

    cout << "a = "; a.print();
    cout << "b = "; b.print();
    cout << "c = "; c.print();

    cout << "a == b: " << (a == b ? "да" : "не") << endl;  // да
    cout << "a == c: " << (a == c ? "да" : "не") << endl;  // не
    cout << "a <  c: " << (a < c  ? "да" : "не") << endl;  // да

    Fraction d;
    d = a;              // operator=
    cout << "d = "; d.print();

    return 0;
}
```

**Изход:**
```
a = 1/2
b = 2/4
c = 3/4
a == b: да
a == c: не
a <  c: да
d = 1/2
```

> 📌 Операторът `=` трябва да върне `Fraction&` (референция), за да работи верижното присвояване `d = c = a`. Без проверката `this == &rhs` самоприсвояването `a = a` би изтрило данните преди да ги копира.

</details>

---

## 🔹 Задача 4 — Приятелски функции (`friend`)

**Концепция:** `friend` функцията не е метод на класа, но има достъп до `private` и `protected` членовете му. Декларира се вътре в класа с ключовата дума `friend`, но се дефинира извън него. Типично се ползва за `operator<<`, `operator>>` и бинарни оператори, при които левият операнд **не е** обект от нашия клас.

Използвай класа `Vector2D` от Задача 2. Добави:
- `friend operator<<` — отпечатва вектора в стил `(x, y)` чрез `cout`
- `friend operator>>` — чете вектора от `cin`
- `friend operator*` — умножение **скалар × вектор** (скаларът е вляво)

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
using namespace std;

class Vector2D {
    double x, y;
public:
    Vector2D(double x = 0, double y = 0) : x(x), y(y) {}

    Vector2D operator+(const Vector2D& rhs) const {
        return Vector2D(x + rhs.x, y + rhs.y);
    }

    // Вектор * скалар (вектор вляво) — може като метод
    Vector2D operator*(double scalar) const {
        return Vector2D(x * scalar, y * scalar);
    }

    // ---- Приятелски функции ----

    // operator<< : ostream е вляво, не можем да го направим метод
    friend ostream& operator<<(ostream& os, const Vector2D& v) {
        os << "(" << v.x << ", " << v.y << ")";
        return os;  // задължително — за верижно: cout << v1 << v2
    }

    // operator>> : istream е вляво
    friend istream& operator>>(istream& is, Vector2D& v) {
        is >> v.x >> v.y;
        return is;
    }

    // скалар * вектор: double е вляво — не може като метод на Vector2D
    friend Vector2D operator*(double scalar, const Vector2D& v) {
        return Vector2D(v.x * scalar, v.y * scalar);
    }
};

int main() {
    Vector2D v1(3.0, 4.0);
    Vector2D v2(1.0, 2.0);

    cout << "v1 = " << v1 << endl;
    cout << "v2 = " << v2 << endl;
    cout << "v1 + v2 = " << (v1 + v2) << endl;
    cout << "v1 * 2.0 = " << (v1 * 2.0) << endl;  // метод
    cout << "3.0 * v2 = " << (3.0 * v2) << endl;  // приятелска функция

    // Верижно извеждане
    cout << "v1 и v2: " << v1 << " и " << v2 << endl;

    return 0;
}
```

**Изход:**
```
v1 = (3, 4)
v2 = (1, 2)
v1 + v2 = (4, 6)
v1 * 2.0 = (6, 8)
3.0 * v2 = (3, 6)
v1 и v2: (3, 4) и (1, 2)
```

> 📌 `operator<<` трябва да е `friend`, защото левият операнд е `ostream&`, не нашият клас. Функцията **не е член** — дефинира се без `Vector2D::`. Връщането на `os` е задължително за верижното `cout << v1 << v2`.

</details>

---

## 🔹 Задача 5 — Статични данни (`static` член-данна)

**Концепция:** Статичните член-данни принадлежат на **класа**, не на отделен обект. Всички обекти споделят едно и също статично поле. Декларира се в класа с `static`, но трябва да се **дефинира** (инициализира) извън класа. Достъпва се като `ClassName::field` или чрез обект.

Създай клас `Student` с:
- `static int count` — брои колко обекта са създадени
- конструктор, който увеличава `count`
- деструктор, който намалява `count`
- `static int getCount()` — връща текущия брой

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
#include <cstring>
using namespace std;

class Student {
    char name[50];
    static int count;   // декларация — само едно копие за целия клас

public:
    Student(const char* n) {
        strcpy(name, n);
        count++;        // всеки нов обект увеличава брояча
        cout << name << " е приет. Общо студенти: " << count << endl;
    }

    ~Student() {
        count--;        // унищоженият обект намалява брояча
        cout << name << " е изписан. Останали: " << count << endl;
    }

    static int getCount() {     // статичен метод — достъп без обект
        return count;
    }
};

// Дефиниция и инициализация извън класа (задължително!)
int Student::count = 0;

int main() {
    cout << "Начало. Студенти: " << Student::getCount() << endl;

    Student s1("Иван");
    Student s2("Мария");

    {
        Student s3("Петър");    // влиза в нов обхват
        cout << "В блока: " << Student::getCount() << endl;
    }   // s3 се унищожава тук

    cout << "След блока: " << Student::getCount() << endl;

    return 0;
}   // s1 и s2 се унищожават
```

**Изход:**
```
Начало. Студенти: 0
Иван е приет. Общо студенти: 1
Мария е приета. Общо студенти: 2
Петър е приет. Общо студенти: 3
В блока: 3
Петър е изписан. Останали: 2
След блока: 2
Мария е изписана. Останали: 1
Иван е изписан. Останали: 0
```

> 📌 Статичното поле `count` съществува **преди** да е създаден какъвто и да е обект — затова достъпът `Student::getCount()` работи без обект. Дефиницията `int Student::count = 0;` **трябва** да е извън класа — иначе компилаторът не заделя памет за полето.

</details>

---

## 🔹 Задача 6 — Статични функции и константи

**Концепция:** Статичен метод се извиква без обект (`ClassName::method()`). Той **няма** достъп до `this` и може да работи само с `static` полета или с аргументи. Полезен е за фабрични методи, помощни изчисления и операции на ниво клас.

Създай клас `MathHelper` само с:
- `static double PI` — константата π, инициализирана извън класа
- `static double circleArea(double r)` — лице на кръг
- `static double circlePerimeter(double r)` — периметър на кръг
- `static double degreesToRadians(double deg)` — конвертира градуси в радиани

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
using namespace std;

class MathHelper {
public:
    static double PI;   // статична константа

    static double circleArea(double r) {
        return PI * r * r;
    }

    static double circlePerimeter(double r) {
        return 2 * PI * r;
    }

    static double degreesToRadians(double deg) {
        return deg * PI / 180.0;
    }
    // Няма нужда от конструктор — класът не се инстанцира
};

// Дефиниция извън класа
double MathHelper::PI = 3.14159265358979;

int main() {
    double r = 5.0;

    // Извикване без обект — директно чрез класа
    cout << "PI = "            << MathHelper::PI                      << endl;
    cout << "Лице (r=5): "     << MathHelper::circleArea(r)           << endl;
    cout << "Периметър (r=5): " << MathHelper::circlePerimeter(r)     << endl;
    cout << "90° в радиани: "  << MathHelper::degreesToRadians(90.0)  << endl;

    return 0;
}
```

**Изход:**
```
PI = 3.14159
Лице (r=5): 78.5398
Периметър (r=5): 31.4159
90° в радиани: 1.5708
```

> 📌 Статичният метод **не може** да достъпва нестатични полета или да извиква нестатични методи, защото няма `this` указател. Когато класът служи само като "пространство от имена" за помощни функции, всичко в него е `static`.

</details>

---

## 🔸 Задача 7 — Комбинация: оператор `+` и `+=`, брояч на обекти

**Концепция:** `operator+=` модифицира текущия обект и връща `*this`. Удобно е да дефинираме `operator+` чрез `operator+=`, за да избегнем повтарящ се код. Комбинираме с `static` брояч, за да следим колко обекта съществуват.

Създай клас `BigInt` (опростен), представящ цяло число като `int`. Предефинирай:
- `operator+=` — добавя към текущия обект
- `operator+` — дефиниран чрез `operator+=`
- `friend operator<<` — отпечатва стойността
- `static int objectCount` — брои живите обекти (конструктор/деструктор)

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
using namespace std;

class BigInt {
    int value;
    static int objectCount;

public:
    BigInt(int v = 0) : value(v) {
        objectCount++;
    }

    // Копиращ конструктор — трябва и той да брои
    BigInt(const BigInt& other) : value(other.value) {
        objectCount++;
    }

    ~BigInt() {
        objectCount--;
    }

    // operator+= : модифицира *this, връща референция
    BigInt& operator+=(const BigInt& rhs) {
        value += rhs.value;
        return *this;
    }

    // operator+ : дефиниран чрез +=
    // Правим копие на lhs, добавяме rhs и го връщаме
    BigInt operator+(const BigInt& rhs) const {
        BigInt result(*this);   // копиращ конструктор
        result += rhs;
        return result;
    }

    friend ostream& operator<<(ostream& os, const BigInt& b) {
        os << b.value;
        return os;
    }

    static int getCount() { return objectCount; }
};

int BigInt::objectCount = 0;

int main() {
    cout << "Обекти: " << BigInt::getCount() << endl;  // 0

    BigInt a(10), b(20), c(30);
    cout << "Обекти: " << BigInt::getCount() << endl;  // 3

    cout << "a = "     << a << endl;
    cout << "b = "     << b << endl;

    a += b;
    cout << "a += b -> " << a << endl;  // 30

    BigInt d = a + c;
    cout << "a + c = " << d << endl;    // 60

    cout << "Обекти: " << BigInt::getCount() << endl;  // 4

    return 0;
}
```

**Изход:**
```
Обекти: 0
Обекти: 3
a = 10
b = 20
a += b -> 30
a + c = 60
Обекти: 4
```

> 📌 Дефинирането на `operator+` чрез `operator+=` е стандарт — пишем логиката само веднъж. Копиращият конструктор **трябва** да брои обекти, защото `BigInt result(*this)` вътре в `operator+` също създава нов обект.

</details>

---

## 🔸 Задача 8 — Приятелски клас

**Концепция:** Освен приятелски функции, в C++ можем да декларираме цял **приятелски клас** — тогава всички методи на приятелския клас имат достъп до `private` членовете на оригиналния. Декларира се с `friend class ИмеНаКлас;`.

Създай клас `BankAccount` с `private` поле `balance`. Създай клас `Auditor` (одитор), деклариран като `friend` на `BankAccount`, с метод `printBalance()` и `addBonus()`, които директно четат и пишат в `balance`.

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
using namespace std;

class BankAccount {
    double balance;     // private!
    char owner[50];

public:
    BankAccount(const char* name, double initial);

    void deposit(double amount) {
        balance += amount;
    }

    friend class Auditor;   // Auditor вижда всичко в BankAccount
};

// Дефиниция на конструктора извън класа
#include <cstring>
BankAccount::BankAccount(const char* name, double initial) {
    strcpy(owner, name);
    balance = initial;
}

class Auditor {
public:
    // Директен достъп до private полета на BankAccount
    void printBalance(const BankAccount& acc) {
        cout << "Одит: " << acc.owner
             << " -> баланс: " << acc.balance << " лв." << endl;
    }

    void addBonus(BankAccount& acc, double bonus) {
        acc.balance += bonus;   // директен достъп!
        cout << "Бонус " << bonus << " лв. добавен на " << acc.owner << endl;
    }
};

int main() {
    BankAccount a1("Иван", 1000.0);
    BankAccount a2("Мария", 2500.0);

    Auditor audit;
    audit.printBalance(a1);
    audit.printBalance(a2);

    audit.addBonus(a1, 150.0);
    audit.printBalance(a1);

    return 0;
}
```

**Изход:**
```
Одит: Иван -> баланс: 1000 лв.
Одит: Мария -> баланс: 2500 лв.
Бонус 150 лв. добавен на Иван
Одит: Иван -> баланс: 1150 лв.
```

> 📌 Приятелството **не е взаимно** — `BankAccount` не вижда `private` членовете на `Auditor`. Приятелството и **не се наследява** — производен клас на `Auditor` не автоматично е приятел на `BankAccount`. Ползвайте `friend` внимателно — то нарушава капсулацията.

</details>

---

## 🔸 Задача 9 — Пълна задача: клас `Matrix2x2`

**Концепция:** Обединяваме всички теми: бинарни оператори като методи, `friend operator<<`, `operator==`, и статичен метод (фабричен). Класът представя 2×2 матрица.

Създай клас `Matrix2x2` с поле `double a[2][2]`. Предефинирай:
- `operator+` и `operator*` (матрично умножение)
- `operator==`
- `friend operator<<` — отпечатва матрицата красиво
- `static Matrix2x2 identity()` — връща единичната матрица

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
using namespace std;

class Matrix2x2 {
    double a[2][2];

public:
    Matrix2x2() {
        for (int i = 0; i < 2; i++)
            for (int j = 0; j < 2; j++)
                a[i][j] = 0;
    }

    Matrix2x2(double a00, double a01, double a10, double a11) {
        a[0][0] = a00; a[0][1] = a01;
        a[1][0] = a10; a[1][1] = a11;
    }

    // Събиране
    Matrix2x2 operator+(const Matrix2x2& rhs) const {
        return Matrix2x2(
            a[0][0] + rhs.a[0][0], a[0][1] + rhs.a[0][1],
            a[1][0] + rhs.a[1][0], a[1][1] + rhs.a[1][1]
        );
    }

    // Матрично умножение
    Matrix2x2 operator*(const Matrix2x2& rhs) const {
        return Matrix2x2(
            a[0][0]*rhs.a[0][0] + a[0][1]*rhs.a[1][0],
            a[0][0]*rhs.a[0][1] + a[0][1]*rhs.a[1][1],
            a[1][0]*rhs.a[0][0] + a[1][1]*rhs.a[1][0],
            a[1][0]*rhs.a[0][1] + a[1][1]*rhs.a[1][1]
        );
    }

    // Равенство
    bool operator==(const Matrix2x2& rhs) const {
        for (int i = 0; i < 2; i++)
            for (int j = 0; j < 2; j++)
                if (a[i][j] != rhs.a[i][j]) return false;
        return true;
    }

    // Статичен фабричен метод — единична матрица
    static Matrix2x2 identity() {
        return Matrix2x2(1, 0, 0, 1);
    }

    // Приятелска функция за изход
    friend ostream& operator<<(ostream& os, const Matrix2x2& m) {
        os << "[" << m.a[0][0] << ", " << m.a[0][1] << "]\n"
           << "[" << m.a[1][0] << ", " << m.a[1][1] << "]";
        return os;
    }
};

int main() {
    Matrix2x2 A(1, 2, 3, 4);
    Matrix2x2 B(5, 6, 7, 8);
    Matrix2x2 I = Matrix2x2::identity();   // статичен метод

    cout << "A =\n" << A << "\n\n";
    cout << "B =\n" << B << "\n\n";
    cout << "I (единична) =\n" << I << "\n\n";

    cout << "A + B =\n" << (A + B) << "\n\n";
    cout << "A * B =\n" << (A * B) << "\n\n";
    cout << "A * I =\n" << (A * I) << "\n\n";

    cout << "A == A: " << (A == A ? "да" : "не") << endl;
    cout << "A == B: " << (A == B ? "да" : "не") << endl;

    return 0;
}
```

**Изход:**
```
A =
[1, 2]
[3, 4]

B =
[5, 6]
[7, 8]

I (единична) =
[1, 0]
[0, 1]

A + B =
[6, 8]
[10, 12]

A * B =
[19, 22]
[43, 50]

A * I =
[1, 2]
[3, 4]

A == A: да
A == B: не
```

> 📌 `A * I` дава `A` — единичната матрица е неутрален елемент при умножение. Статичният метод `identity()` се извиква без обект: `Matrix2x2::identity()`. `friend operator<<` има директен достъп до `private` масива `a`.

</details>

---

## 📝 Обобщение на ключови понятия

| Понятие | Синтаксис | Обяснение |
|---------|-----------|-----------|
| Предефиниране на оператор | `Тип operator+(const Тип& rhs) const` | Дефинира поведение на `+` за потребителски тип |
| Унарен оператор (префикс) | `Тип& operator++()` | Без параметри, връща `*this` |
| Унарен оператор (постфикс) | `Тип operator++(int)` | Фиктивен `int` параметър, връща копие |
| Бинарен оператор (метод) | `Тип operator+(const Тип& rhs) const` | Левият операнд е `*this` |
| `operator=` | `Тип& operator=(const Тип& rhs)` | Връща `*this`, проверка за самоприсвояване |
| `operator+=` | `Тип& operator+=(const Тип& rhs)` | Модифицира `*this`, връща `*this` |
| Приятелска функция | `friend ReturnType operator<<(...)` | Не е метод, но вижда `private` членовете |
| `operator<<` | `friend ostream& operator<<(ostream& os, ...)` | Задължително връща `os` за верижно изход |
| `operator>>` | `friend istream& operator>>(istream& is, ...)` | Задължително връща `is` |
| Статична член-данна | `static int count;` + `int ClassName::count = 0;` | Споделена от всички обекти, дефиниция извън класа |
| Статична функция | `static int getCount()` | Извиква се без обект: `ClassName::getCount()` |
| Приятелски клас | `friend class Auditor;` | Всички методи на `Auditor` виждат `private` членовете |
