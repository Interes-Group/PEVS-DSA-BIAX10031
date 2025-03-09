---
date: '2025-03-02T12:47:41+01:00'
title: 'Stack'
---

Tento príklad ukazuje použitie **`std::stack`** v C++17 na pridávanie, prístup, odoberanie prvkov a prácu so zásobníkom.

```cpp
#include <iostream>
#include <stack>
#include <string>

using namespace std;

int main() {
    // Deklarácia zásobníka pre reťazce
    stack<string> myStack;

    // Pridávanie prvkov na vrchol zásobníka
    myStack.push("C++");
    myStack.push("je");
    myStack.push("výborný");
    myStack.push("jazyk");

    // Výpis vrchného prvku (top)
    cout << "Vrchný prvok: " << myStack.top() << endl;

    // Odstránenie vrchného prvku
    myStack.pop();
    cout << "Po pop(): Vrchný prvok je teraz: " << myStack.top() << endl;

    // Pridanie nového prvku pomocou emplace()
    myStack.emplace("moderný");
    cout << "Po emplace(): Vrchný prvok je teraz: " << myStack.top() << endl;

    // Výpis všetkých prvkov a ich odstránenie
    cout << "Obsah zásobníka: ";
    while (!myStack.empty()) {
        cout << myStack.top() << " ";
        myStack.pop();
    }
    cout << endl;

    return 0;
}
```

---

### **Vysvetlenie kódu:**

1. **Deklarácia `std::stack<string>`**
   ```cpp
   stack<string> myStack;
   ```
    - `std::stack` je LIFO (Last-In, First-Out) dátová štruktúra.
    - Používame ju na uchovanie reťazcov (`std::string`).

2. **Pridanie prvkov na vrchol (`push()`)**
   ```cpp
   myStack.push("C++");
   myStack.push("je");
   myStack.push("výborný");
   myStack.push("jazyk");
   ```
    - Prvky sa pridávajú na **vrchol** zásobníka.

3. **Získanie vrchného prvku (`top()`)**
   ```cpp
   cout << "Vrchný prvok: " << myStack.top() << endl;
   ```
    - `top()` vráti referenciu na **posledne pridaný** prvok bez jeho odstránenia.

4. **Odstránenie vrchného prvku (`pop()`)**
   ```cpp
   myStack.pop();
   cout << "Po pop(): Vrchný prvok je teraz: " << myStack.top() << endl;
   ```
    - `pop()` odstráni najnovší prvok.

5. **Efektívne pridanie nového prvku (`emplace()`)**
   ```cpp
   myStack.emplace("moderný");
   ```
    - `emplace()` je efektívnejšia verzia `push()`, pretože **konštruuje prvok priamo v zásobníku**.

6. **Výpis a vyprázdnenie zásobníka**
   ```cpp
   while (!myStack.empty()) {
       cout << myStack.top() << " ";
       myStack.pop();
   }
   ```
    - Cez `empty()` kontrolujeme, či je zásobník prázdny.
    - Každý prvok vypíšeme a hneď ho odstránime (`pop()`).

---

### **Očakávaný výstup:**

```
Vrchný prvok: jazyk
Po pop(): Vrchný prvok je teraz: výborný
Po emplace(): Vrchný prvok je teraz: moderný
Obsah zásobníka: moderný výborný je C++ 
```
