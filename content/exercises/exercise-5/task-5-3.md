---
date: '2025-03-23T20:54:22+01:00'
title: 'Úloha 5.3'
weight: 3
---

Napíšte program, zdrojový kód, v jazyku C++ použitím štandardu C++17, ktorý realizuje nasledovnú činnosť.

Implementujte trieďací algoritmus **Quick sort** na STL kontajnery `list<string>`, t.j. zreťazený zoznam reťazcov.
Pre porovnanie prvkov zoznamu môžte použiť funkciu [
`std::strcmp()`](https://en.cppreference.com/w/cpp/string/byte/strcmp).
Funkciu pre zotriedenie zoznamu implementujte tak aby bolo možné zadať ako parameter či má algoritmus zotriediť prvky
vzostupne (prvé sú reťazce čo lexikograficky skôr), zostupne (prvé sú reťazce, ktoré sú lexikograficky neskôr).

> [!IMPORTANT]
> Dajte si pozor na to, že STL `list` nemá prísť k prvkov cez index (t.j. nie je `list.at()` alebo `list[0]`).
> Pre prechádzanie zoznamu použite iterátor (`list.begin()` a `list.end()`). Inštancií iterátora môžte mať viacero na
> jeden
> zoznam.

Viac o Quick sort algoritme sa viete dozvedieť napríklad na stránkach:

- [https://en.wikipedia.org/wiki/Quicksort](https://www.geeksforgeeks.org/quick-sort-algorithm/?ref=shm)
- [https://www.programiz.com/dsa/quick-sort](https://www.geeksforgeeks.org/quick-sort-algorithm/?ref=shm)
- [https://www.geeksforgeeks.org/quick-sort-algorithm/?ref=shm](https://www.geeksforgeeks.org/quick-sort-algorithm/?ref=shm)

### Príklady vstupov / výstupov programu

```cpp
["Milan", "Martin", "Eva"] -sort-> ["Eva","Martin","Milan"]

["Fero", "Jano", ""]       -sort-> ["", "Fero", "Jano"]
```

---

{{< details title="Rozbaľ pre ukážku riešenia" closed="true" >}}

```cpp
#include <iostream>
#include <list>
#include <string>
#include <cstring>    // pre std::strcmp
#include <iterator>   // pre std::advance

using namespace std;

/**
 * Quick Sort pre std::list<std::string>, podporuje vzostupné aj zostupné poradie.
 *
 * @param lst        Zreťazený zoznam reťazcov (bude zotriedený in-place).
 * @param ascending  True = vzostupne (lexikograficky menšie prvky prvé),
 *                   False = zostupne (lexikograficky väčšie prvé).
 */
void quickSort(list<string> &lst, bool ascending = true) {
    // Ak je 0 alebo 1 prvok, nič nerobíme (už je zotriedené).
    if (lst.size() <= 1) return;

    // 1) Vyberieme pivot: prvý prvok zoznamu
    string pivot = move(lst.front());
    lst.pop_front();

    // 2) Rozdelíme zvyšok zoznamu na dve časti: left (< pivot) a right (> pivot)
    list<string> left, right;
    auto cmp = [&](const string &a, const string &b) {
        int r = std::strcmp(a.c_str(), b.c_str());
        if (ascending)
            return r <= 0;    // a menšie alebo rovné b => do left
        else
            return r >= 0;    // a väčšie alebo rovné b => do left
    };

    // Pre každý zostávajúci prvok rozhodneme, či ide do left alebo right
    while (!lst.empty()) {
        auto it = lst.begin();
        if (cmp(*it, pivot))
            left.splice(left.end(), lst, it);
        else
            right.splice(right.end(), lst, it);
    }

    // 3) Rekurzívne zotriedime obidve časti
    quickSort(left,  ascending);
    quickSort(right, ascending);

    // 4) Zložíme späť do lst: najprv left, potom pivot, potom right
    lst.splice(lst.end(), left);
    lst.push_back(move(pivot));
    lst.splice(lst.end(), right);
}

/** Pomocná funkcia na výpis obsahu std::list<std::string> */
void printList(const list<string> &lst) {
    cout << "[ ";
    for (const auto &s : lst) {
        cout << '"' << s << "\" ";
    }
    cout << "]\n";
}

int main() {
    // Príklad 1: vzostupné zotriedenie
    list<string> l1 = { "Milan", "Martin", "Eva" };
    cout << "Povodny zoznam: ";
    printList(l1);
    quickSort(l1, true);
    cout << "Zoradene vzostupne: ";
    printList(l1);
    // Očakávaný výstup: ["Eva", "Martin", "Milan"]

    // Príklad 2: vzostupné s prázdnym reťazcom
    list<string> l2 = { "Fero", "Jano", "" };
    cout << "\nPovodny zoznam: ";
    printList(l2);
    quickSort(l2, true);
    cout << "Zoradene vzostupne: ";
    printList(l2);
    // Očakávaný výstup: ["", "Fero", "Jano"]

    // Príklad 3: zostupné zotriedenie
    list<string> l3 = { "Apple", "Banana", "Cherry" };
    cout << "\nPovodny zoznam: ";
    printList(l3);
    quickSort(l3, false);
    cout << "Zoradene zostupne: ";
    printList(l3);
    // Očakávaný výstup: ["Cherry", "Banana", "Apple"]

    return 0;
}
```

### Vysvetlenie riešenia

1. **Výber pivotu**  
   Zoberieme prvý prvok zoznamu (pivot) a odstránime ho z pôvodného zoznamu funkciou `pop_front()`.
2. **Partition**  
   Zvyšné uzly postupne presúvame do dvoch pomocných zoznamov `left` a `right` pomocou `splice`, podľa porovnania s
   pivotom cez `std::strcmp`. Lambda `cmp` zohľadňuje, či chceme vzostupné alebo zostupné poradie.
3. **Rekurzia**  
   Zavoláme `quickSort` na `left` a `right`, ktoré majú menší počet prvkov.
4. **Zlúčenie**  
   Po zotriedení oboch častí ich uzly (a samotný pivot) pomocou `splice` a `push_back` spojíme naspäť do pôvodného `lst`
   v správnom poradí.

Týmto spôsobom triedime `std::list<std::string>` efektívne bez potreby prístupu na prvky cez indexy.

{{< /details >}}
