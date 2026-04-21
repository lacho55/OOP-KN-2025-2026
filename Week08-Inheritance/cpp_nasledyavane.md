# C++ ООП — Наследяване

---

## 🔹 Задача 1 — Базово наследяване

**Концепция:** Производният клас наследява всички `public` и `protected` членове на базовия клас. Използваме синтаксиса `class Derived : public Base`. Конструкторът на производния клас трябва да извика конструктора на базовия чрез `: Base(args)`.

Създай базов клас `Animal` с поле `name` (char масив, 50 символа) и метод `speak()`, който отпечатва `"[name] издава звук."`.

Създай производен клас `Dog`, който наследява `Animal` и презаписва `speak()` с `"[name] казва: Бау!"`.

Създай обекти от двата класа и извикай `speak()`.

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
#include <cstring>
using namespace std;

class Animal {
public:
    char name[50];

    Animal(const char* n) {
        strcpy(name, n);
    }

    void speak() {
        cout << name << " издава звук." << endl;
    }
};

class Dog : public Animal {
public:
    Dog(const char* n) : Animal(n) { }  // извикваме базовия конструктор

    void speak() {
        cout << name << " казва: Бау!" << endl;
    }
};

int main() {
    Animal a("Животно");
    Dog d("Рекс");

    a.speak();
    d.speak();

    return 0;
}
```

**Изход:**
```
Животно издава звук.
Рекс казва: Бау!
```

> 📌 `Dog` наследява полето `name` от `Animal`, затова имаме достъп до него директно. Конструкторът на производния клас **трябва** да извика конструктора на базовия чрез `: Animal(n)`.

</details>

---

## 🔹 Задача 2 — Конструктор и деструктор при наследяване

**Концепция:** Когато създаваме производен обект, първо се извиква конструкторът на **базовия** клас, после на производния. При унищожаване — обратно: първо производен, после базов.

Създай клас `Base` с конструктор и деструктор, които отпечатват съобщения.

Създай клас `Derived : public Base` също с конструктор и деструктор.

Създай обект от `Derived` и наблюдавай реда на извикване.

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
using namespace std;

class Base {
public:
    Base() {
        cout << "Base конструктор" << endl;
    }
    ~Base() {
        cout << "Base деструктор" << endl;
    }
};

class Derived : public Base {
public:
    Derived() {
        cout << "Derived конструктор" << endl;
    }
    ~Derived() {
        cout << "Derived деструктор" << endl;
    }
};

int main() {
    cout << "--- Създаване ---" << endl;
    Derived d;
    cout << "--- Унищожаване ---" << endl;
    return 0;
}
```

**Изход:**
```
--- Създаване ---
Base конструктор
Derived конструктор
--- Унищожаване ---
Derived деструктор
Base деструктор
```

> 📌 Конструкторите вървят **отгоре надолу** (Base → Derived), деструкторите — **отдолу нагоре** (Derived → Base). Точно обратен ред!

</details>

---

## 🔹 Задача 3 — `protected` членове

**Концепция:** Членовете с `protected` достъп са скрити за външен код (като `private`), но са достъпни за производните класове. Това е ключовата разлика между `private` и `protected` при наследяване.

Създай клас `Person` с `protected` полета `name` и `age`.

Създай клас `Student : public Person` с допълнително поле `grade` (оценка).

Добави метод `printInfo()` в `Student`, който отпечатва и трите полета.

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
#include <cstring>
using namespace std;

class Person {
protected:              // достъпно за производните класове
    char name[50];
    int age;

public:
    Person(const char* n, int a) {
        strcpy(name, n);
        age = a;
    }
};

class Student : public Person {
    double grade;

public:
    Student(const char* n, int a, double g) : Person(n, a) {
        grade = g;
    }

    void printInfo() {
        // name и age са protected — достъпни тук
        cout << "Студент: " << name
             << ", Възраст: " << age
             << ", Оценка: " << grade << endl;
    }
};

