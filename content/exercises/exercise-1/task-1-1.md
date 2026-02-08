---
date: '2025-02-13T18:18:11+01:00'
title: 'Úloha 1.1'
weight: 1
---

Napíšte program, zdrojový kód, v jazyku C++ použitím štandardu C++17, ktorý realizuje nasledovnú činnosť.

Program načíta celé číslo `n` zo štandardného vstupu a následne načíta `n` celých čísel. Vypočíta ich súčet a priemer (
zaokrúhlený nadol na celé číslo) a vypíše ich na štandardný výstup.

### Príklady vstupov / výstupov programu

#### Vstup

```text
6
5 10 20 30 40 50
```

#### Výstup

```text
Súčet: 155 
Priemer: 25.83
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
    std::cout << "Zadajte počet čísel: ";
    std::cin >> n;

    if (n <= 0) {
        std::cerr << "Počet čísel musí byť kladné celé číslo." << std::endl;
        return 1;
    }

    int* cisla = new int[n];
    std::cout << "Zadajte " << n << " celých čísel:" << std::endl;
    for (int i = 0; i < n; ++i) {
        std::cin >> cisla[i];
    }

    int sucet = 0;
    for (int i = 0; i < n; ++i) {
        sucet += cisla[i];
    }

    float priemer = (float) sucet / (float) n;

    std::cout << "Súčet: " << sucet << std::endl;
    std::cout << "Priemer: " << priemer << std::endl;

    delete[] cisla;
    return 0;
}
```

### Vysvetlenie

1. Program načíta počet čísel `n`. Ak je `n` neplatné (napríklad záporné alebo nulové), program vypíše chybové hlásenie a ukončí sa.
2. Dynamicky alokuje pole `cisla` veľkosti `n` pomocou operátora `new`.
3. V cykle načíta `n` celých čísel od používateľa a uloží ich do poľa.
4. V ďalšom cykle vypočíta súčet všetkých zadaných čísel.
5. Priemer sa vypočíta celočíselným delením súčtu počtom čísel (`sucet / n`), čo automaticky zaokrúhli výsledok nadol na najbližšie celé číslo.
6. Program vypíše súčet a priemer.
7. Na záver uvoľní dynamicky alokovanú pamäť pomocou `delete[]`.
-->
{{< /details >}}

