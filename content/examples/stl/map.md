---
date: '2025-03-02T12:13:52+01:00'
title: 'Map'
---

Tento príklad ukazuje použitie **`std::map`** v C++17 na ukladanie párov **kľúč → hodnota**, vyhľadávanie, mazanie a
iteráciu cez mapu.

```cpp
#include <iostream>
#include <map>
#include <string>

using namespace std;

int main() {
    // Deklarácia mapy, kde kľúč je string a hodnota je int
    map<string, int> myMap;

    // Vkladanie hodnôt pomocou operátora []
    myMap["Alice"] = 25;
    myMap["Bob"] = 30;
    myMap["Charlie"] = 35;

    // Alternatívne vkladanie pomocou insert()
    myMap.insert({"David", 40});
    myMap.insert(pair<string, int>("Eve", 28));

    // Výpis obsahu mapy (zoradené podľa kľúča)
    cout << "Obsah mapy:" << endl;
    for (const auto &entry : myMap) {
        cout << entry.first << " -> " << entry.second << endl;
    }

    // Vyhľadanie prvku v mape
    string name = "Charlie";
    if (myMap.find(name) != myMap.end()) {
        cout << name << " sa nachádza v mape s hodnotou: " << myMap[name] << endl;
    } else {
        cout << name << " sa nenachádza v mape." << endl;
    }

    // Odstránenie prvku podľa kľúča
    myMap.erase("Bob");
    cout << "Po odstránení Boba:" << endl;
    for (const auto &entry : myMap) {
        cout << entry.first << " -> " << entry.second << endl;
    }

    // Veľkosť mapy
    cout << "Veľkosť mapy: " << myMap.size() << endl;

    return 0;
}
```

---

### **Vysvetlenie kódu:**

1. **Deklarácia `std::map<string, int>`**
   ```cpp
   map<string, int> myMap;
   ```
    - `std::map` uchováva páry **kľúč → hodnota**.
    - Kľúče sú **unikátne** a automaticky **zoradené**.

2. **Vkladanie hodnôt:**
    - Pomocou operátora `[]`:
      ```cpp
      myMap["Alice"] = 25;
      myMap["Bob"] = 30;
      myMap["Charlie"] = 35;
      ```
    - Pomocou `insert()`:
      ```cpp
      myMap.insert({"David", 40});
      myMap.insert(pair<string, int>("Eve", 28));
      ```

3. **Výpis obsahu mapy**
   ```cpp
   for (const auto &entry : myMap) {
       cout << entry.first << " -> " << entry.second << endl;
   }
   ```
    - `entry.first` je kľúč.
    - `entry.second` je hodnota.
    - `std::map` uchováva údaje v **zoradenom poradí** podľa kľúča.

4. **Vyhľadanie prvku (`find()`)**
   ```cpp
   if (myMap.find(name) != myMap.end()) {
       cout << name << " sa nachádza v mape s hodnotou: " << myMap[name] << endl;
   }
   ```
    - `find(kľúč)` vráti iterátor na prvok alebo `end()`, ak neexistuje.

5. **Odstránenie prvku (`erase()`)**
   ```cpp
   myMap.erase("Bob");
   ```
    - `erase(kľúč)` odstráni prvok podľa kľúča.

6. **Zistenie veľkosti (`size()`)**
   ```cpp
   cout << "Veľkosť mapy: " << myMap.size() << endl;
   ```
    - `size()` vráti počet prvkov v mape.

---

### **Očakávaný výstup:**

```
Obsah mapy:
Alice -> 25
Bob -> 30
Charlie -> 35
David -> 40
Eve -> 28
Charlie sa nachádza v mape s hodnotou: 35
Po odstránení Boba:
Alice -> 25
Charlie -> 35
David -> 40
Eve -> 28
Veľkosť mapy: 4
```