int main() {
    Student s("Иван", 20, 5.50);
    s.printInfo();

    // s.name = "Петър";  // ГРЕШКА! protected е скрито навън

    return 0;
}
```

**Изход:**
```
Студент: Иван, Възраст: 20, Оценка: 5.5
```

> 📌 `private` = само собственият клас. `protected` = собственият клас + производните. `public` = всички.

</details>

---

## 🔹 Задача 4 — `virtual` и `override` — полиморфизъм

**Концепция:** Когато метод е деклариран като `virtual` в базовия клас, C++ извиква **правилната** версия дори при извикване през указател към базовия клас. Ключовата дума `override` в производния клас потвърждава, че презаписваме виртуален метод.

Създай клас `Shape` с `virtual` метод `area()`, който връща `0.0`.

Създай класове `Circle` и `Rectangle`, които наследяват `Shape` и `override`-ват `area()`.

Използвай указател `Shape*` и извикай `area()` — провери, че се извиква правилната версия.

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
#include <cmath>
using namespace std;

class Shape {
public:
    virtual double area() {   // virtual = полиморфизъм
        return 0.0;
    }
};

class Circle : public Shape {
    double radius;
public:
    Circle(double r) : radius(r) {}

    double area() override {  // override = презаписваме виртуалния метод
        return 3.14159 * radius * radius;
    }
};

class Rectangle : public Shape {
    double width, height;
public:
    Rectangle(double w, double h) : width(w), height(h) {}

    double area() override {
        return width * height;
    }
};

int main() {
    Shape* s1 = new Circle(5.0);
    Shape* s2 = new Rectangle(4.0, 6.0);

    cout << "Кръг: "          << s1->area() << endl;
    cout << "Правоъгълник: "  << s2->area() << endl;

    delete s1;
    delete s2;
    return 0;
}
```

**Изход:**
```
Кръг: 78.5398
Правоъгълник: 24
```

> 📌 Без `virtual` C++ би извикал `Shape::area()` (0) и за двата обекта — защото решението се взима по **типа на указателя**. С `virtual` решението се взима по **реалния тип на обекта**!

</details>

---

## 🔹 Задача 5 — Абстрактен клас и чисто виртуален метод

**Концепция:** Чисто виртуален метод (`= 0`) прави класа **абстрактен** — не може да се създаде обект директно от него. Производните класове **трябва** да имплементират тези методи, иначе и те стават абстрактни.

Направи `Shape` от задача 4 абстрактен — добави чисто виртуален метод `describe()`.

Имплементирай `describe()` в `Circle` и `Rectangle`.

Направи масив от `Shape*` и извикай двата метода за всяка фигура.

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
using namespace std;

class Shape {
public:
    virtual double area()    = 0;  // чисто виртуален
    virtual void describe()  = 0;  // чисто виртуален

    virtual ~Shape() {}            // виртуален деструктор (важно!)
};

class Circle : public Shape {
    double radius;
public:
    Circle(double r) : radius(r) {}

    double area() override {
        return 3.14159 * radius * radius;
    }

    void describe() override {
        cout << "Кръг с радиус " << radius << endl;
    }
};

class Rectangle : public Shape {
    double w, h;
public:
    Rectangle(double w, double h) : w(w), h(h) {}

    double area() override {
        return w * h;
    }

    void describe() override {
        cout << "Правоъгълник " << w << "x" << h << endl;
    }
};

int main() {
    // Shape s;    // ГРЕШКА! Абстрактен клас!

    Shape* shapes[2];
    shapes[0] = new Circle(3.0);
    shapes[1] = new Rectangle(5.0, 2.0);

    for (int i = 0; i < 2; i++) {
        shapes[i]->describe();
        cout << "  Лице: " << shapes[i]->area() << endl;
        delete shapes[i];
    }

    return 0;
}
```

**Изход:**
```
Кръг с радиус 3
  Лице: 28.2743
Правоъгълник 5x2
  Лице: 10
```

> 📌 Абстрактният клас е „договор" — гарантира, че всеки производен клас **има** тези методи. Масивът от `Shape*` работи еднакво за всякакви фигури!

</details>

---

## 🔹 Задача 6 — `virtual` деструктор

**Концепция:** Когато изтриваме обект чрез указател към базовия клас (`delete base_ptr`), без `virtual` деструктор се извиква **само** деструкторът на базовия клас — деструкторът на производния **не** се извиква. Това е потенциален memory leak.

Демонстрирай разликата между клас **без** и **със** `virtual` деструктор.

Създай два сходни класа — единият с `virtual ~Base()`, другият без.

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
using namespace std;

// Версия БЕЗ virtual деструктор (лошо!)
class BadBase {
public:
    ~BadBase() { cout << "BadBase деструктор" << endl; }
};

class BadDerived : public BadBase {
public:
    ~BadDerived() { cout << "BadDerived деструктор" << endl; }
};

// Версия СЪС virtual деструктор (правилно!)
class GoodBase {
public:
    virtual ~GoodBase() { cout << "GoodBase деструктор" << endl; }
};

class GoodDerived : public GoodBase {
public:
    ~GoodDerived() { cout << "GoodDerived деструктор" << endl; }
};

int main() {
    cout << "--- БЕЗ virtual деструктор ---" << endl;
    BadBase* b = new BadDerived();
    delete b;    // извиква САМО BadBase деструктор!

    cout << "--- СЪС virtual деструктор ---" << endl;
    GoodBase* g = new GoodDerived();
    delete g;    // извиква и двата деструктора

    return 0;
}
```

