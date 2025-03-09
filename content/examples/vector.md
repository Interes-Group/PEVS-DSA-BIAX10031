---
date: '2025-03-02T12:13:41+01:00'
title: 'Vector'
---

Tento príklad ukazuje rôzne metódy práce s `std::vector<std::string>` v C++17.

```cpp
#include <iostream>
#include <vector>
#include <string>

using namespace std;

int main() {
    // Deklarácia a inicializácia vektora reťazcov
    vector<string> words = {"C++", "je", "skvelý", "jazyk"};

    // Pridanie nového prvku na koniec vektora
    words.push_back("!");

    // Výpis obsahu vektora pomocou range-based for loop (C++11+)
    cout << "Obsah vektora: ";
    for (const string &word : words) {
        cout << word << " ";
    }
    cout << endl;

    // Prístup k prvkom pomocou indexovania
    cout << "Prvý prvok: " << words[0] << endl;
    cout << "Posledný prvok: " << words.back() << endl;

    // Použitie iterátorov na výpis obsahu vektora
    cout << "Výpis cez iterátory: ";
    for (auto it = words.begin(); it != words.end(); ++it) {
        cout << *it << " ";
    }
    cout << endl;

    // Vymazanie posledného prvku
    words.pop_back();

    // Výpis veľkosti a kapacity
    cout << "Veľkosť vektora: " << words.size() << endl;
    cout << "Kapacita vektora: " << words.capacity() << endl;

    // Použitie metódy emplace_back() na efektívne pridanie prvku
    words.emplace_back("moderný");

    // Výpis upraveného vektora
    cout << "Upravený vektor: ";
    for (const auto &word : words) {
        cout << word << " ";
    }
    cout << endl;

    return 0;
}
```

---

### **Vysvetlenie kódu:**
1. **Deklarácia a inicializácia vektora**
   ```cpp
   vector<string> words = {"C++", "je", "skvelý", "jazyk"};
   ```
    - `std::vector<std::string>` je dynamické pole reťazcov.
    - Inicializujeme ho štyrmi slovami.

2. **Pridanie prvku na koniec (`push_back()`)**
   ```cpp
   words.push_back("!");
   ```
    - Pridá reťazec `"!"` na koniec vektora.

3. **Výpis obsahu pomocou `range-based for loop`**
   ```cpp
   for (const string &word : words) {
       cout << word << " ";
   }
   ```
    - `const string &word` zabraňuje zbytočnej kopírovaniu.

4. **Prístup k prvkom cez indexy**
   ```cpp
   cout << "Prvý prvok: " << words[0] << endl;
   cout << "Posledný prvok: " << words.back() << endl;
   ```
    - `words[0]` vráti prvý prvok.
    - `words.back()` vráti posledný prvok.

5. **Použitie iterátorov na výpis**
   ```cpp
   for (auto it = words.begin(); it != words.end(); ++it) {
       cout << *it << " ";
   }
   ```
    - `begin()` vráti iterátor na prvý prvok.
    - `end()` vráti iterátor za posledným prvkom.

6. **Vymazanie posledného prvku (`pop_back()`)**
   ```cpp
   words.pop_back();
   ```
    - Odstráni posledný prvok z vektora.

7. **Výpis veľkosti a kapacity**
   ```cpp
   cout << "Veľkosť vektora: " << words.size() << endl;
   cout << "Kapacita vektora: " << words.capacity() << endl;
   ```
    - `size()` vráti aktuálny počet prvkov.
    - `capacity()` vráti počet prvkov, ktoré môže vektor uložiť bez alokácie.

8. **Použitie `emplace_back()`**
   ```cpp
   words.emplace_back("moderný");
   ```
    - `emplace_back()` je efektívnejšia verzia `push_back()`, pretože konštruuje objekt priamo v pamäti vektora.

---

### **Očakávaný výstup:**
```
Obsah vektora: C++ je skvelý jazyk ! 
Prvý prvok: C++
Posledný prvok: !
Výpis cez iterátory: C++ je skvelý jazyk ! 
Veľkosť vektora: 4
Kapacita vektora: 8
Upravený vektor: C++ je skvelý jazyk moderný 
```
