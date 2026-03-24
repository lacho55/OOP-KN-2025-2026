# 📚 C++ ООП — Задачи за Класове

---

## 🔹 Задача 1 — Клас `Куче`

Създай клас `Dog` с полета:
- `name` (char масив с размер 50) — името на кучето
- `age` (цяло число) — възрастта в години

Добави метод `bark()`, който отпечатва:  
`"[Име] казва: Бау!"`

Създай обект от класа и извикай метода.

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
#include <cstring>
using namespace std;

class Dog {
public:
    char name[50];
    int age;

    void bark() {
        cout << name << " казва: Бау!" << endl;
    }
};

int main() {
    Dog myDog;
    strcpy(myDog.name, "Рекс");
    myDog.age = 3;
    myDog.bark();

    return 0;
}
```

**Изход:**
```
Рекс казва: Бау!
```

</details>

---

## 🔹 Задача 2 — Клас `Правоъгълник`

Създай клас `Rectangle` с полета:
- `width` (дробно число) — ширина
- `height` (дробно число) — височина

Добави методи:
- `area()` — връща лицето
- `perimeter()` — връща периметъра

Отпечатай лицето и периметъра на правоъгълник с ширина 5.0 и височина 3.0.

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
using namespace std;

class Rectangle {
public:
    double width;
    double height;

    double area() {
        return width * height;
    }

    double perimeter() {
        return 2 * (width + height);
    }
};

int main() {
    Rectangle rect;
    rect.width = 5.0;
    rect.height = 3.0;

    cout << "Лице: " << rect.area() << endl;
    cout << "Периметър: " << rect.perimeter() << endl;

    return 0;
}
```

**Изход:**
```
Лице: 15
Периметър: 16
```

</details>

---

## 🔹 Задача 3 — Клас `Студент`

Създай клас `Student` с полета:
- `name` (char масив с размер 50) — ime
- `grade` (дробно число) — оценка

Добави метод `isPassing()`, който връща `true` ако оценката е ≥ 3.0, иначе `false`.

Отпечатай дали студентът е успял.

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
#include <cstring>
using namespace std;

class Student {
public:
    char name[50];
    double grade;

    bool isPassing() {
        return grade >= 3.0;
    }
};

int main() {
    Student s;
    strcpy(s.name, "Иван");
    s.grade = 4.5;

    if (s.isPassing()) {
        cout << s.name << " е успял." << endl;
    } else {
        cout << s.name << " не е успял." << endl;
    }

    return 0;
}
```

**Изход:**
```
Иван е успял.
```

</details>

---

## 🔹 Задача 4 — Клас `Банкова сметка`

Създай клас `BankAccount` с поле:
- `balance` — баланс (дробно число), инициализирано на 0

Добави методи:
- `deposit(amount)` — добавя сума
- `withdraw(amount)` — тегли сума (само ако има достатъчно пари)
- `printBalance()` — отпечатва текущия баланс

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
using namespace std;

class BankAccount {
public:
    double balance;

    BankAccount() {
        balance = 0;
    }

    void deposit(double amount) {
        balance += amount;
        cout << "Внесени: " << amount << " лв." << endl;
    }

    void withdraw(double amount) {
        if (amount <= balance) {
            balance -= amount;
            cout << "Изтеглени: " << amount << " лв." << endl;
        } else {
            cout << "Недостатъчно средства!" << endl;
        }
    }

    void printBalance() {
        cout << "Баланс: " << balance << " лв." << endl;
    }
};

int main() {
    BankAccount acc;
    acc.deposit(1000);
    acc.withdraw(300);
    acc.withdraw(800);
    acc.printBalance();

    return 0;
}
```

**Изход:**
```
Внесени: 1000 лв.
Изтеглени: 300 лв.
Недостатъчно средства!
Баланс: 700 лв.
```

</details>

---

## 🔹 Задача 5 — Клас `Температура`

Създай клас `Temperature` с поле:
- `celsius` — температура в Целзий

Добави методи:
- `toFahrenheit()` — връща температурата по Фаренхайт *(F = C × 9/5 + 32)*
- `toKelvin()` — връща температурата в Келвин *(K = C + 273.15)*

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
using namespace std;

class Temperature {
public:
    double celsius;

    double toFahrenheit() {
        return celsius * 9.0 / 5.0 + 32;
    }

    double toKelvin() {
        return celsius + 273.15;
    }
};