**Изход:**
```
--- БЕЗ virtual деструктор ---
BadBase деструктор
--- СЪС virtual деструктор ---
GoodDerived деструктор
GoodBase деструктор
```

> 📌 Правило: ако класът има поне един `virtual` метод, деструкторът му **трябва** да е `virtual`. Иначе `delete base_ptr` води до непълно унищожаване на обекта!

</details>

---

## 🔹 Задача 7 — Многостепенно наследяване

**Концепция:** C++ позволява верига от наследяване: A → B → C. Клас C наследява всичко от B, което от своя страна е наследило всичко от A. Можем да извикаме конкретна версия на виртуален метод чрез `Base::method()`.

Създай йерархия: `Vehicle` (базов) → `Car` → `ElectricCar`.

Всеки клас добавя собствени полета и метод `info()`. `ElectricCar::info()` трябва да извика `Car::info()` вместо да повтаря кода.

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
#include <cstring>
using namespace std;

class Vehicle {
protected:
    char brand[50];
    int year;
public:
    Vehicle(const char* b, int y) {
        strcpy(brand, b);
        year = y;
    }
    virtual void info() {
        cout << brand << " (" << year << ")";
    }
};

class Car : public Vehicle {
protected:
    int doors;
public:
    Car(const char* b, int y, int d) : Vehicle(b, y) {
        doors = d;
    }
    void info() override {
        Vehicle::info();        // извикваме базовия info()
        cout << ", " << doors << " врати";
    }
};

class ElectricCar : public Car {
    int range;
public:
    ElectricCar(const char* b, int y, int d, int r) : Car(b, y, d) {
        range = r;
    }
    void info() override {
        Car::info();            // извикваме Car::info()
        cout << ", пробег: " << range << " км" << endl;
    }
};

int main() {
    Vehicle v("Генерик", 2000);
    Car c("Toyota", 2020, 4);
    ElectricCar e("Tesla", 2023, 4, 500);

    v.info(); cout << endl;
    c.info(); cout << endl;
    e.info();

    return 0;
}
```

**Изход:**
```
Генерик (2000)
Toyota (2020), 4 врати
Tesla (2023), 4 врати, пробег: 500 км
```

> 📌 `Base::method()` ни позволява да извикаме конкретна версия на виртуален метод — вместо да презаписваме целия код, **надграждаме** съществуващото!

</details>

---

## 🔸 Задача 8 — Полиморфизъм с масив от указатели

**Концепция:** Полиморфизмът е особено полезен с масив от `Base*` указатели — всеки сочи към различен производен обект, но един и същи код работи за всеки тип обект.

Създай абстрактен клас `Employee` с чисто виртуален метод `salary()`.

Създай класове `FullTime` (фиксирана месечна заплата) и `PartTime` (почасова ставка × отработени часове).

Направи масив от `Employee*`, отпечатай всеки служител и изчисли общата сума на заплатите.

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
#include <cstring>
using namespace std;

class Employee {
protected:
    char name[50];
public:
    Employee(const char* n) { strcpy(name, n); }

    virtual double salary() = 0;

    virtual void print() {
        cout << name << ": " << salary() << " лв." << endl;
    }

    virtual ~Employee() {}
};

class FullTime : public Employee {
    double monthlySalary;
public:
    FullTime(const char* n, double s) : Employee(n) {
        monthlySalary = s;
    }
    double salary() override {
        return monthlySalary;
    }
};

class PartTime : public Employee {
    double hourlyRate;
    int hoursWorked;
public:
    PartTime(const char* n, double rate, int hours) : Employee(n) {
        hourlyRate  = rate;
        hoursWorked = hours;
    }
    double salary() override {
        return hourlyRate * hoursWorked;
    }
};

int main() {
    Employee* staff[4];
    staff[0] = new FullTime("Иван",  3000);
    staff[1] = new PartTime("Мария", 25, 80);
    staff[2] = new FullTime("Петър", 2500);
    staff[3] = new PartTime("Анна",  30, 60);

    double total = 0;
    for (int i = 0; i < 4; i++) {
        staff[i]->print();
        total += staff[i]->salary();
    }

    cout << "Общо: " << total << " лв." << endl;

    for (int i = 0; i < 4; i++) delete staff[i];
    return 0;
}
```

