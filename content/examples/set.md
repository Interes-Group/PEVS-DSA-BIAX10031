---
date: '2025-03-02T12:13:49+01:00'
title: 'Set'
---

Tento príklad ukazuje použitie **`std::set`** v C++17 na ukladanie unikátnych hodnôt, vyhľadávanie, mazanie a prácu s
množinami.

```cpp
#include <iostream>
#include <set>

using namespace std;

int main() {
    // Deklarácia a inicializácia množiny celých čísel
    set<int> mySet = {5, 2, 8, 3, 5, 10, 2};

    // Výpis prvkov množiny (automaticky zoradené a bez duplikátov)
    cout << "Obsah množiny: ";
    for (int num : mySet) {
        cout << num << " ";
    }
    cout << endl;

    // Pridanie nového prvku
    mySet.insert(7);
    cout << "Po pridaní 7: ";
    for (int num : mySet) {
        cout << num << " ";
    }
    cout << endl;

    // Kontrola existencie prvku
    int value = 3;
    if (mySet.find(value) != mySet.end()) {
        cout << value << " sa nachádza v množine." << endl;
    } else {
        cout << value << " sa nenachádza v množine." << endl;
    }

    // Odstránenie prvku
    mySet.erase(8);
    cout << "Po odstránení 8: ";
    for (int num : mySet) {
        cout << num << " ";
    }
    cout << endl;

    // Veľkosť množiny
    cout << "Veľkosť množiny: " << mySet.size() << endl;

    return 0;
}
```

---

### **Vysvetlenie kódu:**

1. **Deklarácia a inicializácia `std::set`**
   ```cpp
   set<int> mySet = {5, 2, 8, 3, 5, 10, 2};
   ```
    - `std::set` uchováva **unikátne hodnoty** a automaticky ich **triedi**.
    - Duplicity sa ignorujú, takže `"5"` a `"2"` sa pridajú iba raz.

2. **Výpis prvkov množiny**
   ```cpp
   for (int num : mySet) {
       cout << num << " ";
   }
   ```
    - `std::set` sa vypisuje v **triedenom poradí**.

3. **Pridanie prvku (`insert()`)**
   ```cpp
   mySet.insert(7);
   ```
    - `insert()` pridá nový prvok.
    - Ak už existuje, nič sa nezmení.

4. **Vyhľadanie prvku (`find()`)**
   ```cpp
   if (mySet.find(value) != mySet.end()) { ... }
   ```
    - `find(value)` vráti **iterátor** na prvok alebo `end()`, ak sa nenachádza v množine.

5. **Odstránenie prvku (`erase()`)**
   ```cpp
   mySet.erase(8);
   ```
    - `erase()` odstráni prvok zo množiny.

6. **Zistenie veľkosti (`size()`)**
   ```cpp
   cout << "Veľkosť množiny: " << mySet.size() << endl;
   ```
    - `size()` vráti počet unikátnych prvkov.

---

### **Očakávaný výstup:**

```
Obsah množiny: 2 3 5 8 10 
Po pridaní 7: 2 3 5 7 8 10 
3 sa nachádza v množine.
Po odstránení 8: 2 3 5 7 10 
Veľkosť množiny: 5
```
