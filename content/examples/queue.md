---
date: '2025-03-02T12:47:46+01:00'
title: 'Queue'
---

Tento príklad ukazuje použitie **`std::queue`** v C++17 na pridávanie, prístup, odoberanie prvkov a prácu s frontou.

```cpp
#include <iostream>
#include <queue>

using namespace std;

int main() {
    // Deklarácia fronty (FIFO) pre celé čísla
    queue<int> myQueue;

    // Pridávanie prvkov do fronty (enqueue)
    myQueue.push(10);
    myQueue.push(20);
    myQueue.push(30);
    myQueue.push(40);

    // Výpis predného a zadného prvku
    cout << "Predný prvok: " << myQueue.front() << endl;
    cout << "Zadný prvok: " << myQueue.back() << endl;

    // Odstránenie predného prvku (dequeue)
    myQueue.pop();
    cout << "Po pop(): Predný prvok je teraz: " << myQueue.front() << endl;

    // Výpis obsahu fronty
    cout << "Obsah fronty: ";
    while (!myQueue.empty()) {
        cout << myQueue.front() << " ";
        myQueue.pop();
    }
    cout << endl;

    return 0;
}
```

---

### **Vysvetlenie kódu:**

1. **Deklarácia `std::queue<int>`**
   ```cpp
   queue<int> myQueue;
   ```
    - `std::queue` je **FIFO (First-In, First-Out)** dátová štruktúra.
    - Používame ju na uchovanie celých čísel.

2. **Pridávanie prvkov (`push()`)**
   ```cpp
   myQueue.push(10);
   myQueue.push(20);
   myQueue.push(30);
   myQueue.push(40);
   ```
    - `push(value)` pridá prvok **na koniec** fronty.

3. **Získanie predného a zadného prvku**
   ```cpp
   cout << "Predný prvok: " << myQueue.front() << endl;
   cout << "Zadný prvok: " << myQueue.back() << endl;
   ```
    - `front()` vráti prvý prvok (ktorý bol pridaný ako prvý).
    - `back()` vráti posledný prvok (ktorý bol pridaný ako posledný).

4. **Odstránenie predného prvku (`pop()`)**
   ```cpp
   myQueue.pop();
   ```
    - `pop()` odstráni **prvý** prvok fronty.

5. **Výpis a vyprázdnenie fronty**
   ```cpp
   while (!myQueue.empty()) {
       cout << myQueue.front() << " ";
       myQueue.pop();
   }
   ```
    - `empty()` skontroluje, či je fronta prázdna.
    - Každý prvok vypíšeme a odstránime (`pop()`).

---

### **Očakávaný výstup:**

```
Predný prvok: 10
Zadný prvok: 40
Po pop(): Predný prvok je teraz: 20
Obsah fronty: 20 30 40 
```