**Изход:**
```
Иван: 3000 лв.
Мария: 2000 лв.
Петър: 2500 лв.
Анна: 1800 лв.
Общо: 9300 лв.
```

> 📌 Цикълът `for` не знае дали обектът е `FullTime` или `PartTime` — полиморфизмът се грижи за правилното извикване. Можем да добавяме нови типове служители без да пипаме `main()`!

</details>

---

## 🔸 Задача 9 — Йерархия с `virtual` методи и `operator<<`

**Концепция:** Комбинираме наследяване с овърлоудване на оператори. `operator<<` е дефиниран само в базовия клас и работи полиморфно чрез виртуален метод — производните класове само имплементират виртуалния метод.

Създай абстрактен клас `BankAccount` с поле `balance` и методи:
- `virtual void deposit(double)` — добавя сума (имплементирано в базовия)
- `virtual bool withdraw(double) = 0` — теглене (абстрактно)
- `virtual void printType() const = 0` — отпечатва типа (абстрактно)
- `friend operator<<` — извиква `printType()` и отпечатва баланса

Производни класове:
- `SavingsAccount` — не позволява баланса да падне под 100 лв.
- `CurrentAccount` — позволява овърдрафт до -500 лв.

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
using namespace std;

class BankAccount {
protected:
    double balance;

public:
    BankAccount(double initial) : balance(initial) {}

    virtual void deposit(double amount) {
        balance += amount;
    }

    virtual bool withdraw(double amount) = 0;
    virtual void printType() const = 0;

    double getBalance() const { return balance; }

    // operator<< използва виртуалния printType()
    friend ostream& operator<<(ostream& os, const BankAccount& acc) {
        acc.printType();
        os << "  Баланс: " << acc.balance << " лв.";
        return os;
    }

    virtual ~BankAccount() {}
};

class SavingsAccount : public BankAccount {
public:
    SavingsAccount(double initial) : BankAccount(initial) {}

    bool withdraw(double amount) override {
        if (balance - amount < 100.0) {
            cout << "Отказано! Минимален баланс: 100 лв." << endl;
            return false;
        }
        balance -= amount;
        return true;
    }

    void printType() const override {
        cout << "[Спестовна сметка]";
    }
};

class CurrentAccount : public BankAccount {
public:
    CurrentAccount(double initial) : BankAccount(initial) {}

    bool withdraw(double amount) override {
        if (balance - amount < -500.0) {
            cout << "Отказано! Лимит на овърдрафта: -500 лв." << endl;
            return false;
        }
        balance -= amount;
        return true;
    }

    void printType() const override {
        cout << "[Разплащателна сметка]";
    }
};

int main() {
    BankAccount* accounts[2];
    accounts[0] = new SavingsAccount(500);
    accounts[1] = new CurrentAccount(200);

    cout << *accounts[0] << endl;
    cout << *accounts[1] << endl;

    cout << "\n--- Операции ---" << endl;
    accounts[0]->withdraw(450);   // ще откаже (остава под 100)
    accounts[0]->withdraw(350);   // ок
    cout << *accounts[0] << endl;

    accounts[1]->withdraw(600);   // ок (овърдрафт до -500)
    accounts[1]->withdraw(200);   // ще откаже (< -500)
    cout << *accounts[1] << endl;

    for (int i = 0; i < 2; i++) delete accounts[i];
    return 0;
}
```

**Изход:**
```
[Спестовна сметка]  Баланс: 500 лв.
[Разплащателна сметка]  Баланс: 200 лв.

--- Операции ---
Отказано! Минимален баланс: 100 лв.
[Спестовна сметка]  Баланс: 150 лв.
[Разплащателна сметка]  Баланс: -400 лв.
Отказано! Лимит на овърдрафта: -500 лв.
[Разплащателна сметка]  Баланс: -400 лв.
```

> 📌 `operator<<` е дефиниран **само веднъж** в базовия клас и работи за всички производни чрез виртуалния `printType()`. Добавянето на нов тип сметка не изисква промяна на оператора!

</details>

---

## 🔸 Задача 10 — Пълна йерархия: абстрактен клас, `override`, масив, `operator<<`

**Концепция:** Финална задача — обединяваме всичко: абстрактен базов клас, многостепенно наследяване, полиморфизъм с масив, виртуален деструктор и `operator<<` чрез виртуален метод.

Създай йерархия за геометрични тела в 3D:
- `Shape3D` (абстрактен) — чисто виртуални `volume()`, `surfaceArea()`, `printName()`
- `Cube` — страна `a`
- `Sphere` — радиус `r`
- `Cylinder` — радиус `r` и височина `h`

`operator<<` се дефинира само в `Shape3D` и работи за всички тела чрез виртуалния `printName()`.

Намери обекта с най-голям обем от масив от `Shape3D*`.

> 💡 Формули: куб `V = a³`, сфера `V = (4/3)πr³`, цилиндър `V = πr²h`

<details>
<summary>💡 Решение</summary>

```cpp
#include <iostream>
#include <cmath>
using namespace std;