int main() {
    Temperature temp;
    temp.celsius = 100;

    cout << temp.celsius << " C = " << temp.toFahrenheit() << " F" << endl;
    cout << temp.celsius << " C = " << temp.toKelvin()     << " K" << endl;

    return 0;
}
```

**Изход:**
```
100 C = 212 F
100 C = 373.15 K
```

</details>

---

## 🔹 Задача 6 — Клас `Кръг`

Създай клас `Circle` с поле `radius`.

Добави методи:
- `area()` — лице (π × r²)
- `circumference()` — периметър (2 × π × r)

Използвай `3.14159` за π.

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
using namespace std;

class Circle {
public:
    double radius;

    double area() {
        return 3.14159 * radius * radius;
    }

    double circumference() {
        return 2 * 3.14159 * radius;
    }
};

int main() {
    Circle c;
    c.radius = 7.0;

    cout << "Лице: "     << c.area()          << endl;
    cout << "Периметър: " << c.circumference() << endl;

    return 0;
}
```

**Изход:**
```
Лице: 153.938
Периметър: 43.9823
```

</details>

---

## 🔹 Задача 7 — Клас `Часовник`

Създай клас `Clock` с полета `hours`, `minutes`, `seconds`.

Добави метод `display()`, който отпечатва времето в формат `ЧЧ:ММ:СС`.

> 💡 Използвай `setw(2)` и `setfill('0')` от `<iomanip>`.

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
#include <iomanip>
using namespace std;

class Clock {
public:
    int hours;
    int minutes;
    int seconds;

    void display() {
        cout << setfill('0')
             << setw(2) << hours   << ":"
             << setw(2) << minutes << ":"
             << setw(2) << seconds << endl;
    }
};

int main() {
    Clock c;
    c.hours = 9; c.minutes = 5; c.seconds = 7;
    c.display();

    return 0;
}
```

**Изход:**
```
09:05:07
```

</details>

---

## 🔹 Задача 8 — Клас `Точка`

Създай клас `Point` с полета `x` и `y`.

Добави метод `distanceTo(Point other)`, който изчислява разстоянието до друга точка.  
*(Формула: √((x₂−x₁)² + (y₂−y₁)²))*

> 💡 Използвай `sqrt()` от `<cmath>`.

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
#include <cmath>
using namespace std;

class Point {
public:
    double x;
    double y;

    double distanceTo(Point other) {
        double dx = other.x - x;
        double dy = other.y - y;
        return sqrt(dx * dx + dy * dy);
    }
};

int main() {
    Point p1, p2;
    p1.x = 0; p1.y = 0;
    p2.x = 3; p2.y = 4;

    cout << "Разстояние: " << p1.distanceTo(p2) << endl;

    return 0;
}
```

**Изход:**
```
Разстояние: 5
```

</details>

---

## 🔹 Задача 9 — Клас `Книга`

Създай клас `Book` с полета:
- `title` (char[100]) — заглавие
- `author` (char[100]) — автор
- `pages` — брой страници

Добави метод `describe()`. Създай масив от 3 книги и отпечатай всички.

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
#include <cstring>
using namespace std;

class Book {
public:
    char title[100];
    char author[100];
    int pages;

    void describe() {
        cout << "'" << title << "' от " << author
             << " - " << pages << " стр." << endl;
    }
};

int main() {
    Book library[3];

    strcpy(library[0].title,  "Под игото");
    strcpy(library[0].author, "Иван Вазов");
    library[0].pages = 380;

    strcpy(library[1].title,  "Дядо Горио");
    strcpy(library[1].author, "Оноре дьо Балзак");
    library[1].pages = 312;

    strcpy(library[2].title,  "Малкият принц");
    strcpy(library[2].author, "Антоан дьо Сент-Екзюпери");
    library[2].pages = 96;

    for (int i = 0; i < 3; i++) {
        library[i].describe();
    }

    return 0;
}
```

**Изход:**
```
'Под игото' от Иван Вазов - 380 стр.
'Дядо Горио' от Оноре дьо Балзак - 312 стр.
'Малкият принц' от Антоан дьо Сент-Екзюпери - 96 стр.
```

</details>

---

## 🔹 Задача 10 — Клас `Калкулатор`

Създай клас `Calculator` с поле `result` (инициализирано на 0).

Добави методи `add(n)`, `subtract(n)`, `multiply(n)`, `reset()`, `getResult()`.

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
using namespace std;

class Calculator {
public:
    double result;

    Calculator() { result = 0; }

    void add(double n)      { result += n; }
    void subtract(double n) { result -= n; }
    void multiply(double n) { result *= n; }
    void reset()            { result = 0;  }

    double getResult() { return result; }
};

int main() {
    Calculator calc;
    calc.add(10);
    calc.multiply(3);
    calc.subtract(5);

    cout << "Резултат: "     << calc.getResult() << endl;
    calc.reset();
    cout << "След нулиране: " << calc.getResult() << endl;

    return 0;
}
```

