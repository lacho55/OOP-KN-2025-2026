# Базови задачи за структури в C++

---

## Задача 1: Студент

Създайте структура `Student`, която съдържа:
- `name` (string) – име на студента
- `age` (int) – възраст
- `grade` (double) – средна оценка

Напишете програма, която въвежда данни за **един студент** от клавиатурата и ги извежда на екрана.

<details>
<summary>📖 Решение</summary>

```cpp
#include <iostream>
#include <string>
using namespace std;

struct Student {
    string name;
    int age;
    double grade;
};

int main() {
    Student s;

    cout << "Име: ";
    getline(cin, s.name);
    cout << "Възраст: ";
    cin >> s.age;
    cout << "Оценка: ";
    cin >> s.grade;

    cout << "\n--- Данни за студента ---\n";
    cout << "Име: " << s.name << endl;
    cout << "Възраст: " << s.age << endl;
    cout << "Оценка: " << s.grade << endl;

    return 0;
}
```

</details>

---

## Задача 2: Точка в равнината

Създайте структура `Point` с полета `x` и `y` (double). Напишете функция `distance`, която приема две точки и връща разстоянието между тях.

<details>
<summary>📖 Решение</summary>

```cpp
#include <iostream>
#include <cmath>
using namespace std;

struct Point {
    double x;
    double y;
};

double distance(Point a, Point b) {
    return sqrt((a.x - b.x) * (a.x - b.x) + (a.y - b.y) * (a.y - b.y));
}

int main() {
    Point p1 = {1.0, 2.0};
    Point p2 = {4.0, 6.0};

    cout << "Разстояние: " << distance(p1, p2) << endl;

    return 0;
}
```

</details>

---

## Задача 3: Правоъгълник

Създайте структура `Rectangle` с полета `width` и `height` (double). Напишете функции за:
- намиране на **лицето**
- намиране на **периметъра**

<details>
<summary>📖 Решение</summary>

```cpp
#include <iostream>
using namespace std;

struct Rectangle {
    double width;
    double height;
};

double area(Rectangle r) {
    return r.width * r.height;
}

double perimeter(Rectangle r) {
    return 2 * (r.width + r.height);
}

int main() {
    Rectangle r = {5.0, 3.0};

    cout << "Лице: " << area(r) << endl;
    cout << "Периметър: " << perimeter(r) << endl;

    return 0;
}
```

</details>

---

## Задача 4: Масив от студенти – най-висока оценка

Създайте масив от 5 студента (използвайте структурата `Student` от Задача 1). Въведете данните и намерете студента с **най-висока оценка**.

<details>
<summary>📖 Решение</summary>

```cpp
#include <iostream>
#include <string>
using namespace std;

struct Student {
    string name;
    int age;
    double grade;
};

int main() {
    const int N = 5;
    Student students[N];

    for (int i = 0; i < N; i++) {
        cout << "Студент " << i + 1 << ":\n";
        cout << "  Име: ";
        cin.ignore();
        getline(cin, students[i].name);
        cout << "  Възраст: ";
        cin >> students[i].age;
        cout << "  Оценка: ";
        cin >> students[i].grade;
    }

    int bestIndex = 0;
    for (int i = 1; i < N; i++) {
        if (students[i].grade > students[bestIndex].grade) {
            bestIndex = i;
        }
    }

    cout << "\nСтудент с най-висока оценка:\n";
    cout << "Име: " << students[bestIndex].name << endl;
    cout << "Оценка: " << students[bestIndex].grade << endl;

    return 0;
}
```

</details>

---

## Задача 5: Дата

Създайте структура `Date` с полета `day`, `month`, `year` (int). Напишете функция `isLeapYear`, която проверява дали годината е **високосна**, и функция `printDate`, която извежда датата във формат `DD.MM.YYYY`.

<details>
<summary>📖 Решение</summary>

```cpp
#include <iostream>
#include <iomanip>
using namespace std;

struct Date {
    int day;
    int month;
    int year;
};

bool isLeapYear(int year) {
    return (year % 4 == 0 && year % 100 != 0) || (year % 400 == 0);
}

void printDate(Date d) {
    cout << setfill('0') << setw(2) << d.day << "."
         << setw(2) << d.month << "."
         << setw(4) << d.year << endl;
}

int main() {
    Date d = {29, 2, 2024};

    printDate(d);

    if (isLeapYear(d.year)) {
        cout << d.year << " е високосна година." << endl;
    } else {
        cout << d.year << " НЕ е високосна година." << endl;
    }

    return 0;
}
```

</details>

---

## Задача 6: Книга – сортиране по цена

