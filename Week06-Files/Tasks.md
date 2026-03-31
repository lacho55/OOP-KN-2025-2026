# C++ Задачи — Файлове и Структури

---

## Задача 1

Напишете програма, която отпечатва собствения си код.

<details>
<summary>Решение</summary>

```cpp
// Task1_Quine.cpp
#include <iostream>
#include <fstream>

int main() {
    // Отваряме самия изходен файл за четене
    std::ifstream file("Task1_Quine.cpp");

    if (!file.is_open()) {
        std::cout << "Грешка: файлът не може да бъде отворен!" << std::endl;
        return 1;
    }

    char ch;
    while (file.get(ch)) {
        std::cout << ch;
    }

    file.close();
    return 0;
}
```

> 💡 Програмата отваря собствения си `.cpp` файл и го отпечатва символ по символ. Файлът трябва да се казва точно `Task1_Quine.cpp`.

</details>

---

## Задача 2

Напишете функция, която приема ime на съществуващ файл и копира съдържанието му в нов файл.

<details>
<summary>Решение</summary>

```cpp
#include <iostream>
#include <fstream>

void copyFile(const char* sourceName, const char* destName) {
    std::ifstream source(sourceName, std::ios::binary);
    if (!source.is_open()) {
        std::cout << "Грешка: не може да се отвори " << sourceName << std::endl;
        return;
    }

    std::ofstream dest(destName, std::ios::binary);
    if (!dest.is_open()) {
        std::cout << "Грешка: не може да се създаде " << destName << std::endl;
        source.close();
        return;
    }

    // Четем и записваме блокове от 512 байта
    char buffer[512];
    while (source.read(buffer, sizeof(buffer))) {
        dest.write(buffer, source.gcount());
    }
    // Записваме последния непълен блок (ако има)
    dest.write(buffer, source.gcount());

    source.close();
    dest.close();
    std::cout << "Файлът е копиран успешно!" << std::endl;
}

int main() {
    copyFile("source.txt", "copy.txt");
    return 0;
}
```

> 💡 Използваме `std::ios::binary`, за да работи правилно и с нетекстови файлове. Четем на парчета (buffer), за да не зареждаме целия файл в паметта наведнъж.

</details>

---

## Задача 3

Да се имплементира структура `Student`, характеризираща се с:
- факултетен номер
- име
- среден успех

Да се реализират методи **write/read**, приемащи някакъв поток, които съответно да четат данни за студента и да могат да записват студент във файл.

Да се направят функции **writeStudents/readStudents**, които получават масив от студенти и поток към файл.

<details>
<summary>Решение</summary>

```cpp
#include <iostream>
#include <fstream>
#include <cstring>

struct Student {
    int    fn;          // факултетен номер
    char   name[64];   // име
    double grade;      // среден успех

    // Записва студента в поток (бинарно)
    void write(std::ofstream& out) const {
        out.write((const char*)&fn,    sizeof(fn));
        out.write((const char*)name,   sizeof(name));
        out.write((const char*)&grade, sizeof(grade));
    }

    // Чете студента от поток (бинарно)
    void read(std::ifstream& in) {
        in.read((char*)&fn,    sizeof(fn));
        in.read((char*)name,   sizeof(name));
        in.read((char*)&grade, sizeof(grade));
    }

    void print() const {
        std::cout << "FN: "     << fn
                  << ", Name: " << name
                  << ", Grade: "<< grade << std::endl;
    }
};

// Записва масив от студенти във файл
void writeStudents(const Student* students, int count, std::ofstream& out) {
    out.write((const char*)&count, sizeof(count)); // записваме броя
    for (int i = 0; i < count; i++) {
        students[i].write(out);
    }
}

// Чете масив от студенти от файл; връща броя на прочетените
int readStudents(Student* students, int maxCount, std::ifstream& in) {
    int count = 0;
    in.read((char*)&count, sizeof(count));
    if (count > maxCount) count = maxCount;
    for (int i = 0; i < count; i++) {
        students[i].read(in);
    }
    return count;
}

int main() {
    // --- Запис ---
    Student arr[3];
    arr[0] = { 70001, "Ivan Ivanov",     5.50 };
    arr[1] = { 70002, "Maria Petrova",   4.75 };
    arr[2] = { 70003, "Georgi Georgiev", 6.00 };

    std::ofstream out("students.bin", std::ios::binary);
    writeStudents(arr, 3, out);
    out.close();

    // --- Четене ---
    Student loaded[10];
    std::ifstream in("students.bin", std::ios::binary);
    int n = readStudents(loaded, 10, in);
    in.close();

    std::cout << "Прочетени студенти: " << n << std::endl;
    for (int i = 0; i < n; i++) {
        loaded[i].print();
    }
    return 0;
}
```

> 💡 Данните се записват в **бинарен** формат. Броят на студентите се записва в началото на файла, за да знаем колко да четем при зареждане.

</details>

---

## Задача 4

Напишете програма, която чете CSV файл със студенти със следните 4 полета: Първо име, Фамилно име, Имейл, Факултетен номер.

