---
date: '2025-02-19T18:27:45+01:00'
title: 'Základy polí'
---

```cpp
#include <iostream>

using namespace std;

int main() {

    int *pole = new int[item_size];
    for (int i = 0; i < item_size; i++) {
        pole[i] = i + 1;
    }
    for (int i = 0; i < item_size; i++) {
        cout << pole[i] << ", ";
    }

    delete[] pole;

    return 0;
}
```

Tento kód v jazyku C++ demonštruje dynamickú alokáciu pamäte pre pole celých čísel, jeho inicializáciu, výpis hodnôt a
následné uvoľnenie pamäte. Nižšie je podrobný rozbor kódu s vysvetlením jednotlivých konceptov a odkazmi na ďalšie
zdroje:

```cpp
#include <iostream>
```

**Inklúzia knižnice**: Tento riadok zahrňuje štandardnú knižnicu `iostream`, ktorá umožňuje vstup a výstup dát pomocou
objektov `cin` a `cout`.

```cpp
using namespace std;
```

**Deklarácia menného priestoru**: Umožňuje používať členy z menného priestoru `std` bez ich plnej kvalifikácie.
Napríklad namiesto `std::cout` stačí písať `cout`.

```cpp
int main() {
```

**Hlavná funkcia programu**: Funkcia `main` je vstupným bodom každého programu v jazyku C++.

```cpp
    int *pole = new int[item_size];
```

**Dynamická alokácia poľa**: Tento riadok alokuje pamäť pre pole celých čísel s veľkosťou `item_size` pomocou operátora
`new` a vrátený ukazovateľ ukladá do premennej `pole`.

```cpp
    for (int i = 0; i < item_size; ++i) {
        pole[i] = i + 1;
    }
```

**Inicializácia poľa**: Tento cyklus prechádza všetky indexy poľa od 0 po `item_size - 1` a priraďuje im hodnoty od 1 po
`item_size`.

```cpp
    for (int i = 0; i < item_size; ++i) {
        cout << pole[i] << ", ";
    }
```

**Výpis hodnôt poľa**: Tento cyklus prechádza všetky prvky poľa a vypisuje ich hodnoty na štandardný výstup, pričom
jednotlivé hodnoty sú oddelené čiarkou.

```cpp
    delete[] pole;
```

**Dealokácia pamäte**: Tento riadok uvoľňuje pamäť, ktorá bola predtým alokovaná pre pole pomocou operátora `new`. Je
dôležité uvoľňovať dynamicky alokovanú pamäť, aby sa predišlo únikom pamäte.

```cpp
    return 0;
}
```

**Návratová hodnota funkcie `main`**: Funkcia `main` vracia hodnotu 0, čo indikuje, že program skončil úspešne.

**Poznámka**: V kóde sa používa premenná `item_size`, ktorá však nie je definovaná v rámci tohto programu. Pred použitím
tejto premennej je potrebné ju definovať a priradiť jej hodnotu, napríklad:

```cpp
const int item_size = 5;
```

Bez tejto definície by program nebol funkčný a prekladač by hlásil chybu. 
