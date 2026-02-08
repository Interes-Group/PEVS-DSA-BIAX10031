---
date: '2025-03-23T20:54:19+01:00'
title: 'Úloha 5.2'
weight: 2
---

Napíšte program, zdrojový kód, v jazyku C++ použitím štandardu C++17, ktorý realizuje nasledovnú činnosť.

Implementujte trieďací algoritmus **Merge sort** na STL kontajnery `list<string>`, t.j. zreťazený zoznam reťazcov.
Pre porovnanie prvkov zoznamu môžte použiť funkciu [
`std::strcmp()`](https://en.cppreference.com/w/cpp/string/byte/strcmp).
Funkciu pre zotriedenie zoznamu implementujte tak aby bolo možné zadať ako parameter či má algoritmus zotriediť prvky
vzostupne (prvé sú reťazce čo lexikograficky skôr), zostupne (prvé sú reťazce, ktoré sú lexikograficky neskôr).

> [!IMPORTANT]
> Dajte si pozor na to, že STL `list` nemá prísť k prvkov cez index (t.j. nie je `list.at()` alebo `list[0]`).
> Pre prechádzanie zoznamu použite iterátor (`list.begin()` a `list.end()`). Inštancií iterátora môžte mať viacero na
> jeden
> zoznam.

Viac o Merge sort algoritme sa viete dozvedieť napríklad na stránkach:

- [https://en.wikipedia.org/wiki/Merge_sort](https://www.geeksforgeeks.org/merge-sort/?ref=shm)
- [https://www.programiz.com/dsa/merge-sort](https://www.geeksforgeeks.org/merge-sort/?ref=shm)
- [https://www.geeksforgeeks.org/merge-sort/?ref=shm](https://www.geeksforgeeks.org/merge-sort/?ref=shm)

### Príklady vstupov / výstupov programu

```cpp
["Milan", "Martin", "Eva"] -sort-> ["Eva","Martin","Milan"]

["Fero", "Jano", ""]       -sort-> ["", "Fero", "Jano"]
```

---

{{< details title="Rozbaľ pre ukážku riešenia" closed="true" >}}

Musím si počkať kým sa tu objaví príklad riešenia.

Nezabudni, že najviac sa naučíš ak to vypracuješ sám. 😉

<!--


```cpp
#include <iostream>
#include <list>
#include <string>
#include <cstring>    // pre std::strcmp

using namespace std;

/**
 * Zoradí zreťazený zoznam reťazcov pomocou algoritmu Merge Sort.
 *
 * @param lst        Zreťazený zoznam (std::list<std::string>&).
 *                   Po volaní bude obsahovať zotriedené prvky.
 * @param ascending  True = vzostupne (lexikograficky od "a" po "z"),
 *                   False = zostupne.
 */
void mergeSort(list<string> &lst, bool ascending = true) {
    // Základný prípad: triválne zotriedenie pre 0 alebo 1 prvok
    if (lst.size() <= 1) return;

    // 1) Rozdelíme zoznam na dve polovice
    list<string> left, right;
    auto it = lst.begin();
    std::advance(it, lst.size() / 2);  // posunieme iterátor na stred
    // prvú polovicu [begin, it) presunieme do left
    left.splice(left.begin(), lst, lst.begin(), it);
    // zostávajúcu druhú polovicu presunieme do right
    right.splice(right.begin(), lst, lst.begin(), lst.end());
    // lst je teraz prázdny

    // 2) Rekurzívne zotriedime obe polovice
    mergeSort(left, ascending);
    mergeSort(right, ascending);

    // 3) Zlúčime obidva zotriedené zoznamy späť do lst
    auto cmp = [&](const string &a, const string &b) {
        int r = std::strcmp(a.c_str(), b.c_str());
        return ascending ? (r <= 0)   // pre vzostupne: a <= b
                         : (r > 0);   // pre zostupne: a >  b
    };

    // iteratívne porovnávame fronty left a right
    while (!left.empty() && !right.empty()) {
        if (cmp(left.front(), right.front())) {
            // zoznam lst doplníme prvým prvkom z left
            lst.splice(lst.end(), left, left.begin());
        } else {
            lst.splice(lst.end(), right, right.begin());
        }
    }
    // zvyšné prvky (jedného zo zoznamov) presunieme na koniec
    if (!left.empty())
        lst.splice(lst.end(), left);
    if (!right.empty())
        lst.splice(lst.end(), right);
}

/** Pomocná funkcia na výpis obsahu list<string> */
void printList(const list<string> &lst) {
    cout << "[ ";
    for (auto it = lst.begin(); it != lst.end(); ++it) {
        cout << '"' << *it << "\" ";
    }
    cout << "]\n";
}

int main() {
    // Príklad 1: vzostupné zotriedenie
    list<string> l1 = { "Milan", "Martin", "Eva" };
    cout << "Povodny zoznam: ";
    printList(l1);
    mergeSort(l1, true);
    cout << "Zoradene vzostupne: ";
    printList(l1);
    // Očakávaný výstup: ["Eva", "Martin", "Milan"]

    // Príklad 2: vzostupne s prazdnym retazcom
    list<string> l2 = { "Fero", "Jano", "" };
    cout << "\nPovodny zoznam: ";
    printList(l2);
    mergeSort(l2, true);
    cout << "Zoradene vzostupne: ";
    printList(l2);
    // Očakávaný výstup: ["", "Fero", "Jano"]

    // Príklad 3: zostupne zotriedenie
    list<string> l3 = { "Apple", "Banana", "Cherry" };
    cout << "\nPovodny zoznam: ";
    printList(l3);
    mergeSort(l3, false);
    cout << "Zoradene zostupne: ";
    printList(l3);
    // Očakávaný výstup: ["Cherry", "Banana", "Apple"]

    return 0;
}
```

### Vysvetlenie stručne

1. **Rozdeľ–a–zlúč**
    - Zoznam sa rozdelí na dve polovice (`left` a `right`) pomocou operácie `splice`, ktorá má komplexnosť O(1) na každý
      presun uzla.
    - Potom sa rekurzívne zotriedia obe polovice.
    - Nakoniec sa obe zotriedené polovice zlúčia do pôvodného `lst` porovnávaním front prvkov.

2. **Porovnanie reťazcov**
    - Používame `std::strcmp`, ktorý vracia negatívne číslo, nulové alebo kladné číslo podľa lexikografického poradia.
    - Pomocou lambda-funkcie `cmp` sa rozhoduje, či jeden reťazec ide pred druhý pre vzostupné alebo zostupné zoradenie.

3. **In-place triedenie**
    - Všetky operácie nad zoznamom prebiehajú pomocou `splice`, takže sa nealokuje nová pamäť na uzly, iba sa mení ich
      prepojenie, čo zabezpečuje efektívny merge sort pre `std::list`.

-->

{{< /details >}}