**Изход:**
```
Резултат: 25
След нулиране: 0
```

</details>

---

## 🔹 Задача 11 — Матрица 2×2

Създай клас `Matrix2x2` с поле `data[2][2]`.

Добави `set(row, col, value)`, `print()` и `trace()` (сума по главния диагонал).

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
using namespace std;

class Matrix2x2 {
public:
    double data[2][2];

    void set(int row, int col, double value) {
        data[row][col] = value;
    }

    void print() {
        for (int i = 0; i < 2; i++) {
            for (int j = 0; j < 2; j++) {
                cout << data[i][j] << "\t";
            }
            cout << endl;
        }
    }

    double trace() {
        return data[0][0] + data[1][1];
    }
};

int main() {
    Matrix2x2 m;
    m.set(0,0,1); m.set(0,1,2);
    m.set(1,0,3); m.set(1,1,4);

    m.print();
    cout << "Следа: " << m.trace() << endl;

    return 0;
}
```

**Изход:**
```
1    2
3    4
Следа: 5
```

</details>

---

## 🔹 Задача 12 — Светофар

Създай клас `TrafficLight` с поле `state` (цяло число: 0=червено, 1=жълто, 2=зелено).

Добави `setRed()`, `setYellow()`, `setGreen()` и `display()`.

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
using namespace std;

class TrafficLight {
public:
    int state;

    TrafficLight() { state = 0; }

    void setRed()    { state = 0; }
    void setYellow() { state = 1; }
    void setGreen()  { state = 2; }

    void display() {
        const char* names[] = { "Червено", "Жълто", "Зелено" };
        cout << "Светофар: " << names[state] << endl;
    }
};

int main() {
    TrafficLight light;
    light.display();
    light.setYellow(); light.display();
    light.setGreen();  light.display();
    light.setYellow(); light.display();
    light.setRed();    light.display();

    return 0;
}
```

**Изход:**
```
Светофар: Червено
Светофар: Жълто
Светофар: Зелено
Светофар: Жълто
Светофар: Червено
```

</details>

---

---

## 🔸 Задача 13 — Конструктор и деструктор

**Концепция:** Конструкторът се извиква автоматично при **създаване** на обект, деструкторът — при **унищожаването** му (края на scope-а).

Създай клас `Sensor` с поле `id`.

- Конструктор с параметър `id`, отпечатва `"Сензор [id] е включен."`
- Деструктор, отпечатва `"Сензор [id] е изключен."`

Създай два обекта и наблюдавай реда на извикване.

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
using namespace std;

class Sensor {
public:
    int id;

    // Конструктор
    Sensor(int sensorId) {
        id = sensorId;
        cout << "Сензор " << id << " е включен." << endl;
    }

    // Деструктор
    ~Sensor() {
        cout << "Сензор " << id << " е изключен." << endl;
    }
};

int main() {
    Sensor s1(1);
    Sensor s2(2);

    cout << "--- Програмата работи ---" << endl;

    return 0; // деструкторите се извикват автоматично тук
}
```

**Изход:**
```
Сензор 1 е включен.
Сензор 2 е включен.
--- Програмата работи ---
Сензор 2 е изключен.
Сензор 1 е изключен.
```

> 📌 Деструкторите се извикват в **обратен ред** — последно създаденият се унищожава пръв!

</details>

---

## 🔸 Задача 14 — Динамична памет

**Концепция:** `new` заделя памет в heap-а. Тя **не се освобождава автоматично** — трябва да извикаме `delete[]` сами, обикновено в деструктора.

Създай клас `DynamicArray` с полета `int* data` и `int size`.

- Конструктор: приема размер, заделя масив с `new`, запълва с нули
- Деструктор: освобождава паметта с `delete[]`
- `set(index, value)` — задава стойност по индекс
- `print()` — отпечатва елементите

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
using namespace std;

class DynamicArray {
public:
    int* data;
    int  size;

    // Конструктор — заделя памет
    DynamicArray(int n) {
        size = n;
        data = new int[size];
        for (int i = 0; i < size; i++) data[i] = 0;
        cout << "Заделена памет за " << size << " елемента." << endl;
    }

    // Деструктор — освобождава памет
    ~DynamicArray() {
        delete[] data;
        cout << "Паметта е освободена." << endl;
    }

    void set(int index, int value) {
        if (index >= 0 && index < size)
            data[index] = value;
    }

    void print() {
        cout << "[ ";
        for (int i = 0; i < size; i++) cout << data[i] << " ";
        cout << "]" << endl;
    }
};

int main() {
    DynamicArray arr(5);
    arr.set(0, 10);
    arr.set(2, 30);
    arr.set(4, 50);
    arr.print();

    return 0; // ~DynamicArray() се извиква автоматично
}
```

