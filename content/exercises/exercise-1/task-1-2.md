---
date: '2025-02-13T19:11:40+01:00'
title: 'Úloha 1.2'
weight: 2
---

Napíšte program, zdrojový kód, v jazyku C++ použitím štandardu C++17, ktorý realizuje nasledovnú činnosť.

Program načíta celé číslo `n`, ktoré predstavuje počet prvkov v poli. Následne alokuje dynamické pole veľkosti `n`,
načíta `n` celých čísel a následne vypíše všetky párne čísla v poradí, v akom boli zadané.

### Príklady vstupov / výstupov programu

#### Vstup

```text
7
6 1 2 3 4 5 6
```

#### Výstup

```text
2 4 6
```

---

{{< details title="Rozbaľ pre ukážku riešenia" closed="true" >}}
Musím si počkať kým sa tu objaví príklad riešenia.

Nezabudni, že najviac sa naučíš ak to vypracuješ sám. 😉

<!--
```cpp
#include <iostream>

int main() {
    int n;
    std::cout << "Zadajte počet prvkov: ";
    std::cin >> n;

    if (n <= 0) {
        std::cerr << "Počet prvkov musí byť kladné celé číslo." << std::endl;
        return 1;
    }

    int* pole = new int[n];
    std::cout << "Zadajte " << n << " celých čísel:" << std::endl;
    for (int i = 0; i < n; ++i) {
        std::cin >> pole[i];
    }

    std::cout << "Párne čísla: ";
    for (int i = 0; i < n; ++i) {
        if (pole[i] % 2 == 0) {
            std::cout << pole[i] << " ";
        }
    }
    std::cout << std::endl;

    delete[] pole;
    return 0;
}
```

### Vysvetlenie

1. Načítanie počtu prvkov: Program najprv načíta celé číslo `n`, ktoré určuje počet prvkov v poli. Ak je `n` neplatné (napríklad záporné alebo nulové), program vypíše chybové hlásenie a ukončí sa.
2. Dynamická alokácia poľa: Pomocou operátora `new` program alokuje pole veľkosti `n` pre celé čísla.
3. Načítanie prvkov: V cykle `for` program načíta `n` celých čísel od používateľa a uloží ich do poľa.
4. Výpis párnych čísel: Program prechádza pole a pomocou operátora modulo (`%`) kontroluje, či je dané číslo párne. Ak áno, vypíše ho na štandardný výstup.
5. Uvoľnenie pamäte: Na záver program uvoľní dynamicky alokovanú pamäť pomocou operátora `delete[]`.
-->
{{< /details >}}