1. Със стартирането на програмата потребителят да въвежда име на файл, който да бъде зареден в паметта.
2. Функция, която отпечатва информация за студента при подаден **факултетен номер**.
3. Функция, която по подаден **факултетен номер и низ**, променя email-а на студента.
4. Функция, която приема низ (ime на файл) и запазва студентите в CSV формат.

**Пример:**
```
Open file:
>students.csv
File successfully opened!
>print 70000
Name = Ivan Ivanov, Email: ivan@gmail.com, FN: 70000
>edit 70000 ivan@abv.bg
>print 70000
Name = Ivan Ivanov, Email: ivan@abv.bg, FN: 70000
>save students2.csv
file students2.csv successfully saved!
```

<details>
<summary>Решение</summary>

```cpp
#include <iostream>
#include <fstream>
#include <cstring>
#include <cstdlib>

const int MAX_STUDENTS = 200;
const int MAX_STR      = 100;

struct Student {
    char firstName[MAX_STR];
    char lastName[MAX_STR];
    char email[MAX_STR];
    int  fn;
};

// Разделя CSV ред по запетая и попълва студент
void parseLine(char* line, Student& s) {
    char* token = strtok(line, ",");
    if (token) strncpy(s.firstName, token, MAX_STR - 1);

    token = strtok(nullptr, ",");
    if (token) strncpy(s.lastName, token, MAX_STR - 1);

    token = strtok(nullptr, ",");
    if (token) strncpy(s.email, token, MAX_STR - 1);

    token = strtok(nullptr, ",");
    if (token) s.fn = atoi(token);
}

// Зарежда CSV файл; връща броя студенти или -1 при грешка
int loadFile(const char* filename, Student students[]) {
    std::ifstream file(filename);
    if (!file.is_open()) return -1;

    int  count = 0;
    char line[512];
    while (file.getline(line, sizeof(line)) && count < MAX_STUDENTS) {
        if (strlen(line) > 2) {
            parseLine(line, students[count]);
            count++;
        }
    }
    file.close();
    return count;
}

// Отпечатва студент по факултетен номер
void printStudent(const Student students[], int count, int fn) {
    for (int i = 0; i < count; i++) {
        if (students[i].fn == fn) {
            std::cout << "Name = "  << students[i].firstName
                      << " "        << students[i].lastName
                      << ", Email: "<< students[i].email
                      << ", FN: "   << students[i].fn << std::endl;
            return;
        }
    }
    std::cout << "Студент с FN " << fn << " не е намерен." << std::endl;
}

// Променя email по факултетен номер
void editEmail(Student students[], int count, int fn, const char* newEmail) {
    for (int i = 0; i < count; i++) {
        if (students[i].fn == fn) {
            strncpy(students[i].email, newEmail, MAX_STR - 1);
            return;
        }
    }
    std::cout << "Студент с FN " << fn << " не е намерен." << std::endl;
}

// Запазва студентите в CSV файл
void saveFile(const char* filename, const Student students[], int count) {
    std::ofstream file(filename);
    if (!file.is_open()) {
        std::cout << "Грешка при записване на файла!" << std::endl;
        return;
    }
    for (int i = 0; i < count; i++) {
        file << students[i].firstName << ","
             << students[i].lastName  << ","
             << students[i].email     << ","
             << students[i].fn        << "\n";
    }
    file.close();
    std::cout << "file " << filename << " successfully saved!" << std::endl;
}

int main() {
    Student students[MAX_STUDENTS];
    int count = 0;

    std::cout << "Open file: \n>";
    char filename[MAX_STR];
    std::cin.getline(filename, MAX_STR);

    count = loadFile(filename, students);
    if (count < 0) {
        std::cout << "Грешка: файлът не може да бъде отворен!" << std::endl;
        return 1;
    }
    std::cout << "File successfully opened!" << std::endl;

    char line[512];
    while (true) {
        std::cout << ">";
        if (!std::cin.getline(line, sizeof(line))) break;

        char cmd[32] = {};
        sscanf(line, "%31s", cmd);

        if (strcmp(cmd, "print") == 0) {
            int fn;
            sscanf(line, "%*s %d", &fn);
            printStudent(students, count, fn);

        } else if (strcmp(cmd, "edit") == 0) {
            int  fn;
            char newEmail[MAX_STR] = {};
            sscanf(line, "%*s %d %99s", &fn, newEmail);
            editEmail(students, count, fn, newEmail);

        } else if (strcmp(cmd, "save") == 0) {
            char outFile[MAX_STR] = {};
            sscanf(line, "%*s %99s", outFile);
            saveFile(outFile, students, count);

        } else if (strcmp(cmd, "exit") == 0) {
            break;
        } else {
            std::cout << "Непозната команда. Използвайте: print <fn> | edit <fn> <email> | save <file> | exit" << std::endl;
        }
    }
    return 0;
}
```

> 💡 Примерно `students.csv`:
> ```
> Ivan,Ivanov,ivan@gmail.com,70000
> Maria,Petrova,maria@abv.bg,70001
> ```
> Поддържани команди: `print <fn>`, `edit <fn> <email>`, `save <filename>`, `exit`.

</details>