**Изход:**
```
Заделена памет за 5 елемента.
[ 10 0 30 0 50 ]
Паметта е освободена.
```

> 📌 Без `delete[]` в деструктора се получава **memory leak** — паметта никога не се връща на системата!

</details>

---

## 🔸 Задача 15 — Копиращ конструктор

**Концепция:** При копиране на обект с динамична памет, C++ по подразбиране копира само **указателя** (shallow copy) — двата обекта сочат към **едно и също** място. Копиращият конструктор прави **дълбоко копие** (deep copy) с нова памет.

Използвай класа от задача 14 и добави копиращ конструктор.  
Провери, че промяна в копието **не засяга** оригинала.

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
using namespace std;

class DynamicArray {
public:
    int* data;
    int  size;

    // Конструктор
    DynamicArray(int n) {
        size = n;
        data = new int[size];
        for (int i = 0; i < size; i++) data[i] = 0;
    }

    // Копиращ конструктор — прави дълбоко копие
    DynamicArray(const DynamicArray& other) {
        size = other.size;
        data = new int[size];            // нова памет!
        for (int i = 0; i < size; i++)
            data[i] = other.data[i];     // копираме стойностите
        cout << "Копиращ конструктор извикан." << endl;
    }

    // Деструктор
    ~DynamicArray() {
        delete[] data;
    }

    void set(int index, int value) {
        if (index >= 0 && index < size)
            data[index] = value;
    }

    void print() {
        cout << "[ ";
        for (int i = 0; i < size; i++) cout << data[i] << " ";
        cout << "]" << endl;
    }
};

int main() {
    DynamicArray original(4);
    original.set(0, 1);
    original.set(1, 2);
    original.set(2, 3);
    original.set(3, 4);

    DynamicArray copy = original;  // извиква копиращия конструктор
    copy.set(0, 99);               // промяна само в копието

    cout << "Оригинал: "; original.print();
    cout << "Копие:    "; copy.print();

    return 0;
}
```

**Изход:**
```
Копиращ конструктор извикан.
Оригинал: [ 1 2 3 4 ]
Копие:    [ 99 2 3 4 ]
```

> 📌 Без копиращ конструктор двата обекта щяха да сочат към **едно и също** място и `copy.set(0, 99)` щеше да промени и оригинала!

</details>

---

## 🔸 Задача 16 — Шаблон (Template) за двойка

**Концепция:** `template<typename T>` позволява да пишем клас, работещ с **произволен тип** — `int`, `double`, `char` и т.н. — без да го пишем наново за всеки.

Създай шаблонен клас `Pair<T>`, който съхранява **две стойности от тип T**.

Добави:
- Конструктор с два параметъра
- `getFirst()`, `getSecond()`
- `swap()` — разменя двете стойности
- `print()` — отпечатва двойката

Тествай с `int` и с `double`.

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
using namespace std;

template <typename T>
class Pair {
public:
    T first;
    T second;

    Pair(T a, T b) {
        first  = a;
        second = b;
    }

    T getFirst()  { return first;  }
    T getSecond() { return second; }

    void swap() {
        T temp = first;
        first  = second;
        second = temp;
    }

    void print() {
        cout << "(" << first << ", " << second << ")" << endl;
    }
};

int main() {
    Pair<int> ints(3, 7);
    cout << "Преди размяна: "; ints.print();
    ints.swap();
    cout << "След размяна:  "; ints.print();

    cout << endl;

    Pair<double> doubles(1.5, 2.8);
    cout << "Двойка double: "; doubles.print();

    return 0;
}
```

