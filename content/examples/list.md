---
date: '2025-03-02T12:13:44+01:00'
title: 'List'
---

Tento príklad ukazuje použitie **`std::list`** v C++17 na dynamickú manipuláciu s prvkami zoznamu, vkladanie, mazanie a
iteráciu.

```cpp
#include <iostream>
#include <list>

using namespace std;

int main() {
    // Deklarácia obojsmerne viazaného zoznamu (doubly linked list)
    list<int> myList = {10, 20, 30, 40};

    // Pridanie prvkov na začiatok a koniec zoznamu
    myList.push_front(5);
    myList.push_back(50);

    // Výpis prvkov zoznamu
    cout << "Obsah zoznamu: ";
    for (int num : myList) {
        cout << num << " ";
    }
    cout << endl;

    // Prístup k prvému a poslednému prvku
    cout << "Prvý prvok: " << myList.front() << endl;
    cout << "Posledný prvok: " << myList.back() << endl;

    // Odstránenie prvého a posledného prvku
    myList.pop_front();
    myList.pop_back();
    cout << "Po pop_front() a pop_back(): ";
    for (int num : myList) {
        cout << num << " ";
    }
    cout << endl;

    // Vloženie prvku na špecifickú pozíciu pomocou iterátora
    auto it = myList.begin();
    advance(it, 2);  // Posun iterátora na 3. pozíciu
    myList.insert(it, 25);
    cout << "Po vložení 25 na 3. pozíciu: ";
    for (int num : myList) {
        cout << num << " ";
    }
    cout << endl;

    // Odstránenie konkrétneho prvku
    myList.remove(20);
    cout << "Po odstránení 20: ";
    for (int num : myList) {
        cout << num << " ";
    }
    cout << endl;

    // Veľkosť zoznamu
    cout << "Veľkosť zoznamu: " << myList.size() << endl;

    return 0;
}
```

---

### **Vysvetlenie kódu:**

1. **Deklarácia `std::list<int>`**
   ```cpp
   list<int> myList = {10, 20, 30, 40};
   ```
    - `std::list` je **obojsmerne viazaný zoznam (doubly linked list)**.
    - Na rozdiel od `std::vector` umožňuje efektívne vkladanie a mazanie kdekoľvek.

2. **Pridanie prvkov na začiatok a koniec**
   ```cpp
   myList.push_front(5);
   myList.push_back(50);
   ```
    - `push_front(value)` pridá hodnotu **na začiatok**.
    - `push_back(value)` pridá hodnotu **na koniec**.

3. **Výpis prvkov zoznamu pomocou `range-based for loop`**
   ```cpp
   for (int num : myList) {
       cout << num << " ";
   }
   ```
    - Iterujeme cez celý zoznam a vypisujeme hodnoty.

4. **Prístup k prvému a poslednému prvku**
   ```cpp
   cout << "Prvý prvok: " << myList.front() << endl;
   cout << "Posledný prvok: " << myList.back() << endl;
   ```
    - `front()` vráti prvý prvok.
    - `back()` vráti posledný prvok.

5. **Odstránenie prvého a posledného prvku (`pop_front()` a `pop_back()`)**
   ```cpp
   myList.pop_front();
   myList.pop_back();
   ```
    - `pop_front()` odstráni prvý prvok.
    - `pop_back()` odstráni posledný prvok.

6. **Vloženie prvku na konkrétnu pozíciu (`insert()`)**
   ```cpp
   auto it = myList.begin();
   advance(it, 2);  // Posun iterátora na 3. pozíciu
   myList.insert(it, 25);
   ```
    - `insert(iterator, value)` vloží hodnotu **pred** daný iterátor.

7. **Odstránenie konkrétneho prvku (`remove()`)**
   ```cpp
   myList.remove(20);
   ```
    - `remove(value)` odstráni **všetky výskyty** hodnoty.

8. **Zistenie veľkosti (`size()`)**
   ```cpp
   cout << "Veľkosť zoznamu: " << myList.size() << endl;
   ```
    - `size()` vráti počet prvkov v zozname.

---

### **Očakávaný výstup:**

```
Obsah zoznamu: 5 10 20 30 40 50 
Prvý prvok: 5
Posledný prvok: 50
Po pop_front() a pop_back(): 10 20 30 40 
Po vložení 25 na 3. pozíciu: 10 20 25 30 40 
Po odstránení 20: 10 25 30 40 
Veľkosť zoznamu: 4
```