Създайте структура `Book` с полета:
- `title` (string)
- `author` (string)
- `price` (double)

Въведете данни за 3 книги и ги изведете **сортирани по цена** (от най-евтина към най-скъпа).

<details>
<summary>📖 Решение</summary>

```cpp
#include <iostream>
#include <string>
using namespace std;

struct Book {
    string title;
    string author;
    double price;
};

int main() {
    const int N = 3;
    Book books[N];

    for (int i = 0; i < N; i++) {
        cout << "Книга " << i + 1 << ":\n";
        cout << "  Заглавие: ";
        cin.ignore();
        getline(cin, books[i].title);
        cout << "  Автор: ";
        getline(cin, books[i].author);
        cout << "  Цена: ";
        cin >> books[i].price;
    }

    // Bubble sort по цена
    for (int i = 0; i < N - 1; i++) {
        for (int j = 0; j < N - 1 - i; j++) {
            if (books[j].price > books[j + 1].price) {
                Book temp = books[j];
                books[j] = books[j + 1];
                books[j + 1] = temp;
            }
        }
    }

    cout << "\nКниги сортирани по цена:\n";
    for (int i = 0; i < N; i++) {
        cout << books[i].title << " - " << books[i].author
             << " - " << books[i].price << " лв." << endl;
    }

    return 0;
}
```

</details>

---

## Задача 7: Вложени структури – Адрес и Човек

Създайте структура `Address` с полета `city` и `street` (string). Създайте структура `Person` с полета `name` (string), `age` (int) и `address` (от тип `Address`). Въведете и изведете данните за един човек.

<details>
<summary>📖 Решение</summary>

```cpp
#include <iostream>
#include <string>
using namespace std;

struct Address {
    string city;
    string street;
};

struct Person {
    string name;
    int age;
    Address address;
};

int main() {
    Person p;

    cout << "Име: ";
    getline(cin, p.name);
    cout << "Възраст: ";
    cin >> p.age;
    cin.ignore();
    cout << "Град: ";
    getline(cin, p.address.city);
    cout << "Улица: ";
    getline(cin, p.address.street);

    cout << "\n--- Данни ---\n";
    cout << "Име: " << p.name << endl;
    cout << "Възраст: " << p.age << endl;
    cout << "Адрес: ул. " << p.address.street
         << ", гр. " << p.address.city << endl;

    return 0;
}
```

</details>

---

## Задача 8: Предаване по референция

Създайте структура `Counter` с поле `count` (int). Напишете функция `increment`, която приема `Counter` **по референция** и увеличава `count` с 1. Демонстрирайте разликата между предаване по стойност и по референция.

<details>
<summary>📖 Решение</summary>

```cpp
#include <iostream>
using namespace std;

struct Counter {
    int count;
};

// По стойност – НЕ променя оригинала
void incrementByValue(Counter c) {
    c.count++;
    cout << "Вътре (по стойност): " << c.count << endl;
}

// По референция – ПРОМЕНЯ оригинала
void incrementByRef(Counter &c) {
    c.count++;
    cout << "Вътре (по референция): " << c.count << endl;
}

int main() {
    Counter c = {0};

    cout << "Начално: " << c.count << endl;

    incrementByValue(c);
    cout << "След по стойност: " << c.count << endl;

    incrementByRef(c);
    cout << "След по референция: " << c.count << endl;

    return 0;
}
```

**Изход:**
```
Начално: 0
Вътре (по стойност): 1
След по стойност: 0
Вътре (по референция): 1
След по референция: 1
```

</details>

---

## Задача 9: Комплексни числа

Създайте структура `Complex` с полета `real` и `imag` (double). Напишете функции за **събиране** и **умножение** на две комплексни числа.

<details>
<summary>📖 Решение</summary>

```cpp
#include <iostream>
using namespace std;

struct Complex {
    double real;
    double imag;
};

Complex add(Complex a, Complex b) {
    return {a.real + b.real, a.imag + b.imag};
}

// (a + bi)(c + di) = (ac - bd) + (ad + bc)i
Complex multiply(Complex a, Complex b) {
    return {
        a.real * b.real - a.imag * b.imag,
        a.real * b.imag + a.imag * b.real
    };
}

void print(Complex c) {
    cout << c.real;
    if (c.imag >= 0) cout << " + " << c.imag << "i";
    else cout << " - " << -c.imag << "i";
    cout << endl;
}

int main() {
    Complex a = {3, 2};
    Complex b = {1, 4};

    cout << "a = "; print(a);
    cout << "b = "; print(b);
    cout << "a + b = "; print(add(a, b));
    cout << "a * b = "; print(multiply(a, b));

    return 0;
}
```

</details>

---