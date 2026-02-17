---
date: '2025-02-13T19:11:52+01:00'
title: '✨ Úloha 1.6'
weight: 6
---

Napíšte program v jazyku C++ použitím štandardu C++17, ktorý realizuje nasledovnú činnosť.

Program načíta celé číslo `n` zo štandardného vstupu, ktoré predstavuje počet prvkov v poli. Následne alokuje dynamické
pole veľkosti `n` a načíta do neho `n` celých čísel. Program potom nájde najdlhšiu postupnosť po sebe idúcich prvkov,
ktoré sú v neklesajúcom poradí (každý nasledujúci prvok je rovnaký alebo väčší ako predchádzajúci), a vypíše dĺžku tejto
postupnosti.

### Príklady vstupov / výstupov programu

#### Vstup

```text
11
10 5 3 4 4 5 6 2 2 2 3
```

#### Výstup

```text
Najdlhšia neklesajúca postupnosť má dĺžku: 4
```

---

{{< details title="Rozbaľ pre ukážku riešenia" closed="true" >}}

```cpp
#include <iostream>

int main() {
    int n;
    std::cout << "Zadajte počet prvkov v poli: ";
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

    int max_dlzka = 1;
    int aktualna_dlzka = 1;

    for (int i = 1; i < n; ++i) {
        if (pole[i] >= pole[i - 1]) {
            ++aktualna_dlzka;
        } else {
            if (aktualna_dlzka > max_dlzka) {
                max_dlzka = aktualna_dlzka;
            }
            aktualna_dlzka = 1;
        }
    }

    if (aktualna_dlzka > max_dlzka) {
        max_dlzka = aktualna_dlzka;
    }

    std::cout << "Najdlhšia neklesajúca postupnosť má dĺžku: " << max_dlzka << std::endl;

    delete[] pole;
    return 0;
}
```

### Vysvetlenie

1. Načítanie počtu prvkov: Program načíta celé číslo `n`, ktoré určuje počet prvkov v poli. Ak je `n` neplatné (
   napríklad
   záporné alebo nulové), program vypíše chybové hlásenie a ukončí sa.
2. Dynamická alokácia poľa: Pomocou operátora `new` program alokuje pole veľkosti `n` pre celé čísla.
3. Načítanie prvkov: V cykle `for` program načíta `n` celých čísel od používateľa a uloží ich do poľa.
4. Nájdenie najdlhšej neklesajúcej postupnosti:
    - Program inicializuje premenné `max_dlzka` a `aktualna_dlzka` na 1.
    - V cykle prechádza pole od druhého prvku po posledný.
    - Ak je aktuálny prvok väčší alebo rovný predchádzajúcemu, zvýši `aktualna_dlzka` o 1.
    - Ak nie, porovná `aktualna_dlzka` s `max_dlzka`. Ak je `aktualna_dlzka` väčšia, aktualizuje `max_dlzka`. Potom
      resetuje
      `aktualna_dlzka` na 1.
    - Po ukončení cyklu ešte raz porovná `aktualna_dlzka` s `max_dlzka` pre prípad, že najdlhšia neklesajúca postupnosť
      bola
      na konci poľa.
5. Výpis výsledku: Program vypíše dĺžku najdlhšej neklesajúcej postupnosti.
6. Uvoľnenie pamäte: Na záver program uvoľní dynamicky alokovanú pamäť pomocou operátora `delete[]`.

{{< /details >}}
