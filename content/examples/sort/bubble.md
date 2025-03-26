---
date: '2025-03-24T11:21:57+01:00'
title: 'Bubble Sort'
---

**Bubble Sort** je jednoduchý triediaci algoritmus, ktorý opakovane prechádza zoznamom, porovnáva susedné prvky a prehadzuje ich, ak sú v nesprávnom poradí.

#### Princíp:
- V každom prechode „vybublá“ najväčší (alebo najmenší) prvok na koniec (alebo začiatok).
- Opakuje sa, kým zoznam nie je zoradený.

#### Príklad (vzostupne):
Zoznam: `[5, 3, 8]`
1. porovnám `5 > 3` → prehodím → `[3, 5, 8]`
2. porovnám `5 > 8` → nič  
   Hotovo – zoznam je zoradený.

#### Výhody:
- Jednoduchý na implementáciu

#### Nevýhody:
- Pomalý pri veľkých dátach: **O(n²)** časová zložitosť
- Neefektívny v porovnaní s inými algoritmami ako QuickSort, MergeSort

## Implementácia

```cpp
#include <iostream>
#include <vector>

using namespace std;

bool compare(int a, int b, int direction) {
    return direction > 0 ? a > b : a < b;
}

void bubble_sort(vector<int>& values, int direction) {
    size_t size = values.size();

    for (int i = 0; i < size - 1; i++) {
        for (int j = 0; j < size - i - 1; j++) {
            if (compare(values[j], values[j + 1], direction)) {
                int tmp = values[j];
                values[j] = values[j + 1];
                values[j + 1] = tmp;
            }
        }
    }
}

void print(vector<int>& values) {
    for (int value: values) {
        cout << value << " ";
    }
    cout << endl;
}

int main() {
    vector<int> values = {3, 8, 5, 66, -7};
    print(values);
    bubble_sort(values, 1);
    print(values);
    return 0;
}
```

## Vysvetlenie kódu

Kód implementuje **Bubble Sort** algoritmus s možnosťou triedenia **vzostupne alebo zostupne** podľa parametra `direction`.

#### 🔹 Základné hlavičky a `using namespace`

```cpp
#include <iostream>
#include <vector>
using namespace std;
```

- `#include <iostream>` – umožňuje vstup a výstup (napr. `cout`).
- `#include <vector>` – na použitie triedy `std::vector`, dynamické pole.
- `using namespace std;` – aby sme nemuseli písať `std::` pred každým `cout`, `vector`, atď.

#### 🔹 Funkcia `compare`

```cpp
bool compare(int a, int b, int direction) {
    return direction > 0 ? a > b : a < b;
}
```

- Porovnáva dve hodnoty `a` a `b` podľa hodnoty `direction`:
    - Ak `direction > 0` → triedenie **vzostupne** (`a > b`)
    - Ak `direction <= 0` → triedenie **zostupne** (`a < b`)

#### 🔹 Funkcia `bubble_sort`

```cpp
void bubble_sort(vector<int>& values, int direction) {
    size_t size = values.size();

    for (int i = 0; i < size - 1; i++) {
        for (int j = 0; j < size - i - 1; j++) {
            if (compare(values[j], values[j + 1], direction)) {
                int tmp = values[j];
                values[j] = values[j + 1];
                values[j + 1] = tmp;
            }
        }
    }
}
```

- Implementuje **Bubble Sort**:
    - Porovnáva susedné prvky a prehadzuje ich podľa porovnania z `compare`.
    - Opakuje to, kým najväčšie (alebo najmenšie) hodnoty „nevyplávajú“ na koniec poľa.
- Zložitosť: **O(n²)**

#### 🔹 Funkcia `print`

```cpp
void print(vector<int>& values) {
    for (int value: values) {
        cout << value << " ";
    }
    cout << endl;
}
```

- Vypíše všetky prvky vektora na štandardný výstup.