**Изход:**
```
Преди размяна: (3, 7)
След размяна:  (7, 3)

Двойка double: (1.5, 2.8)
```

> 📌 Един клас, много типове — `Pair<int>`, `Pair<double>`, `Pair<char>` работят с един и същи код!

</details>

---

## 🔸 Задача 17 — Шаблонен стек (Template + динамична памет + всички специални методи)

**Концепция:** Обединяваме всичко научено: `template`, `new`/`delete[]`, конструктор, копиращ конструктор и деструктор.

Стекът е структура "последен влязъл, пръв излязъл" (LIFO).

Създай `Stack<T>` с капацитет, зададен при създаване. Добави:
- Конструктор (заделя памет с `new`)
- Копиращ конструктор (дълбоко копие)
- Деструктор (`delete[]`)
- `push(value)` — добавя елемент
- `pop()` — връща и премахва върха
- `peek()` — връща върха без премахване
- `isEmpty()` — проверява дали е празен
- `print()` — отпечатва елементите

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
using namespace std;

template <typename T>
class Stack {
public:
    T*  data;
    int capacity;
    int top;

    // Конструктор
    Stack(int cap) {
        capacity = cap;
        data     = new T[capacity];
        top      = -1;
        cout << "Стек с капацитет " << capacity << " е създаден." << endl;
    }

    // Копиращ конструктор
    Stack(const Stack& other) {
        capacity = other.capacity;
        top      = other.top;
        data     = new T[capacity];
        for (int i = 0; i <= top; i++)
            data[i] = other.data[i];
        cout << "Стекът е копиран." << endl;
    }

    // Деструктор
    ~Stack() {
        delete[] data;
        cout << "Стекът е унищожен." << endl;
    }

    void push(T value) {
        if (top < capacity - 1)
            data[++top] = value;
        else
            cout << "Стекът е пълен!" << endl;
    }

    T pop() {
        if (!isEmpty()) return data[top--];
        cout << "Стекът е празен!" << endl;
        return T();
    }

    T peek() {
        if (!isEmpty()) return data[top];
        cout << "Стекът е празен!" << endl;
        return T();
    }

    bool isEmpty() { return top == -1; }

    void print() {
        cout << "Стек (отгоре надолу): ";
        for (int i = top; i >= 0; i--) cout << data[i] << " ";
        cout << endl;
    }
};

int main() {
    Stack<int> s(5);
    s.push(10);
    s.push(20);
    s.push(30);
    s.print();

    cout << "Връх: " << s.peek() << endl;
    cout << "Pop:  " << s.pop()  << endl;
    s.print();

    // Тест на копиращия конструктор
    Stack<int> copy = s;
    copy.push(99);
    cout << "Оригинал: "; s.print();
    cout << "Копие:    "; copy.print();

    return 0;
}
```

**Изход:**
```
Стек с капацитет 5 е създаден.
Стек (отгоре надолу): 30 20 10
Връх: 30
Pop:  30
Стек (отгоре надолу): 20 10
Стекът е копиран.
Оригинал: Стек (отгоре надолу): 20 10
Копие:    Стек (отгоре надолу): 99 20 10
Стекът е унищожен.
Стекът е унищожен.
```

> 📌 Виждат се две съобщения "Стекът е унищожен" — по едно за оригинала и за копието. Всеки управлява **собствената си памет**!

</details>

---

## 📝 Обобщение на ключови понятия

| Понятие | Синтаксис | Обяснение |
|---------|-----------|-----------|
| `class` | `class Foo { };` | Шаблон за създаване на обекти |
| `public:` | `public: int x;` | Полетата/методите са достъпни отвън |
| Конструктор | `Foo(int x) { ... }` | Извиква се автоматично при създаване |
| Деструктор | `~Foo() { ... }` | Извиква се автоматично при унищожаване |
| Копиращ конструктор | `Foo(const Foo& other)` | Дълбоко копие при `Foo b = a;` |
| `new` / `delete` | `int* p = new int[n];` | Динамична памет в heap-а |
| `delete[]` | `delete[] p;` | Освобождава динамичен масив |
| Template | `template<typename T>` | Клас, работещ с произволен тип |
| Обект | `Foo obj;` | Конкретен екземпляр на клас |
| `.` (точка) | `obj.method()` | Достъп до поле/метод |
