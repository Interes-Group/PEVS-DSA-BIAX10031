---
date: '2025-02-13T19:11:42+01:00'
title: 'Úloha 1.3'
weight: 3
---

Napíšte program, zdrojový kód, v jazyku C++ použitím štandardu C++17, ktorý realizuje nasledovnú činnosť.

Program načíta celé číslo `n` zo štandardného vstupu, ktoré predstavuje počet riadkov a stĺpcov štvorcovej matice.
Následne alokuje dynamickú dvojrozmernú maticu veľkosti `n x n` a načíta do nej celé čísla po riadkoch (t.j. načíta `n`
čísel prvého riadku, potom ďalší atď.). Program potom transponuje túto maticu (vymení riadky za stĺpce) a vypíše
transponovanú maticu na štandardný výstup.

### Príklady vstupov / výstupov programu

#### Vstup

```text
3
3 1 2
3 4 5
6 7 8
```

#### Výstup

```text
3 3 6
1 4 7
2 5 8
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
    std::cout << "Zadajte počet riadkov a stĺpcov matice (n): ";
    std::cin >> n;

    if (n <= 0) {
        std::cerr << "Počet riadkov a stĺpcov musí byť kladné celé číslo." << std::endl;
        return 1;
    }

    // Dynamická alokácia dvojrozmerného poľa
    int** matica = new int*[n];
    for (int i = 0; i < n; ++i) {
        matica[i] = new int[n];
    }

    std::cout << "Zadajte prvky matice po riadkoch:" << std::endl;
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < n; ++j) {
            std::cin >> matica[i][j];
        }
    }

    // Transponovanie matice
    for (int i = 0; i < n; ++i) {
        for (int j = i + 1; j < n; ++j) {
            std::swap(matica[i][j], matica[j][i]);
        }
    }

    std::cout << "Transponovaná matica:" << std::endl;
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < n; ++j) {
            std::cout << matica[i][j] << " ";
        }
        std::cout << std::endl;
    }

    // Uvoľnenie dynamicky alokovanej pamäte
    for (int i = 0; i < n; ++i) {
        delete[] matica[i];
    }
    delete[] matica;

    return 0;
}
```

### Vysvetlenie

1. Načítanie veľkosti matice: Program načíta celé číslo `n`, ktoré určuje počet riadkov a stĺpcov štvorcovej matice. Ak je `n` neplatné (napríklad záporné alebo nulové), program vypíše chybové hlásenie a ukončí sa.
2. Dynamická alokácia dvojrozmerného poľa: Program alokuje pole ukazovateľov `int** matica`, kde každý ukazovateľ smeruje na jednorozmerné pole celých čísel s veľkosťou `n`. Týmto spôsobom vznikne dvojrozmerné pole reprezentujúce maticu.
3. Načítanie prvkov matice: Program načíta `n x n` celých čísel od používateľa a uloží ich do matice po riadkoch.
4. Transponovanie matice: Program prechádza hornú trojuholníkovú časť matice (nad hlavnou diagonálou) a pre každý prvok `matica[i][j]` vymení jeho hodnotu s prvkom `matica[j][i]`. Tým sa dosiahne transpozícia matice.
5. Výpis transponovanej matice: Program vypíše transponovanú maticu na štandardný výstup.
6. Uvoľnenie dynamicky alokovanej pamäte: Program uvoľní pamäť alokovanú pre každý riadok matice pomocou `delete[]` a následne uvoľní aj pole ukazovateľov.
-->
{{< /details >}}