const double PI = 3.14159265;

class Shape3D {
public:
    virtual double volume()      const = 0;
    virtual double surfaceArea() const = 0;
    virtual void   printName()   const = 0;

    friend ostream& operator<<(ostream& os, const Shape3D& s) {
        s.printName();
        os << "  V=" << s.volume()
           << "  S=" << s.surfaceArea();
        return os;
    }

    virtual ~Shape3D() {}
};

class Cube : public Shape3D {
    double a;
public:
    Cube(double side) : a(side) {}

    double volume()      const override { return a * a * a; }
    double surfaceArea() const override { return 6 * a * a; }
    void   printName()   const override { cout << "Куб(a=" << a << ")"; }
};

class Sphere : public Shape3D {
    double r;
public:
    Sphere(double radius) : r(radius) {}

    double volume()      const override { return (4.0 / 3.0) * PI * r * r * r; }
    double surfaceArea() const override { return 4 * PI * r * r; }
    void   printName()   const override { cout << "Сфера(r=" << r << ")"; }
};

class Cylinder : public Shape3D {
    double r, h;
public:
    Cylinder(double radius, double height) : r(radius), h(height) {}

    double volume()      const override { return PI * r * r * h; }
    double surfaceArea() const override { return 2 * PI * r * h + 2 * PI * r * r; }
    void   printName()   const override { cout << "Цилиндър(r=" << r << ", h=" << h << ")"; }
};

int main() {
    const int N = 4;
    Shape3D* shapes[N];
    shapes[0] = new Cube(3.0);
    shapes[1] = new Sphere(2.0);
    shapes[2] = new Cylinder(2.0, 5.0);
    shapes[3] = new Cube(4.0);

    for (int i = 0; i < N; i++)
        cout << *shapes[i] << endl;

    // Намираме тялото с най-голям обем
    int maxIdx = 0;
    for (int i = 1; i < N; i++)
        if (shapes[i]->volume() > shapes[maxIdx]->volume())
            maxIdx = i;

    cout << "\nНай-голям обем: ";
    shapes[maxIdx]->printName();
    cout << " -> " << shapes[maxIdx]->volume() << endl;

    for (int i = 0; i < N; i++) delete shapes[i];
    return 0;
}
```

**Изход:**
```
Куб(a=3)  V=27  S=54
Сфера(r=2)  V=33.5103  S=50.2655
Цилиндър(r=2, h=5)  V=62.8318  S=87.9646
Куб(a=4)  V=64  S=96

Най-голям обем: Куб(a=4) -> 64
```

> 📌 `operator<<` е дефиниран **само в `Shape3D`** — работи за всички производни чрез виртуалния `printName()`. Добавянето на ново тяло изисква само новия клас, без промени в `main()` или в оператора!

</details>

---

## 📝 Обобщение на ключови понятия

| Понятие | Синтаксис | Обяснение |
|---------|-----------|-----------|
| Базов клас | `class Animal { ... };` | Родителският клас, от който се наследява |
| Производен клас | `class Dog : public Animal { };` | Наследява полетата и методите на базовия |
| `public` наследяване | `: public Base` | `public` → `public`, `protected` → `protected` |
| Конструктор на база | `: Base(args)` | Извиква се **преди** тялото на производния |
| `override` | `void speak() override` | Презаписваме виртуален метод на базовия |
| `virtual` | `virtual void speak()` | Позволява полиморфизъм при указатели |
| Абстрактен метод | `virtual void f() = 0;` | Чисто виртуален — класът не може да се инстанцира |
| Абстрактен клас | Клас с поне един `= 0` | Не може да се създаде обект директно |
| Полиморфизъм | `Base* p = new Derived();` | Указател/референция към производен клас |
| `virtual` деструктор | `virtual ~Base()` | Задължително при полиморфизъм с `new`! |
| `Base::method()` | `Vehicle::info()` | Извикване на конкретна версия от йерархията |
